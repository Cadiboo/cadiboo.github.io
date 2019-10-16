---
layout: page
title: 1.4 - First Item
---
This tutorial assumes you have already
- Read the [Pre-requisites](https://cadiboo.github.io/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.14.3 tutorials page](/tutorials/1.14.3/forge/)
- Read and followed [1.0 - Gradle Configuration](https://cadiboo.github.io/tutorials/1.14.3/forge/1.0-gradle-configuration/)
- Read and followed [1.1 - Importing the project into your IDE](https://cadiboo.github.io/tutorials/1.14.3/forge/1.1-importing-project/)
- Read and followed [1.2 - Basic Mod](https://cadiboo.github.io/tutorials/1.14.3/forge/1.2-basic-mod/)
- Read and followed [1.3 - Doing Something](https://cadiboo.github.io/tutorials/1.14.3/forge/1.3-doing-something/)

# This tutorial isn't finished yet. However you can always look at the code in [my ExampleMod](https://github.com/Cadiboo/Example-Mod/), where I write and perfect the stuff I teach in these tutorials. Item registration and creation is [here](https://github.com/Cadiboo/Example-Mod/blob/1d848d74c94786bc7f5d3757d2dfc5cb248dba5f/src/main/java/io/github/cadiboo/examplemod/ModEventSubscriber.java#L76-L78)

You're finally ready to add your first item to the game!

![Why isn't my Event Subscriber Working](/tutorials/1.14.3/forge/1.4-first-item/eventsubscriber.png "Why isn't my Event Subscriber Working")

1) Make a new class called "ModEventSubscriber" in the same package as your main mod class (`mod.yourname.modpackagename`)
2) >Annotate it with >`@EventBusSubscriber` (and import `EventBusSubscriber` from `net.minecraftforge.fml.common.Mod`)  
3) add the parameter >"modid = MainModClass.MODID"  
4) add the parameter >"bus = MOD" (`import static net.minecraftforge.fml.common.Mod.EventBusSubscriber.Bus.MOD`)  

5) final class should look like
```java

import static net.minecraftforge.fml.common.Mod.EventBusSubscriber.Bus.MOD;

@EventBusSubscriber(modid = Purrgatory.MODID, bus = MOD)
public final class ModEventSubscriber {

}
```
6?) That subscribes your class to mod >events like registry events and loading events, Forge events (such as render hooks and world hooks) are fired on a different bus.
> Registry (explain)
7) This is the first of only four places in this tutorial in which I will tell you to blindly copy paste code without understanding how it works. I don’t expect you to understand how the code in these places works because it deals with advanced java concepts like <generics> and works through very advanced code on Forge’s part that uses <ASM> and <Reflection>. The following code is a very elegant solution to the problem of having very messy and fragile registration code. It uses <generics> and <method overloading> to create a method called “setup” that we will call to ***correctly*** setup up all our registry entries (items/blocks/entities). This method is important because there are many (only slightly) wrong ways of setting up registry entries that completely break the mod that uses them (and can also easily break other mods).
> Bad and improper registration code is the leading cause of bad and/or broken mods. These bad/broken mods can also break other mods that are doing everything right *and* improper registration stops Forge from being able to deliver useful features. These features that could be implemented by Forge include dynamically reloading the registries (adding/removing items while the game is running) and even adding/removing entire mods without restarting the game. Forge has wanted to enable these features for more than 4 years now, but mods that do their registration in the wrong way has been stopping this from happening.

For Forge to be able to do this (and for registry replacements to work properly) Mods need to instantiate and register their objects in the correct registry event. They also need to use references to their objects that can change as mods are loaded & unloaded (and registry replacements are activated/deactivated). Mods can do this by using the @ObjectHolder annotation (which automatically fills fields with the appropriate values after each registry event fires) or manually by setting their fields after all other registration has been done (by subscribing to the registry event and running after everything else). Mods that create and/or register their objects with static initialisers stop Forge from being able to do cool stuff AND they can break unexpectedly and uncontrollably if/when their registration classes are loaded before the expected time (you have no control over when your classes are loaded). Static initialisers are initialising objects inside a “static {}” block or doing “static Object object = new Object();” in a class.  

The 2 setup methods that we create will work perfectly for all our items/blocks/entities. These two methods *correctly* set the proper registry name for our entries. Add these two methods to the bottom of your ModEventSubscriber class.
```java
	public static <T extends IForgeRegistryEntry<T>> T setup(final T entry, final String name) {
		return setup(entry, new ResourceLocation(ExampleMod.MODID, name));
	}

	public static <T extends IForgeRegistryEntry<T>> T setup(final T entry, final ResourceLocation registryName) {
		entry.setRegistryName(registryName);
		return entry;
	}
```