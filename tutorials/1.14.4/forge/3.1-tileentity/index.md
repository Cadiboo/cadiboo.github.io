---
layout: page
title: 3.1 - TileEntity
---
This tutorial assumes you have already
- Read the [Pre-requisites](/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.14.4 tutorials page](/tutorials/1.14.4/forge/)
- Read and followed all of Chapter 1
- Read and followed all of Chapter 2
- Read and followed [3.0 - Block with BlockStates](../3.0-block-with-blockstates/)

Making an Infuser `TileEntity`

Before we start on our `TileEntity`, we need a place to store our `TileEntityType`.  
> `TileEntityType`s  
> Unlike most objects that require a registry, `TileEntity`s are not [singeltons](https://en.wikipedia.org/wiki/Singleton_pattern). Previously this was gotten around by using class-based registration which was not optimal. Currently `TileEntityType`s are used for the registration of `TileEntity`s. A `TileEntityType` contains some data about a `TileEntity`, such as it's registry name, the blocks the `TileEntity` exists for, the (optional) datafixer for the `TileEntity`'s data and a [factory](http://en.wikipedia.org/wiki/Factory_method_pattern) that creates new instances of the `TileEntity`.

Create a class called "ModTileEntityTypes" in your `init` package (`mod.yourname.modpackagename.init`). Annotate this class with `@ObjectHolder` (`net.minecraftforge.registries.ObjectHolder`) and have the parameter of the annotation be `ExampleMod.MODID`.
> `@ObjectHolder`  
> When you put the `@ObjectHolder` annotation on a class, Forge will look at every field in the class and set the value of each field. The value of the field will be set to the object in the field type's registry that has a registry name made up of the parameter of the annotation and the field's name (in lowercase).  
> For example a field with a type of `Block` and a name of `INFUSER` (`public static final Block INFUSER = null;`) in a class annotated with `@ObjectHolder(ExampleMod.MODID)` will be filled with the the `Block` whose registry name is `examplemod:infuser`. [Read more](https://mcforge.readthedocs.io/en/latest/concepts/registries/#injecting-registry-values-into-fields)

Then add a constant `TileEntityType<?>` called "INFUSER" with a value of `null`.  
Your class should look something like
```java
@ObjectHolder(ExampleMod.MODID)
public class ModTileEntityTypes {
	public static final TileEntityType<?> INFUSER = null;
}
```

Now we're going to make our actual `TileEntity` class.  
Create a new package called "tileentity" (`mod.yourname.modpackagename.tileentity`) and make a new class in it called "InfuserTileEntity". Make this class extend `TileEntity` (`net.minecraft.tileentity.TileEntity`) and create constructor with no parameters that passes `ModTileEntityTypes.INFUSER` to `super`. 
Next, make a new class in your `block` package (`mod.yourname.modpackagename.block`) called "InfuserBlock". This class should extend `Block` (`net.minecraft.block.Block`). Use your IDE to create a constructor "matching super" (i.e. with the same parameters as the one in the `Block` class).  
To actually make this block have a `TileEntity` we need to override `hasTileEntity(IBlockState)` to return `true` and `createTileEntity(IBlockState)` to return the result of `ModTileEntityTypes.INFUSER.create()`.  
Your class should look something like
```java
public class InfuserBlock extends Block {

	public InfuserBlock(final Properties properties) {
		super(properties);
	}

	@Override
	public boolean hasTileEntity(final BlockState state) {
		return true;
	}

	@Override
	public TileEntity createTileEntity(final BlockState state, final IBlockReader world) {
		return ModTileEntityTypes.INFUSER.create();
	}

}
```

Next we need to create an instance of our `InfuserBlock` and register it. Go to the `onRegisterBlocks` event subscriber method inside `ModEventSubscriber`. Inside the inside the `registerAll` call of the method make a call to our `setup` method with a new `InfuserBlock` as the first parameter and `"infuser"` as the second parameter. The new `InfuserBlock` should be constructed with a `Properties` with the `ROCK` `Material` and a `hardnessAndResistance` of `3.5F`. This makes the block very similar to the Furnace.  
Your `onRegisterBlocks` method should now look something like
```java
@SubscribeEvent
public static void onRegisterBlocks(RegistryEvent.Register<Block> event) {
	event.getRegistry().registerAll(
		setup(new Block(Block.Properties.create(Material.ROCK).hardnessAndResistance(3.0F, 3.0F)), "example_ore"),
		setup(new InfuserBlock(Block.Properties.create(Material.ROCK).hardnessAndResistance(3.5F)), "infuser")
	);
}
```

Next we need to register our `TileEntityType`. However, to do this we need a `static` reference to our infuser block. We need this reference so that we can add our `InfuserBlock` as being a valid block for our `TileEntity`.  
To do this, create a class called "ModBlocks" in your `init` package (`mod.yourname.modpackagename.init`). Annotate this class with `@ObjectHolder` (`net.minecraftforge.registries.ObjectHolder`) and have the parameter of the annotation be `ExampleMod.MODID`. In this class create a constant `Block` called `INFUSER` with a value of `null`. The value of the field will be changed from `null` to our infuser block once we register it  
Your class should look something like
```java
@ObjectHolder(ExampleMod.MODID)
public class ModBlocks {
	public static final Block INFUSER = null;
}
```

Head over to `ModEventSubscriber` and create a `public static void` method called `onRegisterTileEntityTypes` with a `RegistryEvent.Register<TileEntityType>` as its only parameter and annotate the method with `@SubscribeEvent`.  
Your method should look something like
```java
@SubscribeEvent
public static void onRegisterTileEntityTypes(RegistryEvent.Register<TileEntityType> event) {

}
```

Inside the `onRegisterTileEntityTypes` event subscriber method add a call to the event's registry's `registerAll` method with `event.getRegistry().registerAll();`. Inside the final brackets (The ones that invoke `registerAll`) make 2 new lines and create a new `TileEntityType` with `TileEntityType.Builder.create(InfuserTileEntity::new, ModBlocks.INFUSER).build(null)`. This simply creates a new `TileEntityType` with `ModBlocks.INFUSER` as its block that will create a new `InfuserTileEntity` and that has no data fixer. We also need to set the `TileEntityType`'s **registry name**. Do this by passing the new `TileEntityType` as the first parameter of a call to the `setup` method, and the `String` `"infuser"` as the second parameter.  
All this combined should look something like
```java
event.getRegistry().registerAll(
	setup(TileEntityType.Builder.create(InfuserTileEntity::new, ModBlocks.INFUSER).build(null), "infuser")
);
```
