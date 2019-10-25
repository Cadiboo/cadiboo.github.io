---
layout: page
title: 1.5 - First Item
---
This tutorial assumes you have already
- Read the [Pre-requisites](/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.14.4 tutorials page](/tutorials/1.14.4/forge/)
- Read and followed [1.0 - Gradle Configuration](../1.0-gradle-configuration/)
- Read and followed [1.1 - Importing the project into your IDE](../1.1-importing-project/)
- Read and followed [1.2 - Basic Mod](../1.2-basic-mod/)
- Read and followed [1.3 - Doing Something](../1.3-doing-something/)
- Read and followed [1.4 - Proxies](../1.4-proxies/)

You're finally ready to add your first item to the game!

Firstly, make a new class called "ModEventSubscriber" in the same package as your main mod class (`mod.yourname.modpackagename`)  
Now annotate it with `@EventBusSubscriber` (and import `EventBusSubscriber` from `net.minecraftforge.fml.common.Mod`)  
> The `@EventBusSubscriber` annotation indicates to Forge that this class contains methods that should be subscribed to handle events. It contains the `modid`, `bus` and `value` parameters.  

Add the parameter `modid = MainModClass.MODID`. This parameter tells Forge that this class belongs to your mod. This is important because `EventBusSubscriber` scanning is done before mods are fully loaded and Forge doesn't automatically know what mod the class it is scanning belongs to.  
Next, add the parameter `bus = EventBusSubscriber.Bus.MOD`. This parameter tells Forge that `@SubscribeEvent` methods in this class should receive events from the `MOD` event bus.  
> Events and Event Subscribing ([More info](https://mcforge.readthedocs.io/en/1.13.x/events/intro/))  
> - An event listener or *event subscriber* is a function in a computer program that waits for an event to occur. The listener is programmed to react to the event it listens for and execute logic based on the data of that event. Key press handlers or mouse click handlers are examples of event listeners. Mods *subscribe* methods to events that Forge posts to run their own logic and influence the game. These events are run on Event Buses.  
> - Mod-specific events like Registry events and Mod Loading events are fired on the `MOD` event bus.  
> - Non-mod-specific events such as Tick events, Rendering events, World events and Player events are fired on the `FORGE` event bus.  
> - Only events (i.e. objects whose class extends `net.minecraftforge.eventbus.api.Event`) can be listened for/subscribed to on the Event Buses.  

Your final class should look something like
```java
package io.github.cadiboo.examplemod;

import net.minecraftforge.fml.common.Mod.EventBusSubscriber;

@EventBusSubscriber(modid = ExampleMod.MODID, bus = EventBusSubscriber.Bus.MOD)
public final class ModEventSubscriber {

}
```
This code structure subscribes your class to mod specific events (like Registry events and loading events).  
> Registries. 
> A Registry is a data structure that maps keys (in this case **registry names**) to values. It is basically a glorified `Map`. The Item registry maps the name of your items to the actual `Item` instance. To let the game know about your `Item`s you need to set your `Item`'s registry name and add it to the registry. This also applies to `Blocks`, `TileEntity`s, `EntityEntry`s, `Dimension`s, and anything else that implements `IForgeRegistryEntry`. [More info](https://mcforge.readthedocs.io/en/latest/concepts/registries/)  

Now create a `public static void` method called `onRegisterItems` with a `RegistryEvent.Register<Item>` as its only parameter and annotate the method with `@SubscribeEvent`. You will need to import `net.minecraftforge.event.RegistryEvent` and `net.minecraftforge.eventbus.api.SubscribeEvent`. The `@SubscribeEvent` annotation tells Forge that this method wants to subscribe to/listen for an event and the single `RegistryEvent.Register<Item>` parameter tells Forge that you want this method to be called when it is time for your mod to register its items. [More info](https://mcforge.readthedocs.io/en/1.13.x/concepts/registries/#registering-things)  
Your method should look something like
```java
@SubscribeEvent
public static void onRegisterItems(RegistryEvent.Register<Item> event) {

}
```
This is the first of only four places in this tutorial in which I will tell you to blindly copy paste code without understanding how it works. I don’t expect you to understand how the code in these places works because it deals with advanced java concepts like **generics** and works through very advanced code on Forge’s part that uses **ASM** and **Reflection**. The following code is a very elegant solution to the problem of having very messy and fragile registration code. It uses **generics** and **method overloading** to create a method called `setup` that we will call to ***correctly*** setup up all our registry entries (`Item`s/`Block`s/`TileEntity`s/`EntityEntry`s/`Dimension`s). This method is important because there are many (only slightly) wrong ways of setting up registry entries that completely break the mod that uses them (and that can also easily break other mods).  
> Proper Registration  
> Bad and improper registration code is the leading cause of bad and/or broken mods. These bad/broken mods can also break other mods that are doing everything right *and* improper registration stops Forge from being able to deliver useful features. These features that could be implemented by Forge include dynamically reloading the registries (adding/removing items while the game is running) and even adding/removing entire mods without restarting the game. Forge has wanted to enable these features for more than 4 years now, but mods that do their registration in the wrong way has been stopping this from happening. For Forge to be able to do this (and for registry replacements to work properly) Mods need to instantiate and register their objects in the correct registry event. They also need to use references to their objects that can change as mods are loaded & unloaded (and registry replacements are activated/deactivated). Mods can do this by using the `@ObjectHolder` annotation (which automatically fills fields with the appropriate values after each registry event fires) or manually by setting their fields after all other registration has been done (by subscribing to the registry event and running after everything else). Mods that create and/or register their objects with static initialisers stop Forge from being able to do cool stuff AND they can break unexpectedly and uncontrollably if/when their registration classes are loaded before the expected time (you have no control over when your classes are loaded). Static initialisers are initialising objects inside a `static {}` block or doing `static Object object = new Object();` in a class.  

The 2 setup methods that we create will work perfectly for all our `Item`s/`Blocks`/`TileEntity`s/`EntityEntry`s/`Dimension`s. These two methods *correctly* set the proper registry name for our entries. Add these two methods to the bottom of your ModEventSubscriber class.
```java
public static <T extends IForgeRegistryEntry<T>> T setup(final T entry, final String name) {
	return setup(entry, new ResourceLocation(ExampleMod.MODID, name));
}

public static <T extends IForgeRegistryEntry<T>> T setup(final T entry, final ResourceLocation registryName) {
	entry.setRegistryName(registryName);
	return entry;
}
```
Now, we are going to finally create and register our `Item`! Inside your `onRegisterItems` event subscriber method add a call to the event's registry's `registerAll` method with `event.getRegistry().registerAll();`. Inside the final brackets (The ones that invoke `registerAll`) make 2 new lines and create a new `Item` with `new Item(new Item.Properties())`. This simply creates a new `Item`, but we need to also set its **registry name**. Do this by passing the new `Item` as the first parameter of a call to the `setup` method, and the `String` `"example_item"` as the second parameter.  
All this combined should look something like
```java
event.getRegistry().registerAll(
	setup(new Item(new Item.Properties()), "example_item")
);
```
To finish off, run the game and you should be able to get your item with `/give`. **Your item won't have a model or do anything yet though.**  

If you run into issues please make sure you look at the following image before asking for help. (Make sure you've annotated your method with `@SubscribeEvent`, that the method is a `public static void` and that the method has a single parameter (The class of the parameter must extend `net.minecraftforge.eventbus.api.Event`))  
![Why isn't my Event Subscriber Working](./eventsubscriber.png "Why isn't my Event Subscriber Working")


##### [1.6 - Item Model](../1.6-item-model)
