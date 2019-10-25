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

1. Create a class called "ModTileEntityTypes" in your `init` package (`mod.yourname.modpackagename.init`)
2. Annotate `ModTileEntityTypes` with `@ObjectHolder(ExampleMod.MODID)`
3. Add a constant `TileEntityType<?>` called "INFUSER" with a value of `null`.
Your class should look something like
```java
@ObjectHolder(ExampleMod.MODID)
public class ModTileEntityTypes {
	public static final TileEntityType<?> INFUSER = null;
}
```

4. Create a new package called "tileentity" (`mod.yourname.modpackagename.tileentity`)
5. Make a new class called "InfuserTileEntity" and make it extend `TileEntity` (`net.minecraft.tileentity.TileEntity`)
6. Make a constructor with no parameters that passes `ModTileEntityTypes.INFUSER` to `super`
7. Make a new class in your `block` package (`mod.yourname.modpackagename.block`) called "InfuserBlock" that extends `Block` (`net.minecraft.block.Block`)
8. Make a constructor "matching super" as reccomended by your IDE
9. Override `hasTileEntity(IBlockState)` to return `true` and `createTileEntity(IBlockState)` to return the result of `ModTileEntityTypes.INFUSER.create()`
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

10. Create and register the block in `ModEventSubscriber` in the `onRegisterBlocks` event subscriber method by creating a new `InfuserBlock` and passing it to `setup` inside the `registerAll` call. Your `InfuserBlock` should have properties with the `ROCK` `Material` and a `hardnessAndResistance` of `3.5F`. This makes the block very similar to the furnace
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

11. We now need a `static` reference to our infuser block. W 
12. Create a class called "ModBlock" in your `init` package (`mod.yourname.modpackagename.init`)  
13. Annotate the `ModBlock` with `@ObjectHolder` (`net.minecraftforge.registries.ObjectHolder`) and have the parameter of the annotation be `ExampleMod.MODID`  
> `@ObjectHolder`  
> When you put the `@ObjectHolder` annotation on a class, Forge will look at every field in the class and set the value of each field. The value of the field will be set to the object in the field type's registry that has a registry name made up of the parameter of the annotation and the field's name (in lowercase). For example a field with a type of `Block` and a name of `INFUSER` (`public static final Block INFUSER = null;`) in a class annotated with `@ObjectHolder(ExampleMod.MODID)` will be filled with the the `Block` whose registry name is `examplemod:infuser`. [Read more](https://mcforge.readthedocs.io/en/latest/concepts/registries/#injecting-registry-values-into-fields)  
14. In this class create a constant `Block` called `INFUSER` with a value of `null`. The value of the field will be changed from `null` to our infuser block once we register it  
Your class should look something like
```java
@ObjectHolder(ExampleMod.MODID)
public class ModBlocks {
	public static final Block INFUSER = null;
}
```

15. In `ModEventSubscriber` create a `public static void` method called `onRegisterTileEntityTypes` with a `RegistryEvent.Register<TileEntityType>` as its only parameter and annotate the method with `@SubscribeEvent`. 
Your method should look something like
```java
@SubscribeEvent
public static void onRegisterTileEntityTypes(RegistryEvent.Register<TileEntityType> event) {

}
```

16. Inside the `onRegisterTileEntityType` event subscriber method add a call to the event's registry's `registerAll` method with `event.getRegistry().registerAll();`. Inside the final brackets (The ones that invoke `registerAll`) make 2 new lines and create a new `TileEntityType` with `TileEntityType.Builder.create(InfuserTileEntity::new, ModBlocks.INFUSER).build(null)`. This simply creates a new `TileEntityType` with `ModBlocks.INFUSER` as its block that will create a new `InfuserTileEntity` and that has no data fixer. We also need to set the `TileEntityType`'s **registry name**. Do this by passing the new `TileEntityType` as the first parameter of a call to the `setup` method, and the `String` `"infuser"` as the second parameter.  
All this combined should look something like
```java
event.getRegistry().registerAll(
	setup(TileEntityType.Builder.create(InfuserTileEntity::new, ModBlocks.INFUSER).build(null), "infuser")
);
```
