---
layout: page
title: 1.7 - ItemGroup
---
This tutorial assumes you have already
- Read the [Pre-requisites](/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.15.2 tutorials page](/tutorials/1.15.2/forge/)
- Read and followed [1.0 - Gradle Configuration](../1.0-gradle-configuration/)
- Read and followed [1.1 - Importing the project into your IDE](../1.1-importing-project/)
- Read and followed [1.2 - Basic Mod](../1.2-basic-mod/)
- Read and followed [1.3 - Doing Something](../1.3-doing-something/)
- Read and followed [1.4 - Proxies](../1.4-proxies/)
- Read and followed [1.5 - First Item](../1.5-first-item/)
- Read and followed [1.6 - Item Model](../1.6-item-model/)

Now we're going to add an `ItemGroup` (previously called a `CreativeTab`) to our mod to contain our mod's items in the creative menu.

### Setup & Util code
First of all, make a new package called `init` (`mod.yourname.modpackagename.init`). Then make a new class called "ModItemGroups" in that package. Next, create an `public static` inner class called "ModItemGroup" that extends `ItemGroup` (from `net.minecraft.item.ItemGroup`) in `ModItemGroups`.  
> **Inner classes**  
> Inner classes are classes which are declared *inside* a class or interface. Inner classes are used to implement encapsulation and logically group classes & interfaces together in one location to make them more readable and maintainable. [Read](https://www.javatpoint.com/java-inner-class) [more](https://www.tutorialspoint.com/java/java_innerclasses.htm)  

To allow us to write less code, we're going to pass our icon into the constructor instead of making a bunch of anonymous classes (the way vanilla does it). We're going to do this by using a `Supplier<ItemStack>`
> **Suppliers**  
> `Supplier`s are interfaces that represent a function which does not take in any arguments but produces an object. Suppliers are a key feature of [functional programming](https://www.geeksforgeeks.org/functional-programming-paradigm/). [Read more](https://www.geeksforgeeks.org/supplier-interface-in-java-with-examples/)  

> **Lambdas**  
> *Lambdas* are expressions that implement a Functional Interface. They are characterized by use of the arrow operator (`->`) and the syntax `parameter(s) -> expression body`. Lambda expressions facilitate functional programming.  

> **Functional Interfaces**
> A *Functional Interface* is an interface that contains exactly one abstract method. A `Supplier` meets this definition and is therefor a functional interface.  

> The lambda expression assigned to an object of `Supplier` type is used to define its `get()` method which produces a value when called. `Supplier`s are useful for deferring the creation of objects.  
> An example of a `Supplier` would be `() -> new Object()` or `() -> "Hello"` or `() -> new Thing()`.  

In our case we use a `Supplier` because need to delay the creation of the `ItemGroup`'s icon `ItemStack`. We need to delay it because `ItemGroup`s are created before any `Item`s are registered. The icon is not needed before the first time the ItemGroup is rendered (by which time `Item`s will have been registered). If we try to make a new `ItemStack` with an `Item` when we create our `ItemGroup`, we will get an error because all `Item`s are null at this time and trying to use them will cause a `NullPointerException`. Using a `Supplier` delays creation of the icon `ItemStack` until its needed.  

Now, create a `public` constructor for your `ModItemGroup` class with two parameters, a `String` called "name" and a `Supplier<ItemStack>` called `iconSupplier`. Add a `final` to the class of type `Supplier<ItemStack>` called `iconSupplier` and assign the constructor parameter to it. To actually use the icon, override the `createIcon()` method and return the result of `iconSupplier.get()`  
Your inner class should look like
```java
public static class ModItemGroup extends ItemGroup {

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

### Creating the ItemGroup
Now that we've made our helper class, we can make our actual `ItemGroup`. To do this, create a constant `ItemGroup` called `MOD_ITEM_GROUP` in `ModItemGroups` and initialise this field to a new `ModItemGroup` with `ExampleMod.MODID` as the name and `() -> new ItemStack(Items.LIGHT_BLUE_BANNER)` as the iconSupplier (import `Items` from `net.minecraft.item.Items`).  
This sets up your `ItemGroup` with the vanilla light blue banner as it's icon. You use your own item by replacing `Items.LIGHT_BLUE_BANNER `with a reference to your own item (If you don't have one yet, read on).  
The declaration of `MOD_ITEM_GROUP` should look something like
```java
public static final ItemGroup MOD_ITEM_GROUP = new ModItemGroup(ExampleMod.MODID, () -> new ItemStack(Items.LIGHT_BLUE_BANNER));
```

### Using our ItemGroup
Now we're going to add all our `Item`s and `BlockItem`s to our new tab. We do this by calling `group(ModItemGroups.MOD_ITEM_GROUP)` on every `Item.Properties` that we pass into our `Item` constructors. For example, `new Item(new Item.Properties())` will become `new Item(new Item.Properties().group(ModItemGroups.MOD_ITEM_GROUP))`. We need to do this for our `example_item` that we register in `ModEventSubscriber` inside the `onRegisterItems` event subscriber method.  
![item-properties-group](./item-properties-group.png "item-properties-group")

### Using our own example item as our icon
Now our `ItemGroup` works perfectly, but it has a vanilla banner as its icon. To change this we need to change `Items.LIGHT_BLUE_BANNER` to reference our own item. However, we don't have a static reference to our own item so we need to make one.  
To do this, create a new class called "ModItems" in your `init` package (`mod.yourname.modpackagename.init`). Then annotate the class with `@ObjectHolder` (`net.minecraftforge.registries.ObjectHolder`) and have the parameter of the annotation be `ExampleMod.MODID`  
> **`@ObjectHolder`**  
> When you put the `@ObjectHolder` annotation on a class, Forge will look at every field in the class and set the value of each field. The value of the field will be set to the object in the field type's registry that has a registry name made up of the parameter of the annotation and the field's name (in lowercase).  
> For example a field with a type of `Item` and a name of `EXAMPLE_ITEM` (`public static final Item EXAMPLE_ITEM = null;`) in a class annotated with `@ObjectHolder(ExampleMod.MODID)` will be filled with the the `Item` whose registry name is `examplemod:example_item`. [Read more](https://mcforge.readthedocs.io/en/latest/concepts/registries/#injecting-registry-values-into-fields)

Next create a constant `Item` called `EXAMPLE_ITEM` with a value of `null`. The value of the field will be changed from `null` to our example item once we register it  
The class should now look something like
```java
@ObjectHolder(ExampleMod.MODID)
public class ModItems {
	public static final Item EXAMPLE_ITEM = null;
}
```
Finally, change the result of the icon supplier in `ModItems` from `Items.LIGHT_BLUE_BANNER` to `ModItems.EXAMPLE_ITEM` to change our `ItemGroup`'s icon to our own example item.


##### [1.8 - Localisation](../1.8-localisation)
