---
layout: page
title: 1.1 - Importing the project into your IDE
subtitle: Eclipse
---
This tutorial assumes you have already
- Read the [Pre-requisites](/tutorials/Pre-requisites)
- Downloaded the latest Forge MDK
- Setup your mod folder as described at the top of [the main Forge 1.14.4 tutorials page](/tutorials/1.14.4/forge/)
- Read and followed [1.0 - Gradle Configuration](../../1.0-gradle-configuration/)
- Chosen Eclipse from [1.1 - Importing the project into your IDE](..)

# Eclipse
1. Open up Eclipse and (if you aren't already there) switch to your "workbench".  
![workbench](./workbench.png "workbench")  
2. Go to `File > Import > Existing Gradle Project` and import the project.
![menu-import](./menu-import.png "menu-import")
![import-gradle-popup](./import-gradle-popup.png "import-gradle-popup")
![import-gradle-wizard](./import-gradle-wizard.png "import-gradle-wizard")
![import-gradle-select0](./import-gradle-select0.png "import-gradle-select0")  
![import-gradle-select1](./import-gradle-select1.png "import-gradle-select1")
3. Wait for the import/sync to complete  
![import-sync0](./import-sync0.png "import-sync0")  
![import-sync1](./import-sync1.png "import-sync1")  
![import-sync2](./import-sync2.png "import-sync2")  
4. Go to Gradle Tasks  
![gradle-tasks](./gradle-tasks.png "gradle-tasks")  
5. Run the `eclipse` task  
This will take a while as Forge downloads everything and sets up your modding environment.  
![gradlew-eclipse](./gradlew-eclipse.png "gradlew-eclipse")  
![gradlew-eclipse-exec](./gradlew-eclipse-exec.png "gradlew-eclipse-exec")  
6. Run the `genEclipseRuns` task to generate the run configs to run minecraft  
![gradlew-genEclipseRuns](./gradlew-genEclipseRuns.png "gradlew-genEclipseRuns")  
![gradlew-genEclipseRuns-exec](./gradlew-genEclipseRuns-exec.png "gradlew-genEclipseRuns-exec")
7. Choose your run configuration  
Go to the Run Configurations menu  
![menu-run-configs](./menu-run-configs.png "menu-run-configs")  
Choose `Java Application > runClient`  
![run-configs-popup](./run-configs-popup.png "run-configs-popup")  
Open the Run menu and select the `runClient` run configuration  
![menu-run-configs-after](./menu-run-configs-after.png "menu-run-configs-after")  
All of these run configs launch minecraft.
![run-configs-list](./run-configs-list.png "run-configs-list")  
	- `runClient` launches the minecraft client
	- `runData` launches minecraft in a special mode that only does the bare minimum required to automatically generate json files like recipes and blockstates.
	- `runServer` launches the minecraft server
8. Run the game  
![running](./running.png "running")  
9. Change package representation to `hierarchical` (optional)  
![package-presentation](./package-presentation.png "package-presentation")  
![package-presentation-hierarchical](./package-presentation-hierarchical.png "package-presentation-hierarchical")  

# Troubleshooting
When asking for help make sure to include your console log.  
![gradlew-genEclipseRuns-log](./gradlew-genEclipseRuns-log.png "gradlew-genEclipseRuns-log")  

#### If your run configs fail to generate:  
First try closing eclipse, then running `gradlew genEclipseRuns` from the command line, then opening eclipse back up.  
If this doesn't work **but the `.launch` files generate in your mod's folder properly**, try importing them manually.  
Do this by going to `File > Import > Launch Configurations`, selecting your mods folder and importing all run configs from there.  
Before:  
![run-configs-before](./run-configs-before.png "run-configs-before")  
![run-config-files](./run-config-files.png "run-config-files")  
After:  
![menu-import](./menu-import.png "menu-import")  
![import-launch-popup](./import-launch-popup.png "import-launch-popup")  
![import-launch-select](./import-launch-select.png "import-launch-select")  
![run-configs-after](./run-configs-after.png "run-configs-after")  

##### [1.2 - Basic Mod](../../1.2-basic-mod)
