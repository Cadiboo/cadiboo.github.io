---
layout: page
title: Forge MDK Files
---

The only files we will need are highlighted. You can discard the other files.  
![required-files](./required-files.png "required-files")  
- `.gitignore` is used for Version Control. It specifies intentionally untracked files that Git should ignore. It is required for our mod to work as we will be using Version Control in the future.
- `build.gradle` is the [build configuration script](https://www.tutorialspoint.com/gradle/gradle_build_script.htm) that gradle uses for setting up and building your mod and mod dev workspace. It is required for our mod to work as it allows gradle to function.
- `changelog.txt` contains Forge's changelog and is not needed for our mod to work.
- `CREDITS.txt` contains Forge's credits and is not needed for our mod to work.
- The `gradle` folder contains the [gradle wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html). It is required for our mod to work so that we can use the gradle wrapper.
- `gradle.properties` is the extra configuration file that gradle uses for settings related to the project. By default it only sets the memory available to gradle and disables the gradle daemon (Because Forge's gradle system doesn't work with the daemon yet). However, you can add your own variables to this file and use them in `build.gradle` as I do in my [example mod](https://github.com/Cadiboo/Example-Mod) [here](https://github.com/Cadiboo/Example-Mod/blob/1.15.1/gradle.properties#L8-L15) and [here](https://github.com/Cadiboo/Example-Mod/blob/1.15.1/build.gradle#L21). It is required for our mod to work so that gradle does not use the daemon.
- `gradlew` is the gradle wrapper startup script for UN\*X (Mac OSX, Linus, Solaris etc.). It is required for our mod to work so that we can launch the gradle wrapper on UN\*X.
- `gradlew.bat` is the gradle wrapper startup script for Windows. It is required for our mod to work so that we can launch the gradle wrapper on Windows.
- `LICENSE.txt` contains Forge's license and is not needed for our mod to work.
- `README.txt` contains info about Forge's MDK. You should read it but it is not needed for our mod to work.
- The `src` folder contains our mods code and assets. Currently it only contains an example of a very basic mod provided by Forge, but we will be changing that soon.  It is required for our mod to work so that we have a place for our mod's files.

##### [Forge 1.15.1 Tutorials](..)
