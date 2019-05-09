---
layout: page
title: 1.2 - Basic mod
---
This tutorial assumes you have already
- Read the [Pre-requisites](https://cadiboo.github.io/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.13.2 tutorials page](/tutorials/1.13.2/forge/)
- Read and followed [1.0 - Gradle Configuration](https://cadiboo.github.io/tutorials/1.13.2/forge/1.0-gradle-configuration/)
- Read and followed [1.1 - Importing the project into your IDE](https://cadiboo.github.io/tutorials/1.13.2/forge/1.1-importing-project/)

This tutorial will help you set up your main mod class.  
> Your mod's Main mod class is the class that is loaded by Forge and sets up the data structures for your mod. In current versions this class doesn't really have much functionality except for making your mod a mod, but in previous versions, especially older ones, this class handled almost everything related to Forge and being a mod.

![java](/tutorials/1.13.2/forge/1.2-basic-mod/java.png "java") 
Java Concepts (Click each concept to view the official primers by the makers of Java):  
- Objects  
> Objects are things. Real world examples of Objects are your dog, your desk, your computer, your bed and your bicycle.  
> Real-world objects share two characteristics: They all have state and behavior. Dogs have state (name, color, breed, hungry) and behavior (barking, fetching, wagging tail). Bicycles also have state (current gear, current pedal cadence, current speed) and behavior (changing gear, changing pedal cadence, applying brakes).
> Your mod is an Object like everything else in Java and it has state (modid, author, logo) and behaviour (what it does when the game is lauched, what it does when other mods are loaded, what it does when Minecraft is fully loaded)

- [Classes](https://docs.oracle.com/javase/tutorial/java/concepts/class.html)    
> In the real world, you'll often find many individual objects all of the same kind. There may be thousands of other bicycles in existence, all of the same make and model. Each bicycle was built from the same set of blueprints and therefore contains the same components.
> Your mod is no different. Your main mod class is the blueprint for your mod, which will get loaded by Forge.

- [Packages](https://docs.oracle.com/javase/tutorial/java/concepts/package.html)
> A package is a namespace that organizes a set of related classes and interfaces. Conceptually you can think of packages as being similar to different folders on your computer. You might keep HTML pages in one folder, images in another, and scripts or applications in yet another. Because software written in the Java programming language can be composed of hundreds or thousands of individual classes, it makes sense to keep things organized by placing related classes and interfaces into packages.
> You're going to want to keep stuff seperate in your mod too. You're going to want to keep Entities separate from Blocks and Item separate from Sounds for example. You're also going to want to keep client side code separate from server side code.

- [Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/)
> Annotations provide metadata about parts of a program without being part of the program itself. The `@Mod` annotation contains metadata (your modid). Forge sees this annotation on your main mod class and knows to try to find a `mods.toml` file for your mod. If it finds this file it will load your mod and create an instance of it when it is time to load mods.


The first step to making your main mod class doesn't involve your mod at all. The first step is to disable the example mod that comes with the Forge MDK. If you don't disable this example mod, Forge will try and load both your mod and the example mod

> The packaging for your mod is the same as the Mod Group. **It can consist of only *lowercase* alphanumeric letters (`a-z` and `0-9`).** It is in the format `yourwebsitereversed.modid`. Websites are used in packaging to avoid name collisions. Your web presence is unique so it is commonly used. If you don’t have a website, you can use your version control repository as your web presence, e.g. `com.github.username.modid`. If you don’t use version control, start using it! In the meantime you can use the packaging `mod.yournameinlowercase.modid`.  
**The entirety of your Mod Group needs to be in lowercase!** This is because some file systems (windows) consider `iTeMs` and `items` to be the same, but every other system considers them to be different. *Trying to load non-lowercase files on different filesystems can cause massive problems and hard-to-track-down bugs!*  
Examples of packaging for your mod:  
> 1) `io.github.cadiboo.nocubes` (the website is "cadiboo.github.io" and the modid is "nocubes")  
> 2) `io.github.krevik.kathairis` (the website is "krevik.github.io" and the modid is "kathairis")  
> 3) `com.github.coolalias.coolmod` (the website is "github.com/coolalias" and the modid is "coolmod")  
> 4) `mod.coolalias.coolmod` (there is no website, the author is "coolalias" and the modid is "coolmod")


1) >Comment out the `@Mod` >annotation on the example mod >class
2) Open up >`mods.toml` and change the
- >modid
- mod name
- description
- author
- dependencies
- (may also need to change the order of the entries? Might only be necessary on older forge versions)
3) new >package `mod.yourname.modid` (**needs to be lowercase**)
4) new >class called Main/ModName (explain naming conventions)
5) >constant `MOD_ID` (remind lowercase and explain what it’s used for and where) (```java
public static final String MOD_ID = “purrgatory”;```)
6) `@Mod(MOD_ID)` on mod class (explain static imports and what the @Mod annotation does) (also explain imports and which class to import)
7) Run/debug minecraft -> 2 mods loaded, Your mod in the mod list