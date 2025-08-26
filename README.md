# ⚡🦊 RapidFox: Simple Zen / Firefox Optimization Guide

A highly structured and comprehensive performance guide for Firefox/Zen Browser users seeking peak responsiveness, efficiency and memory management optimized for 4GB, 8GB and 16GB RAM systems.

**🧪 Tested Performance Improvement:** [Speedometer 3.1](https://browserbench.org/Speedometer3.0/) benchmark on 2016 ASUS laptop (i5-6300HQ, 8GB RAM, GTX 960M, 70 Mbps) improved from **6.90 → 10.10 (+47%)** with these optimizations.

# > [Open The Optimization Guide](https://github.com/Eratas/rapidfox/wiki/Guide)

## > [Featured on Reddit](https://www.reddit.com/r/zen_browser/comments/1l3y35d/zen_optimizations/)


## Why V8.0 will be the final version

After testing almost all avaiable browsers, I went back on Vivaldi; everything is so much better now.

Unoptimized Firefox gets a DOM score of ~7 for me, ~85 for web assembly & javascript and ~500 for 3D (with 2 GPU incompatibility).

After optimization: ~10, ~105 and ~650.

Vivaldi gets ~13, ~166, ~1200. Even with custom CSS, fully customized to look similar to Zen/Arc, same privacy, good security (isolation too), less RAM consumption, more compatibility.

New experimental Chrome compiled builds discard old CPU compatibility (before 2011) to gain ~30% more performances, with modifications to SSE4.2, AVX, AES, CFLAGS, LDFLAGS, thinLTO flags, import_instr_limit, LLVM LOOP, -mllvm flags, PGO and other compiler flags.

They are also actively working on a new UI and Skia Graphite (Metal, Vulkan, WebGPU) with GPU rasterization, which you can already try; I Scored a whopping 2200 score on motion mark, but it is still unstable.

GPU rasterization alone works fine, all pages are loaded with the CPU and both GPUs.
