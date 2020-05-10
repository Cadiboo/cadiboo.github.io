---
layout: page
title: 3.3 - Config
---
This tutorial assumes you have already
- Read the [Pre-requisites](/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.15.1 tutorials page](/tutorials/1.15.1/forge/)
- Read and followed all of Chapter 1
- Read and followed all of Chapter 2

Firstly, make a class in a new package called config. Call it "MyModConfig" or something.  
```java
public class YourConfig {

	public static final ClientConfig CLIENT;
	public static final ForgeConfigSpec CLIENT_SPEC;
	static {
		final Pair<ClientConfig, ForgeConfigSpec> specPair = new ForgeConfigSpec.Builder().configure(ClientConfig::new);
		CLIENT_SPEC = specPair.getRight();
		CLIENT = specPair.getLeft();
	}

	// Doesn't need to be an inner class
	public static class ClientConfig {

		 public ClientConfig(ForgeConfigSpec.Builder builder) {

		 }

	}
}
```
This code creates the specification for your config. This controls what is allowed in your config. Forge automatically handles validating your config based on this.  

Now edit your ClientConfig class to put your config spec values in.

```java

public class ClientConfig {

	public final BooleanValue aBoolean;
	public final IntValue anInt;

	public ClientConfig(ForgeConfigSpec.Builder builder) {
		aBoolean = builder
				.comment("aBoolean usage description")
				.translation(YourMod.MODID + ".config." + "aBoolean")
				.define("aBoolean", false);

		builder.push("category");
		anInt = builder
				.comment("anInt usage description")
				.translation(YourMod.MODID + ".config." + "anInt")
				.defineInRange("anInt", 10, 0, 100);
		builder.pop();
	}

}

```
This code actually defines the values in your config, their name, their comment, their translation key and their default value. The calls to `push` and `pop` make a category. Categories can be nested.  
Next you need to register your config (inside your mod's constructor in your main mod class)  
```java
ModLoadingContext.get().registerConfig(ModConfig.Type.CLIENT, YourConfig.CLIENT_SPEC);
```
This code registers your config with Forge. This allows server configs to be synced when you join a server so everyone has the same configuration and Forge to fire the appropriate config events for you.  

Now make a method to bake your config values  
> "Baking" means taking the values from the config object, turning them into Java objects and putting them in a field.

We bake a config because calling the get/set methods on the config object are much more expensive than just field access.  
```java
public class YourConfig {
	public static final ClientConfig CLIENT;
	public static final ForgeConfigSpec CLIENT_SPEC;
	static {
		final Pair<ClientConfig, ForgeConfigSpec> specPair = new ForgeConfigSpec.Builder().configure(ClientConfig::new);
		CLIENT_SPEC = specPair.getRight();
		CLIENT = specPair.getLeft();
	}

	// These can be made private and replaced with getters
	public static boolean aBoolean;
	public static int anInt;

	public static void bakeConfig() {
		aBoolean = CLIENT.aBoolean.get();
		anInt = CLIENT.anInt.get();
	}

	//...

}
```

Next we need to call the bake method at the right time.  
Make an event subscription method and subscribe the class to the mod event bus.  
Event subscription method:  
```java
@SubscribeEvent
public static void onModConfigEvent(final ModConfig.ModConfigEvent configEvent) {
	if (configEvent.getConfig().getSpec() == YourConfig.CLIENT_SPEC) {
		bakeConfig();
	}
}
```
Class event subscriber annotation:  
```java
@EventBusSubscriber(modid = YourMod.MODID, bus = EventBusSubscriber.Bus.MOD)
public class YourConfig {
	//...
}
```

Your final config class should look something like  
```java
@EventBusSubscriber(modid = YourMod.MODID, bus = EventBusSubscriber.Bus.MOD)
public class YourConfig {
	public static final ClientConfig CLIENT;
	public static final ForgeConfigSpec CLIENT_SPEC;
	static {
		final Pair<ClientConfig, ForgeConfigSpec> specPair = new ForgeConfigSpec.Builder().configure(ClientConfig::new);
		CLIENT_SPEC = specPair.getRight();
		CLIENT = specPair.getLeft();
	}

	public static boolean aBoolean;
	public static int anInt;

	@SubscribeEvent
	public static void onModConfigEvent(final ModConfig.ModConfigEvent configEvent) {
		if (configEvent.getConfig().getSpec() == YourConfig.CLIENT_SPEC) {
			bakeConfig();
		}
	}

	public static void bakeConfig() {
		aBoolean = CLIENT.aBoolean.get();
		anInt = CLIENT.anInt.get();
	}

	public static class ClientConfig {

		public final BooleanValue aBoolean;
		public final IntValue anInt;

		public ClientConfig(ForgeConfigSpec.Builder builder) {
			aBoolean = builder
					.comment("aBoolean usage description")
					.translation(YourMod.MODID + ".config." + "aBoolean")
					.define("aBoolean", false);

			builder.push("category");
			anInt = builder
					.comment("anInt usage description")
					.translation(YourMod.MODID + ".config." + "anInt")
					.defineInRange("anInt", 10, 0, 100);
			builder.pop();
		}

	}

}
```
And you should have added  
```java
ModLoadingContext.get().registerConfig(ModConfig.Type.CLIENT, YourConfig.CLIENT_SPEC);
```
to your mod's constructor
