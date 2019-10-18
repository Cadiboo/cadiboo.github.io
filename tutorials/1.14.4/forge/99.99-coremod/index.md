---
layout: page
title: 99.99 - Coremod
subtitle: Expert modders only!
---

# This is not meant to be a tutorial on writing ASM. If you need a tutorial on that, you shouldn't be reading this.
### This is mostly about how to update coremods from 1.12.2 to 1.14.4

### This is just a draft of the tutorial, more details will be added soon™

#### To have a javascript coremod you need
- A mod
- A `coremods.json` file in `META-INF` containing a list of all your transformers
- Javascript (`.js`) transformer files (listed in `coremods.json`)

##### `META-INF/coremods.json` ([Example](https://github.com/Cadiboo/NoCubes/tree/1.14.x/src/main/resources/META-INF/coremods.json))
```json
{
	"Transformer 1 Name": "path/to/transformer-1.js",
	"Transformer 2 Name": "path/to/transformer-2.js"
}
```

##### Javascript transformers ([Example](https://github.com/Cadiboo/NoCubes/blob/1.14.x/src/main/resources/nocubes-transformer.js))
Your transformer script must have a function called `initializeCoreMod` which returns a JSON object containing all your transformer objects.  
Each transformer can target either a `FIELD`, `METHOD` or `CLASS`.  
ModLauncher runs transformers in the order `FIELD`, then `METHOD` then `CLASS` for each class it loads.  
###### Field transformers
Format:
```javascript
"Transformer Name": {
	"target": {
		"type": "METHOD",
		"class": "cannonical.name.of.Class",
		"fieldName": "srgFieldName"
	},
	"transformer": function(fieldNode) {
		// Apply transformations
		return fieldNode;
	}
}
```
###### Method transformers
Format:
```javascript
"Transformer Name": {
	"target": {
		"type": "METHOD",
		"class": "cannonical.name.of.Class",
		"methodName": "srgMethodName",
		"methodDesc": "srgMethodDesc"
	},
	"transformer": function(methodNode) {
		// Apply transformations
		return methodNode;
	}
}
```
###### Class transformers
Format:
Single Class:
```javascript
"Transformer Name": {
	"target": {
		"type": "CLASS",
		"name": "cannonical.name.of.Class"
	},
	"transformer": function(classNode) {
		// Apply transformations
		return classNode;
	}
}
```
Multiple Classes:
```javascript
"Transformer Name": {
	"target": {
		"type": "CLASS",
		"names": function(listOfClasses) {
			return ["cannonical.name.of.Class1", "cannonical.name.of.Class2"];
		}
	},
	"transformer": function(classNode) {
		// Apply transformations
		return classNode;
	}
}
```
###### Transformer setup
Using Java classes
```javascript
var Opcodes = Java.type("org.objectweb.asm.Opcodes");
var ASMAPI = Java.type("net.minecraftforge.coremod.api.ASMAPI");
```
See allowed classes at `net.minecraftforge.coremod.CoreModEngine#ALLOWED_CLASSES`
###### ASMAPI
```javascript
var mappedFieldName = ASMAPI.mapField("srgFieldName");
var mappedMethodName = ASMAPI.mapMethod("srgMethodName");
```
Examples:
- [testcoremod.js](https://github.com/MinecraftForge/CoreMods/blob/master/src/test/javascript/testcoremod.js)
- [testmethodcoreinsert.js](https://github.com/MinecraftForge/CoreMods/blob/master/src/test/javascript/testmethodcoreinsert.js)

###### Transforming
Loop Instructions (Instructions are an `InsnList`)
```javascript
var arrayLength = instructions.size();
for (var i = 0; i < arrayLength; ++i) {
	var instruction = instructions.get(i);
	// Do stuff with instruction
}
```
```javascript
for (var it = instructions.iterator(); it.hasNext();) {
	var instruction = it.next();
	// Do stuff with instruction
}
```
Loop Methods (Methods are a `List<MethodNode>`)
```javascript
for (var i in methods) {
	var method = methods[i];
	// Do stuff with method
}
```
```javascript
methods.forEach(function(method) {
	// Do stuff with method
});
```
###### Debugging
If the coremod has syntax errors or something check the very top of your log (before anything like `Scanning Mod File:` appears). This is at the very top and is usually to high up for IntelliJ's console to catch it so use the log in `/run/logs/`.  
If the coremod failed to transform properly search for `<eval>` and/or `XFORM` in the log. This will be wherever the class gets initialised so probably towards the end.
###### Full Transformer
Example
```javascript
function initializeCoreMod() {
	FIELD_INSN = Java.type("org.objectweb.asm.tree.AbstractInsnNode").FIELD_INSN;
	nameTessellatorINSTANCE = Java.type("net.minecraftforge.coremod.api.ASMAPI").mapField("field_78398_a");
	return {
		"Tessellator.getInstance() Transformer": {
			"target": {
				"target": {
					"type": "METHOD",
					"class": "net.minecraft.client.renderer.Tessellator",
					"methodName": "func_178181_a",
					"methodDesc": "()Lnet/minecraft/client/renderer/Tessellator;"
				},
				"transformer": function(methodNode) {
					// Changes "return Tessellator.INSTANCE" in "Tessellator.getInstance()" to "return NewTessellator.INSTANCE"
					for (var it = methodNode.instructions.iterator(); it.hasNext();) {
						var instruction = it.next();
						if (instruction.getType() == FIELD_INSN && instruction.name.equals(nameTessellatorINSTANCE)) {
							instruction.owner = "io/github/cadiboo/examplemod/client/hooks/NewTessellator";
							instruction.name = "INSTANCE";
							instruction.desc = "Lio/github/cadiboo/examplemod/client/hooks/NewTessellator;";
							break;
						}
					}
					return classNode;
				}
			}
		}
	}
}
```
