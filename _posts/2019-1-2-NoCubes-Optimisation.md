---
layout: post
title: NoCubes Optimisation
subtitle: SurfaceNets is now theoretically 61x faster!
tags: [nocubes]
---

After dealing with Java *randomly swapping the values* of 2 of my variables, I’ve written my own implementation of Surface Nets that scans a 19\*19\*19 (6895) area, down from the original 18\*18\*18\*8\*9 (419,904) lookups. I've also done some variable/object pooling for better memory use. All this should make the algorithm more than 61 times faster. I still have some stuff to do with the buffer and buffer indices, but they shouldn't take long, though I might have to swap the x & z loops...