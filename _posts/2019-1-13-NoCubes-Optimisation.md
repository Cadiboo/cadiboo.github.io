---
layout: post
title: NoCubes Optimisation
subtitle: Improved RAM Usage! All algoritms are now a lot faster!
tags: [nocubes]
---

I've made everything pooled (Faces, Caches, Vec3s).
I've modularised the entire mod.
Rendering is now completely split from mesh generation.
I build my caches as `meshSize + 1` on all axis, `meshSize = 18` for Surface Nets and `meshSize = 17` for all other mesh generators.
I now use 1 `PooledMutableBlockPos` for my entire mesh gen & rendering instead of more than 500,000 `BlockPos`s.
This means I do 19\*19\*19 (6859) lookups with 1 pos for Surface Nets and 18\*18\*18 (5832) lookups with 1 pos for all other mesh generators.
This makes my current approach more than 61x faster for Surface Nets and 72x faster for all other all other mesh generators†.
_All_ transparency issues are now solved!

†This does not include the performance gains I get from only using 1 `PooledMutableBlockPos` instead of many, many `BlockPos`s.
