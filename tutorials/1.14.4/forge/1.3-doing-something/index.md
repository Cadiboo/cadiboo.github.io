---
layout: page
title: 1.3 - Doing Something
---
This tutorial assumes you have already
- Read the [Pre-requisites](https://cadiboo.github.io/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.14.4 tutorials page](/tutorials/1.14.4/forge/)
- Read and followed [1.0 - Gradle Configuration](https://cadiboo.github.io/tutorials/1.14.4/forge/1.0-gradle-configuration/)
- Read and followed [1.1 - Importing the project into your IDE](https://cadiboo.github.io/tutorials/1.14.4/forge/1.1-importing-project/)
- Read and followed [1.2 - Basic Mod](https://cadiboo.github.io/tutorials/1.14.4/forge/1.2-basic-mod/)

# This tutorial isn't finished yet. However you can always look at the code in [my ExampleMod](https://github.com/Cadiboo/Example-Mod/), where I write and perfect the stuff I teach in these tutorials. My main mod class is [here](https://github.com/Cadiboo/Example-Mod/blob/f7493385a1d6e4b41fabb1f75e4ff942208ca97a/src/main/java/io/github/cadiboo/examplemod/ExampleMod.java)

1) make a >public >no args >constructor with nothing in it  
2) Now make a constant logger for your mod  
`public static final Logger LOGGER = LogManager.getLogger(MOD_ID);` (import the log4j logger not the java.util one)  
3) Then in your constructor call `LOGGER.debug("Hello from YourModName!");`  
4) for eclipse people refresh `/src/`  
5) If you run your game again, you should be able to see "Hello from YourModName!" in your log  
The final result should look something like this
```java
package io.github.cadiboo.examplemod;

import net.minecraftforge.fml.common.Mod;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

/**
 * @author Cadiboo
 */
@Mod(ExampleMod.MODID)
public final class ExampleMod {

	public static final String MODID = "examplemod";

	public static final Logger LOGGER = LogManager.getLogger(MODID);

	public ExampleMod() {
		LOGGER.debug("Hello from Example Mod!");
	}

}
```
![Log](/tutorials/1.14.4/forge/1.3-doing-something/log.png "Log")
