cmake_minimum_required(VERSION 3.15)
project(juce_vscode_audiofx VERSION 0.0.1)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(PLUGIN_MANIFACTURE_CODE AKYK)# 大文字4字でプラグインベンダーのIDを指定
set(PLUGIN_CODE AEFX)# 大文字4字でプラグインのユニークIDを指定
set(PLUGIN_NAME AudioEffect)

# include(CTest)
include(FetchContent)
# FetchContent_Declare(ame GIT_REPOSITORY https://github.com/AkiyukiOkayasu/ame GIT_TAG v1.0.0-rc5)
# FetchContent_MakeAvailable(ame)

# JUCE
add_subdirectory(JUCE)

# JUCE
juce_add_plugin(${PLUGIN_NAME}
    VERSION ${PROJECT_VERSION}
    # ICON_BIG ""   # ICON_* arguments specify a path to an image file to use as an icon for the Standalone
    # ICON_SMALL "${CMAKE_SOURCE_DIR}/assets/icon.png " 
    COMPANY_NAME "Akiyuki Okayasu"                    # Specify the name of the plugin's author
    COMPANY_COPYRIGHT "Akiyuki Okayasu"
    # COMPANY_WEBSITE ""
    COMPANY_EMAIL "akiyuki.okayasu@gmail.com"
    IS_SYNTH FALSE                       # Is this a synth or an effect?
    NEEDS_MIDI_INPUT FALSE               # Does the plugin need midi input?
    NEEDS_MIDI_OUTPUT FALSE              # Does the plugin need midi output?
    IS_MIDI_EFFECT FALSE                 # Is this plugin a MIDI effect?
    # EDITOR_WANTS_KEYBOARD_FOCUS TRUE/FALSE    # Does the editor need keyboard focus?
    COPY_PLUGIN_AFTER_BUILD TRUE        # ビルドするたびにプラグインをインストールするか？
    PLUGIN_MANUFACTURER_CODE ${PLUGIN_MANIFACTURE_CODE}               # 大文字4字でプラグインベンダーのIDを指定
    PLUGIN_CODE ${PLUGIN_CODE}                            # 大文字4字でプラグインのユニークIDを指定
    FORMATS 
    # ここでビルドしたいプラグインの種類を指定する
            Standalone 
            AU
            # AUv3 
            VST3 
            # Unity 
            # AAX           
    VST3_CATEGORIES "Fx" #複数(3つまで)列挙するとホスト上でフォルダにネストされて表示される
    AU_MAIN_TYPE "kAudioUnitType_Effect"
    # AU_SANDBOX_SAFE TRUE
    # AAX_CATEGORY ""
    # HARDENED_RUNTIME_ENABLED # macOS app settings
    # HARDENED_RUNTIME_OPTIONS
    # APP_SANDBOX_ENABLED TRUE
    # APP_SANDBOX_INHERIT
    # DESCRIPTION "" #設定するとProtoolsのプラグイン名がこのDescriptionになってしまう（バグ？）
    MICROPHONE_PERMISSION_ENABLED TRUE
    MICROPHONE_PERMISSION_TEXT "This applicaion requires a permission to use an audio input device of your computer. By Default, Built-In microphone will be used."
    PRODUCT_NAME ${PLUGIN_NAME}        # The name of the final 
)

add_subdirectory(src)

juce_generate_juce_header(${PLUGIN_NAME})

target_compile_options(${PLUGIN_NAME} PUBLIC -O2 -Wall)
target_compile_definitions(${PLUGIN_NAME} PUBLIC JUCE_VST3_CAN_REPLACE_VST2=0)
target_compile_features(${PLUGIN_NAME} PUBLIC cxx_std_20)
target_link_libraries(${PLUGIN_NAME} PRIVATE
    juce::juce_audio_basics
    juce::juce_audio_devices
    juce::juce_audio_formats
    juce::juce_audio_plugin_client
    juce::juce_audio_processors
    juce::juce_audio_utils
    juce::juce_core    
    juce::juce_data_structures
    juce::juce_dsp
    juce::juce_events
    juce::juce_graphics
    juce::juce_gui_basics
    juce::juce_gui_extra
    # ame
)