---
layout: page
title: 1.4 - Proxies
---

This page assumes you already have basic knowledge of minecraft modding and Forge.

# If you're reading this, you don't need a Proxy
- Proxies are a way to run different code depending on the ***PHYSICAL*** side (Physical sides are also called `Distribution`s) that your mod is running on.  
- 1.7.10 introduced Registry Events, which made proxies and the preInit mod loading event pretty much unnecessary.  
- Many tutorials still recommend proxies because the tutorial writers are using extremely outdated practices that they learned from people who learned from people who used proxies back when they were necessary in 1.2.5.  
- Proxies are still useful in a tiny handful of situations but by the time you get into one of these situations you will understand the need for the proxy and this won't happen until your mod is very advanced  
- Beginners don’t need a proxy and beginners attempting to use a proxy just makes it a lot easier for beginners to break everything  

If you have a class called `ClientProxy`, `CommonProxy` or `ServerProxy` from a tutorial you followed I recommend you delete it.
Sided Event Subscribers replace proxies functionality in 99% of cases and are necessary anyway.
