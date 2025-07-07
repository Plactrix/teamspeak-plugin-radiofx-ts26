cmake_minimum_required (VERSION 3.6)

project (RadioFx LANGUAGES CXX)

find_package(Qt5 COMPONENTS Gui Widgets REQUIRED)
link_libraries(Qt5::Gui Qt5::Widgets)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

include_directories(
    "deps/teamspeak-plugin-radiofx/src"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/include"
)

set (DSP_FILTERS
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Bessel.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Biquad.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Butterworth.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Cascade.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/ChebyshevI.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/ChebyshevII.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Custom.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Design.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Documentation.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Elliptic.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Filter.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Legendre.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/Param.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/PoleFilter.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/RBJ.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/RootFinder.cpp"
    "deps/teamspeak-plugin-radiofx/deps/DSPFilters/shared/DSPFilters/source/State.cpp"
)
source_group("DSPFilters" FILES ${DSP_FILTERS})

set (PLUGIN_SRC
    "deps/teamspeak-plugin-radiofx/src/config_radio.h"
    "deps/teamspeak-plugin-radiofx/src/config_radio.cpp"
    "deps/teamspeak-plugin-radiofx/src/config_radio.ui"
    "deps/teamspeak-plugin-radiofx/src/config_radio_groupbox.h"
    "deps/teamspeak-plugin-radiofx/src/config_radio_groupbox.cpp"
    "deps/teamspeak-plugin-radiofx/src/config_radio_groupbox.ui"
    "deps/teamspeak-plugin-radiofx/src/dsp_radio.h"
    "deps/teamspeak-plugin-radiofx/src/dsp_radio.cpp"
    "deps/teamspeak-plugin-radiofx/src/mod_radio.h"
    "deps/teamspeak-plugin-radiofx/src/mod_radio.cpp"
    "deps/teamspeak-plugin-radiofx/src/plugin.h"
    "deps/teamspeak-plugin-radiofx/src/plugin.cpp"
    "deps/teamspeak-plugin-radiofx/src/plugin_qt.h"
    "deps/teamspeak-plugin-radiofx/src/plugin_qt.cpp"
    "deps/teamspeak-plugin-radiofx/src/settings_radio.h"
    "deps/teamspeak-plugin-radiofx/src/settings_radio.cpp"

    "deps/teamspeak-plugin-radiofx/radiofx.qrc"
)
source_group("plugin" FILES ${PLUGIN_SRC})

# Create named folders for the sources within the .vcproj
# Empty name lists them directly under the .vcproj
#source_group("" FILES ${PLUGIN_SRC})

# Turn on the ability to create folders to organize projects (.vcproj)
# It creates "CMakePredefinedTargets" folder by default and adds CMake
# defined projects like INSTALL.vcproj and ZERO_CHECK.vcproj
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# Set compiler flags and options. 
# Here it is setting the Visual Studio warning level to 4
# set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")

# Command to output information to the console
# Useful for displaying errors, warnings, and debugging
# message ("cxx Flags:" ${CMAKE_CXX_FLAGS})

# Sub-directories where more CMakeLists.txt exist
#add_subdirectory(app)
#add_subdirectory (math)
set(WITH_VOLUME true)
set(WITH_VOLUME_WIDGETS true)
#add_subdirectory(../teamspeak-plugin-qt-common common_out)
include(deps/teamspeak-plugin-radiofx/deps/teamspeak-plugin-qt-common/CMakeLists.txt)

add_library(RadioFx SHARED
    ${PLUGIN_SRC} ${TS_QT_CORE} ${TS_QT_VOLUME} ${DSP_FILTERS})
    
set_property(TARGET RadioFx PROPERTY CXX_STANDARD 17)
set_property(TARGET RadioFx PROPERTY CXX_STANDARD_REQUIRED ON)
