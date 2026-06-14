# Novix Project - Minecraft 26.1.2 in the Browser

⚡ Minecraft 26.1.2 has been ported to the browser. Introducing the Novix Project. 

🚀 Novix 26.1.2 allows you to play the most recent version of Minecraft (**26.1.2**) in your browser for free. Novix implements the Rust language and WebGPU (NOT webgl like all other eagler versions), which boosts performance up to **120 fps**. The project is being **actively developed**!

--------------------------------------------------------------------------------------------------------------------------------------------------

novus has been working on an experimental browser port of modern minecraft java, and i finally have an initial build worth sharing.


the main goal is to keep mojang’s 26.1.2 client code as close to vanilla as possible, and shim around it surgically instead of maintaining a heavily modified source tree. since modern minecraft ships effectively unobfuscated, the port works from a vineflower decompile and uses bytecode repatching where needed.

the biggest difference is rendering. instead of emulating opengl over webgl, this implements the modern blaze3d backend spi directly on webgpu, while leaving the vanilla renderer frontend mostly untouched. shaders are translated from glsl to wgsl at build time.

there’s also a lot of teavm compiler work. this is running on newer teavm (14.x), but modern minecraft hits a bunch of edge cases, so i ended up adding compiler plugins and classlib overrides for things like class initialization order, float precision, suppressed exceptions, and patched library behavior.

the integrated server worker also uses the real vanilla packet encoder and decoder instead of a custom ipc protocol. the worker boundary carries the actual protocol byte stream through transferable arraybuffers, so the goal is to stay byte-identical to the normal java path.

chunk meshing is being moved into workers too. the mesh-worker pipeline snapshots section data into a byte-parity codec, runs the vanilla section compiler off-thread, and sends results back w/ frame-budgeted uploads. the performance focus is worst-frame time (not fps).

currently, only singleplayer is supported. superflat worlds are easiest to load into. currently the performance is abhorrent but this will be fixed relatively easily to allow for wider support on all devices (yes, your school chromebooks too) due to its relatively more performant nature (projected large increase over regular eagler ports). remember to lower your settings.

dc (bugs? comments? questions?):

https://discord.gg/knMvyeGMaq
