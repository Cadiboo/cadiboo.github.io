---
layout: page
title: 1.7 - ItemGroup
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
- Read and followed [1.5 - First Item](../1.5-first-item/)
- Read and followed [1.6 - Item Model](../1.6-item-model/)

Now we're going to add an `ItemGroup` (previously called a `CreativeTab`) to our mod to contain our mod's items in the creative menu.

1) Make a new package called `init` (`mod.yourname.modpackagename.init`)  
2) Make a new class called "ModItemGroups" in that package  
3) Create an `public static` inner class called "ModItemGroup" that extends `ItemGroup` (from `net.minecraft.item.ItemGroup`) in `ModItemGroups`.  
> Inner classes are classes which are declared *inside* a class or interface. Inner classes are used to implement encapsulation and logically group classes & interfaces together in one location to make them more readable and maintainable. [Read](https://www.javatpoint.com/java-inner-class) [more](https://www.tutorialspoint.com/java/java_innerclasses.htm)  

4) Create a `public` constructor in your `ModItemGroup` class with two parameters, a `String` called "name" and a `Supplier<ItemStack>` called `iconSupplier`.  
> `Supplier`s are interfaces that represent a function which does not take in any arguments but produces an object. Suppliers are a key feature of [functional programming](https://www.geeksforgeeks.org/functional-programming-paradigm/). [Read more](https://www.geeksforgeeks.org/supplier-interface-in-java-with-examples/)  
> *Lambdas* are expressions that implement a Functional Interface. They are characterized by use of the arrow operator (`->`) and the syntax `parameter(s) -> expression body`. Lambda expressions facilitate functional programming.  
> A *Functional Interface* is an interface that contains exactly one abstract method. A `Supplier` meets this definition and is therefor a functional interface.  
> The lambda expression assigned to an object of `Supplier` type is used to define its `get()` method which produces a value when called. `Supplier`s are useful for deferring the creation of objects.  
> An example of a `Supplier` would be `() -> new Object()` or `() -> "Hello"` or `() -> new Thing()`.  

5) Create a `final` field in your `ModItemGroup` class of type `Supplier<ItemStack>` called `iconSupplier`.  
6) Override the `createIcon()` method and return the result of `iconSupplier.get()`  
Your inner class should look like
```java
public static final class ModItemGroup extends ItemGroup {

	private final Supplier<ItemStack> iconSupplier;

	public ModItemGroup(final String name, final Supplier<ItemStack> iconSupplier) {
		super(name);
		this.iconSupplier = iconSupplier;
	}

	@Override
	public ItemStack createIcon() {
		return iconSupplier.get();
	}

}
```
7) Create a constant `ItemGroup` called `MOD_ITEM_GROUP` in `ModItemGroups`.  
8) Initialise this field to a new `ModItemGroup` with `ExampleMod.MODID` as the name and `() -> new ItemStack(Items.LIGHT_BLUE_BANNER)` as the iconSupplier (`Items` from `net.minecraft.item.Items`).  
9) Now add all our `Item`s and `BlockItem`s to our new tab by calling `group(ModItemGroups.MOD_ITEM_GROUP)` on every `Item.Properties` that we pass into our `Item` constructors. For example, `new Item(new Item.Properties())` will become `new Item(new Item.Properties().group(ModItemGroups.MOD_ITEM_GROUP))`.  
10) Now our `ItemGroup` works perfectly, but it has a vanilla banner as its icon. To change this we need to change `Items.LIGHT_BLUE_BANNER` to reference our own item. However, we don't have a static reference to our own item, so we need to make one.  
11) Create a class called "ModItems" in your `init` package (`mod.yourname.modpackagename.init`)  
12) Annotate the `ModItems` with `@ObjectHolder` (`net.minecraftforge.registries.ObjectHolder`) and have the parameter of the annotation be `ExampleMod.MODID`  
> When you put the `@ObjectHolder` annotation on a class, Forge will look at every field in the class and set the value of each field. The value of the field will be set to the object in the field type's registry that has a registry name made up of the parameter of the annotation and the field's name (in lowercase). For example a field with a type of `Item` and a name of `EXAMPLE_ITEM` (`public static final Item EXAMPLE_ITEM = null;`) in a class annotated with `@ObjectHolder(ExampleMod.MODID)` will be filled with the the `Item` whose registry name is `examplemod:example_item`. [Read more](https://mcforge.readthedocs.io/en/latest/concepts/registries/#injecting-registry-values-into-fields)  

13) In this class create a constant `Item` called `EXAMPLE_ITEM` with a value of `null`. The value of the field will be changed from `null` to our example item once we register it  
Your class should look like
```java
@ObjectHolder(ExampleMod.MODID)
public class ModItems {
	public static final Item EXAMPLE_ITEM = null;
}
```
15) Change the result of the icon supplier in `ModItems` from `Items.LIGHT_BLUE_BANNER` to `ModItems.EXAMPLE_ITEM`  
