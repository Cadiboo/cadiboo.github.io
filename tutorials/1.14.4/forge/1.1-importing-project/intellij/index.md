---
layout: page
title: 1.1 - Importing the project into your IDE
subtitle: IntelliJ
---
This tutorial assumes you have already
- Read the [Pre-requisites](/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.14.4 tutorials page](/tutorials/1.14.4/forge/)
- Read and followed [1.0 - Gradle Configuration](../../1.0-gradle-configuration/)
- Chosen IntelliJ from [1.1 - Importing the project into your IDE](..)

# IntelliJ
1. Open build.gradle as a Project by going to `Menu > Import > build.gradle` or `File > Open > build.gradle > Open as Project` to switch if you are already working on another project  
![Import](./import.png "Import")  
![Import File](./import-file.png "Import File")  
2. Use default settings  
![Import Project](./import-project.png "Import Project")  
Personally I uncheck `Create separate module per source set`  
![Separate Module Checkbox](./separate-module-checkbox.png "Separate Module Checkbox")  
3. Wait for build sync to download everything and finish  
This will take a while as Forge downloads everything and sets up your modding environment.  
![Sync View 0](./sync-view-0.png "Sync View 0")  
![Sync View 1](./sync-view-1.png "Sync View 1")  
![Sync Download](./sync-download.png "Sync Download")  
4. Refresh gradle project  
![Gradle Tab](./gradle-tab.png "Gradle Tab")  
![Gradle Refresh](./gradle-refresh.png "Gradle Refresh")  
5. Run the task `genIntellijRuns` to generate the run configs to run minecraft  
![genIntellijRuns](./genIntellijRuns.png "genIntellijRuns")  
![genIntellijRuns exec](./genIntellijRuns-exec.png "genIntellijRuns exec")  
6. Go to the Run Configurations menu and fix (set the module of) your `runClient` run configuration  
![Configs](./configs.png "Configs")  
![Select Run Config](./select-run-config.png "Select Run Config")  
![Edit Configs](./edit-configs.png "Edit Configs")  
![Error No Module](./error-no-module.png "Error No Module")  
![Module](./module.png "Module")  
![Set Module](./set-module.png "Set Module")  
Save your changes  
All of these run configs launch minecraft.  
![Run configs list](./run-configs-list.png "Run configs list")  
	- `runClient` launches the minecraft client
	- `runData` launches minecraft in a special mode that only does the bare minimum required to automatically generate json files like recipes and blockstates.
	- `runServer` launches the minecraft server
7. Run the game  
![Run `runClient`](./run-runClient.png "Run runClient")  
![Running](./running.png "Running")  

# Troubleshooting
When asking for help make sure to include your console log.  
![genIntellijRuns log](./genIntellijRuns-exec.png "genIntellijRuns log")  

##### [1.2 - Basic Mod](../../1.2-basic-mod)
