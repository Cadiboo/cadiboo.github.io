---
layout: page
title: 1.0 - Gradle Configuration
---
This tutorial assumes you have
- Read the [Pre-requisites](https://cadiboo.github.io/tutorials/Pre-requisites) already
- Downloaded the latest Forge MDK  

Gradle is an extremely powerful system for building Java applications. Forge uses gradle for modding because of the many things it can do with it, such as perform the deobfuscation and other setup tasks required to make a developer workspace.  

To start modding you will need to open up the `build.gradle` file in the root folder of your project and modify it a bit.  
![build.gradle](/tutorials/1.13.2/forge/1.0-gradle-configuration/build-gradle.png "build.gradle")  

First of all you need to change the version of your mod from `version = '1.0'` to `version = '1.13.2-0.1.0'`. Versioning in modding is in the format `MinecraftVersion-ModMajorVersion.ModMinorVersion.ModPatchVersion`. You can read more about versioning at [SemVer](https://semver.org).
!!! information
	The basics of SemVer versioning are:
	1) Increment the `ModMajorVersion` whenever you make incompatible changes in your mod (like removing items/blocks). Never reset it.  
	2) Increment the `ModMinorVersion` whenever you make backwards-compatible changes in your mod (like adding items/blocks). Reset it when you increment your `ModMajorVersion`.  
	3) Increment the `ModPatchVersion` whenever you make small backwards-compatible bug fix changes in your mod (like fixing an incorrect texture on an item/block). Reset it when you increment your `ModMajorVersion` or your `ModMinorVersion`.  
	4) Your initial development starts at version `0.1.0` and your first public release is at version `1.0.0`.  
	5) **NEVER, *NEVER* release two different versions of your mod with the same version number.**  
	6) You can add extra data to the end of your version, like `-pre3` or `-alpha2`.  
![Version](/tutorials/1.13.2/forge/1.0-gradle-configuration/version.png "Version")  

Secondly replace `modid` with your mod's actual Mod Id. 
!!! information
	A Mod Id is a unique identifier for your mod. It must be between 8 and 64 characters and be made up of only **lowercase** alphanumeric letters (`a-z` and `0-9`), dashes (`-`) and underscores (`_`).
You also need to replace `com.yourname.modid` with `mod.yourname.modid` where "yourname" is your name and "modid" is your mod's actual Mod Id. **"yourname" needs to be lowercase!**
![modid](/tutorials/1.13.2/forge/1.0-gradle-configuration/modid.png "modid")  

Thirdly you should update your MCP mappings to the latest stable release. **This step is optional**, however it is recommended to keep your mappings as updated as possible. MCP (Mod Coder Pack) Mappings are what Forge uses to deobfuscate minecraft's code and turn it into something human-readable. These names are provided by the community and can change, so its relatively important to keep them up to date. You can find a list of mappings [here](http://export.mcpbot.bspk.rs). Simply copy the name/date of the release and put it into your `build.gradle` file in the `minecraft` block.
![MCP Mappings](/tutorials/1.13.2/forge/1.0-gradle-configuration/mcp-mappings.png "MCP Mappings")  

You also need to add the following block of code to your `build.gradle`. This code makes sure that variables are correctly inserted into `mods.toml` when the mod is built or run.
```groovy
// Process resources on build
processResources {
	// This will ensure that this task is redone when the versions change.
	inputs.property 'version', project.version

	// Replace stuff in mods.toml, nothing else
	from(sourceSets.main.resources.srcDirs) {
		include 'META-INF/mods.toml'

		// Replace version
		expand 'version':project.version
	}

	// Copy everything else except the mods.toml
	from(sourceSets.main.resources.srcDirs) {
		exclude 'META-INF/mods.toml'
	}
}
```  

Finally you need to change your version in `mods.toml` at `/src/main/resources/META-INF/mods.toml`. This isn't technically related to gradle, but is required to build and run your mod. Simply change `version="${file.jarVersion}"` to `version="${version}"` in `mods.toml`. This changes the version from pointing to version of the final built mod (which doesn't exist yet, so won't work) to the current version of your mod as specified in build.gradle (which always exists).  
![mods.toml](/tutorials/1.13.2/forge/1.0-gradle-configuration/toml0.png "mods.toml")  
![mods.toml](/tutorials/1.13.2/forge/1.0-gradle-configuration/toml1.png "mods.toml")
