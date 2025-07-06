Hertz measures the refresh rate of the screen itself.
If a computer monitor is set to 60 Hz that means it can display a maximum of 60 images per second.

75Hz, 85Hz, and 144Hz are also common for desktop monitors. For the competitive gaming scene there are even higher refresh rates.
So if your app could reliably reach 85FPS then it will perform well with VSync on for all displays.

default built-in render pipeline - BRP for short
Unity基本有两种render方式, BRP和URP.



# Render basic

每次图形被render几次,会影响渲染速度.
Example:
There were 30,003 batches, and zero saved by batching.
These are draw commands sent to the GPU.
Our graph contains 10,000 points, so it appears that each point got rendered three times.

 - once for a depth pass
 - once for shadow casters—listed separately as well
 - once to render the final cube, per point

The other three batches are for additional work like the sky box and shadow processing that is independent of our graph.
There were also six set-pass calls, which can be thought of as the GPU getting reconfigured to render in a different way, like with a different material.

If we use URP instead the statistics are different. It renders faster.
It's easy to see why: there are only 20,002 batches, 10,001 less than for BRP. That's because URP doesn't use a separate depth pass for directional shadows.
It does have more set-pass calls, but that doesn't appear to be a problem.

URP uses the SRP batcher by default.
SRP Batcher 是一个渲染循环，可通过许多使用同一着色器变体的材质来加快场景中的 CPU 渲染速度。
The SRP batcher doesn't eliminate individual draw commands but can make them much more efficient.
如果disabled SRP batcher渲染会变慢.

使用dynamic batching也可以优化渲染.
This is an old technique that dynamically combines small meshes into a single larger one which then gets rendered instead.

The SRP batches isn't available for BRP, but we can enable dynamic batching for it.

enabling GPU instancing也可以优化渲染.
>https://zhuanlan.zhihu.com/p/523702434
GPU Instancing是一种Draw call的优化方案，使用一个Draw call就能渲染具有多个相同材质的网格对象。而这些网格的每个copy称为一个实例。此技术在一个场景中对于需要绘制多个相同对象来说是一个行之有效办法，例如树木或灌木丛的绘制。

In this case we have to enable it per material. Ours have an Enable GPU Instancing toggle for it.

URP prefers the SRP batcher over GPU instancing.
>选了SRP batcher会自动disable GPU instancing?

We could conclude from this data that for URP **GPU Instancing** is best, followed by dynamic batching, and then the SRP batcher.
_But the difference is small, so they seem effectively equivalent for our graph._
For BRP GPU instancing results in more batches than dynamic batching, but the frame rate is a little bit higher.



# Frame debugger

使用frame debugger可以看到每个frame都发生了什么.要使用frame debugger得在play mode,因为只有play mode graph才会被draw.
When enabled via its toolbar button it shows a list of all draw commands sent to the GPU for the last frame of the game window, grouped under profiling samples.
>Window / Analysis / Frame Debugger

在教材里这里用frame debugger很清晰的看到了dynamic batching和SRP batcher从万减轻到几十,牛逼!

>A significant difference is that dynamic batching doesn't appear to work for the shadow map, which explains why it's less effective for URP.



# Point light

What if add a point light via GameObject / Light / Point Light

BRP supports shadows for point lights, but URP still doesn't.

With the extra light BRP now draws all points an additional time. - 直接乘2!
Even worse, dynamic batching now only works for the depth and shadow passes, not the forward passes.
>dynamic batching在这里没有把一万给缩小.

This happens because BRP draws each object once per light. It has a main pass that works with a single directional light, followed by additional passes are rendered on top of it.
This happens because it's an old-fashioned forward-additive render pipeline.
Dynamic batching cannot handle these different passes, so doesn't get used.

The same is true for GPU instancing, except that it still works for the main pass. Only the additional light passes don't benefit from it.



# Profiler window

To get a better idea of what's happening on the CPU side we can open the profiler window.
>open the window via Window / Analysis / Profiler.
