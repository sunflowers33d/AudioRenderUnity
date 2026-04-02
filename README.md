# AudioRenderUnity
A wrapper for tikonen's [AudioRender](https://github.com/tikonen/AudioRender) X/Y oscilloscope image drawing library to work with 3D Unity projects. Works best with analog oscilloscopes.

![A spinning grey cube in a Unity scene.](/images/Cube_Unity.gif)
![The same spinning cube in osci-render, a simulated X/Y oscilloscope.](/images/Cube_CXaudio.gif)
![An animated mushroom in a Unity scene.](/images/Mushroom_Unity.gif)
![The same animated mushroom as X/Y oscilloscope data.](/images/Mushroom_CXAudio.gif)

## IMPORTANT: Your audio output matters!
It is essential that the sound device sending the X/Y audio data to your oscilloscope is **DC-coupled**, as the audio data this project generates will have moments where the signal will suddenly stop. AC-coupled DACs tend to "smooth" out the end of an abrupt waveform in a way that significantly distorts the resultant oscilloscope image.
*TL;DR: please don't just use your computer's built-in sound card, it won't go well...!*

![The same spinning cube, routed through a Realtek built-in sound device.](/images/Cube_RealtekAudio.gif)

I have had success using CX31993-based USB-C dongles, though the developer of AudioRender has [made their own purpose-based solution](https://github.com/tikonen/DACDriver) with an STM32F407-DISCOVERY1 which may be more favorable.

## How do I use this in Unity?
1. Clone this repository into a folder where you intend to build off of in Unity. The example project in this repo is from Unity 2021.1, though it can be upgraded to 2022.3.62f3 without issue. *Unity 6 has not yet been tested*.
2. Place any 3D GameObjects under the **Wireframe Renderer**, ensuring that the *Render Device Type* is set to your intended option.
    1. Audio: send your 3D scene and objects as X/Y audio data to send to a real analog oscilloscope or a simulated analog oscilloscope. *While digital oscilloscopes will technically work, the latency and slow update timing may not be ideal.*
    2. Emulator: send 3D scene as wireframe data to the oscilloscope "emulator" contained within AudioRenderCAPI.
    3. Gfx: replace every 3D object inside of **Wireframe Renderer** with an oscilloscope-style wireframe shader.

## Other considerations
If you are not using the pre-made Unity project, ensure that the following is true:
 - Static Batching: Off
 - Dynamic Batching: Off
 - Scripting Backend: IL2CPP
 - Api Compatibility Level: .NET Framework (labeled as .NET 4.x in older Unity versions)
 - Packages: Burst, Collections, Mathematics
 - Read/Write enabled from Unity is required for meshes
 - **Oscilloscope rendering:** set the desired audio output to the default device in Windows
    - Open Run (Win + R), enter <code>mmsys.cpl</code>, click the desired oscilloscope audio output device and then press **Set Default**.

## Features
 - Audio rendering (oscilloscope)
 - Emulator renderer (external included program)
 - Shader rendering (Unity)
 - Signal scaling (for oscilloscopes)
 - Signal intensity
 - Global random line offset
 - Line clipping
 - Edge angle limit
 - Backface culling
 - Occlusion culling
 - LODs
 - Static geomery
 - Skinned geometry
 - Example scenes
  - Clipping (multiple overlapping objects)
  - Cube (basic rendering)
  - Effect (explosion)
  - RenderDevice (direct drawing for custom routines)
  - Skinned (animation)
  - Triangles (LODs)

## Performance Tips
 - Try to keep line count as low as possible.
 - Do not split model edges, as this doubles the line count (use smooth faces).
 - Use high edge angle limits on wireframe objects to skip lines in geometry.
 - Break static geomery into small sections for baked occlusion culling.
 - Keep camera far clipping plane as low as possible.
 - Use LODs on distant objects.
 
Command line arguments for builds:
 - <code>-audioRender</code> (render to oscilloscope)
 - <code>-emulator</code> (render to emulator built into AudioRenderCAPI)
 - <code>-gfx</code> (render oscilloscope shader in Unity)
 - <code>-scaleX [int]</code> (scale signal for oscilloscopes)
 - <code>-scaleY [int]</code> (scale signal for oscilloscopes)