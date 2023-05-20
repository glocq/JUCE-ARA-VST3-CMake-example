JUCE example ARA VST3 plugin with CMake - for dummies
======================================================

This is a battery-included adaptation of the JUCE ARA plugin example to a CMake project. It features a working project, as well as the correct version of the necessary libraries and SDK in the form of git submodules.

It was not exactly easy for me to get an ARA plugin to build with CMake, and I was happy that I got *something* to work at last,
so I'm saving the result of my work here; maybe it'll help other dummies.

You should be able to compile and use JUCE's example ARA plugin without pain as long as you are able to follow the instructions here.
Note that this was only tested on Linux though.

Also note that my *only* aim here was to get a plugin to compile and work on my machine, and I achieved that by starting from [JUCE's example project](https://github.com/juce-framework/JUCE/blob/master/examples/Plugins/ARAPluginDemo.h) in the form of a "PIP", where all source code is crammed into a big text file. If you plan to write your own plugin, you will probably want to restructure the code to get a normal-looking C++ project; that I haven't managed to do yet.

Prerequisites
--------------

For building:
* You'll need CMake (minimum version 3.20; lower version number up to and including 3.15 should work too but you'll need to modify th relevant line in `CMakeLists.txt`).
* You'll need the standard build tools for your system, such as gcc, make for Linux, Xcode for MacOS... On Linux, those are probably installed on your system already.

For running:
* You need an ARA-compatible DAW. See [this Wikipedia page](https://en.wikipedia.org/wiki/Audio_Random_Access#cite_note-16) for the list. I have tested it with REAPER.

Instructions
-------------

1. Clone this repository **recursively**:
```
git clone --recurse-submodules https://github.com/glocq/JUCE-ARA-VST3-CMake-example
```
2. Navigate into it:
```
cd JUCE-ARA-VST3-CMake-example
```
3. Create a directory for compilation and enter it:
```
mkdir build
cd build
```
4. Run CMake, then make (on Linux) to build the plugin:
```
cmake ..
make
```
5. If you've made it this far, things are looking good. Copy the generated plugin into your VST3 folder:
```
cp -r AudioPluginExample_artefacts/VST3/Audio\ Plugin\ Example.vst3/ ~/.vst3
```
(the directory names might not correspond exactly, just make sure you copy the whole .vst3 folder into a folder where your DAW looks for VST3s)
6. Open your DAW, make sure `~/.vst3` (or whatever folder you copied your plugin into) is in your DAW's search paths.
7. Scan for new plugins.
8. Add the "Audio Plugin Example" to a track in your DAW.

