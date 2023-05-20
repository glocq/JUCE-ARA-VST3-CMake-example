# Specify minimal CMake version.
# JUCE's original example says 3.15, but this blog post https://alexreinking.com/blog/how-to-use-cmake-without-the-agonizing-pain-part-1.html
# recommends using the most recent version, so I bumped it up.
cmake_minimum_required(VERSION 3.20)

# Name and version of our project
project(MY_ARA_PLUGIN VERSION 0.0.1)


# JUCE library code, and ARA SDK, both added to this repository as a submodule
# (don't forget to recursively pull submodules!)
add_subdirectory(JUCE)
juce_set_ara_sdk_path(ARA_SDK)


# Lots of options here, see https://github.com/juce-framework/JUCE/blob/master/docs/CMake%20API.md
# for more info
juce_add_plugin(MyARAPlugin
    # VERSION ...                               # Set this if the plugin version is different to the project version
    # ICON_BIG ...                              # ICON_* arguments specify a path to an image file to use as an icon for the Standalone
    # ICON_SMALL ...
    # COMPANY_NAME ...                          # Specify the name of the plugin's author
    # IS_SYNTH TRUE/FALSE                       # Is this a synth or an effect?
    # NEEDS_MIDI_INPUT TRUE/FALSE               # Does the plugin need midi input?
    # NEEDS_MIDI_OUTPUT TRUE/FALSE              # Does the plugin need midi output?
    # IS_MIDI_EFFECT TRUE/FALSE                 # Is this plugin a MIDI effect?
    IS_ARA_EFFECT TRUE
    # EDITOR_WANTS_KEYBOARD_FOCUS TRUE/FALSE    # Does the editor need keyboard focus?
    # COPY_PLUGIN_AFTER_BUILD TRUE/FALSE        # Should the plugin be installed to a default location after building?
    PLUGIN_MANUFACTURER_CODE Juce               # A four-character manufacturer id with at least one upper-case character
    PLUGIN_CODE Dem0                            # A unique four-character plugin id with exactly one upper-case character
                                                # GarageBand 10.3 requires the first letter to be upper-case, and the remaining letters to be lower-case
    FORMATS VST3                                # The formats to build. Other valid formats are: AAX Unity VST AU AUv3
    PRODUCT_NAME "MyARAPlugin")                 # The name of the final executable, which can differ from the target name


# Enable use of `#include <JuceHeader.h>` in C++ source code
juce_generate_juce_header(MyARAPlugin)


# Source code for our plugin
target_sources(MyARAPlugin
    PRIVATE
        src/Main.cpp
        src/ARAPluginDemo.h)


# JUCE options as well as your own definitions to be used in the code
target_compile_definitions(MyARAPlugin
    PUBLIC
        JUCE_DISPLAY_SPLASH_SCREEN=0 # from what I understand, you are only allowed to remove the splash screen
                                     # if your code is licensed under the GPL, so if not, you should remove this definition
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0)


target_link_libraries(MyARAPlugin
    PRIVATE
        juce::juce_audio_utils
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)
