---
layout: page
title: 1.3 - Doing Something
---

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
![Log](/tutorials/1.13.2/forge/1.3-doing-something/log.png "Log")
