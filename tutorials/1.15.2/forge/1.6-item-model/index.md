---
layout: page
title: 1.6 - Item Model
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

Our item currently doesn't have a model. This is evidenced by its having the missing model when you look at it in-game and there being a `FileNotFoundException` error in your logs.  
Your logs will contain something very similar to
```
[minecraft/ModelBakery]: Unable to load model: 'examplemod:example_item#inventory' referenced from: examplemod:example_item#inventory: {}
[minecraft/ModelBakery]: java.io.FileNotFoundException: examplemod:models/item/example_item.json
```
This tells us that the game tried to find our model at `models/item/example_item.json` but couldn't find it.  
We are going to fix this and add a model and texture for our Item.  
First of all, make a file called "example_item.json" in `src/main/resources/assets/examplemod/models/item/`.
> **Resources**  
> A resource is data (images, audio, text, and so on) inside a program (this data isn't code though) that is used by that program. Resources are located in the assets directory (at `src/main/resources/assets/`). Mods contains resources including images, `obj` or `b3d` (advanced model) files and `json` files (like recipes, `json` models, blockstate json files and localisation files). [More](https://docs.oracle.com/javase/8/docs/technotes/guides/lang/resources.html#overview) [info](https://mcforge.readthedocs.io/en/latest/concepts/resources/)  

> **Item Models**
> Item Model files are a specific subtype of json file. [More info](https://minecraft.gamepedia.com/Model#Item_models)  

Inside the new `example_item.json` file paste the following text.
```json
{
	"parent": "item/generated",
	"textures": {
		"layer0": "examplemod:item/example_item"
	}
}
```
This text defines our example item's model.  
The `"parent"` of `"item/generated"` means that this model is an item and uses the normal flat model.  
The texture `"examplemod:item/example_item"` of `"layer0"` in the `"textures"` tag makes the model's texture be our example item's texture (which we haven't created yet).  
The texture `"layer0"` is defined in the model `"item/generated"` so it will be rendered. [More info](https://minecraft.gamepedia.com/Model#Simple_example:_2D_beds)  

Finally, download [this texture](./example_item.png) (or make your own) and put it at `src/main/resources/assets/examplemod/textures/item/` with the name "example_item.png"  
> **Important**: Texture files ***must*** be `.png` files

Your item should now have a model and a texture. If anything goes wrong, check your logs, they're usually very descriptive.


##### [1.7 - ItemGroup](../1.7-itemgroup)
