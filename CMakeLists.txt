cmake_minimum_required(VERSION 2.8)
project(AntTweakBar)

set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)

option(BUILD_EXAMPLES "Build AntTweakBar examples" OFF)

include_directories(include)


################### Build the library ###################
if (APPLE)
    set (platform MACOSX)
elseif (UNIX)
    set (platform UNIX)
else()
    message(SEND_ERROR "This platform (${CMAKE_SYSTEM_NAME}) is not handled yet! Consider a pull-request.")
endif()

set(ANTWB_SOURCES
    src/TwColors.cpp
    src/TwFonts.cpp
    src/TwOpenGL.cpp
    src/TwOpenGLCore.cpp
    src/TwBar.cpp
    src/TwMgr.cpp
    src/TwPrecomp.cpp
    src/LoadOGL.cpp
    src/LoadOGLCore.cpp
    src/TwEventGLFW.c
    src/TwEventGLUT.c
    src/TwEventSDL.c
    src/TwEventSDL12.c
    src/TwEventSDL13.c
    src/TwEventSFML.cpp
    src/TwEventX11.c
)
add_definitions(-Wall -fPIC -fno-strict-aliasing -D_${platform} -D__PLACEMENT_NEW_INLINE)
include_directories(${CMAKE_SOURCE_DIR}/../include)

add_library(AntTweakBar SHARED ${ANTWB_SOURCES})


if (BUILD_EXAMPLES)
    set (LIBS AntTweakBar GL GLU X11 Xext Xmu Xrandr pthread m)

    link_directories(${LIBRARY_OUTPUT_PATH})

    add_executable(TwSimpleGLUT examples/TwSimpleGLUT.c)
    target_link_libraries(TwSimpleGLUT ${LIBS} glut)

    add_executable(TwSimpleSDL examples/TwSimpleSDL.c)
    target_link_libraries(TwSimpleSDL ${LIBS} SDL)

    add_executable(TwSimpleGLFW examples/TwSimpleGLFW.c)
    target_link_libraries(TwSimpleGLFW ${LIBS} glfw)

    add_executable(TwDualGLUT examples/TwDualGLUT.c)
    target_link_libraries(TwDualGLUT ${LIBS} glut)

    add_executable(TwAdvanced1 examples/TwAdvanced1.cpp)
    target_link_libraries(TwAdvanced1 ${LIBS} glfw)

    add_executable(TwString examples/TwString.cpp)
    target_link_libraries(TwString ${LIBS} glut)
endif()

