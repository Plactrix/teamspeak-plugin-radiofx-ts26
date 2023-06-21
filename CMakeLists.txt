cmake_minimum_required (VERSION 3.6)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

project (RadioFx LANGUAGES CXX)

find_package(Qt5 COMPONENTS Gui Widgets REQUIRED)
link_libraries(Qt5::Gui Qt5::Widgets)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

include_directories(
    "${CMAKE_CURRENT_LIST_DIR}/src"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/include"
)

set (DSP_FILTERS
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Bessel.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Biquad.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Butterworth.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Cascade.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/ChebyshevI.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/ChebyshevII.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Custom.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Design.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Documentation.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Elliptic.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Filter.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Legendre.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/Param.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/PoleFilter.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/RBJ.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/RootFinder.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/deps/DSPFilters/shared/DSPFilters/source/State.cpp"
)
source_group("DSPFilters" FILES ${DSP_FILTERS})

set (PLUGIN_SRC
    "${CMAKE_CURRENT_LIST_DIR}/src/config_radio.h"
    "${CMAKE_CURRENT_LIST_DIR}/src/config_radio.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/src/config_radio.ui"
    "${CMAKE_CURRENT_LIST_DIR}/src/config_radio_groupbox.h"
    "${CMAKE_CURRENT_LIST_DIR}/src/config_radio_groupbox.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/src/config_radio_groupbox.ui"
    "${CMAKE_CURRENT_LIST_DIR}/src/dsp_radio.h"
    "${CMAKE_CURRENT_LIST_DIR}/src/dsp_radio.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/src/mod_radio.h"
    "${CMAKE_CURRENT_LIST_DIR}/src/mod_radio.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/src/plugin.h"
    "${CMAKE_CURRENT_LIST_DIR}/src/plugin.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/src/plugin_qt.h"
    "${CMAKE_CURRENT_LIST_DIR}/src/plugin_qt.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/src/settings_radio.h"
    "${CMAKE_CURRENT_LIST_DIR}/src/settings_radio.cpp"

    "${CMAKE_CURRENT_LIST_DIR}/radiofx.qrc"
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
include(${CMAKE_CURRENT_LIST_DIR}/deps/teamspeak-plugin-qt-common/CMakeLists.txt)

add_library(RadioFx SHARED
    ${PLUGIN_SRC} ${TS_QT_CORE} ${TS_QT_VOLUME} ${DSP_FILTERS})
    
set_property(TARGET RadioFx PROPERTY CXX_STANDARD 17)
set_property(TARGET RadioFx PROPERTY CXX_STANDARD_REQUIRED ON)