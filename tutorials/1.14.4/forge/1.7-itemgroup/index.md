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

ItemGroups (previously CreativeTabs)
1. New package init
2. New class `ModItemGroups`
3. New >Inner Class `ModItemGroup`
4. Override method `createItem` >Supplier (>Lambdas) 
5. Constant `ItemGroup` `MOD_GROUP`
6. Oh no, no reference to Item!
    1. Create `ModItems`
    2. `@ObjectHolder`
    3. `public static final Item REGISTRY_NAME = null`
    4. Oh no, warnings!
    5. `_null();`
