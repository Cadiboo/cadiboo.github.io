---
layout: post
title: NoCubes Optimisation
subtitle: SurfaceNets is now 8x faster!
tags: [nocubes]
---

I’ve fixed all the world-hole bugs and optimised the render algorithms in a pre-release on GitHub, it’s not stable yet though. All algorithms are now faster by a factor of 8. Instead of doing 18\*18\*18\*8\*9 (419,904) lookups I’m now only doing 18\*18\*18\*9 (52,488) lookups. It’s now faster than vanilla, and I’m planning on getting the lookups down to smaller than 20\*20\*20 (8000) to make it about 52x faster. 

I’m also working on improving the lighting code and the liquid extension code.