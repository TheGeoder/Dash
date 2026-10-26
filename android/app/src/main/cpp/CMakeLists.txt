cmake_minimum_required(VERSION 3.22.1)

# Name your native rendering engine library for Dash
project("dash_core_engine")

# Look for Android's high-performance system libraries
find_library(log-lib log)             # Android system logging framework
find_library(android-lib android)     # Native Android window/storage abstraction layer
find_library(jnigraphics-lib jnigraphics) # Native Bitmap handling API for photo textures
find_library(gles-lib GLESv3)         # OpenGL ES 3.0+ graphics pipeline library
find_library(vulkan-lib vulkan)       # Vulkan graphics pipeline library

# Define your application's native C++ source files
add_library(${PROJECT_NAME} SHARED
        dash_jni_bridge.cpp           # The core Java/Kotlin communication bridge
        dash_database.cpp             # Embedded SQLite project execution engine
        dash_video_renderer.cpp       # Vulkan/GLES frame shader processing module
)

# Specify include directories for your header files
target_include_directories(${PROJECT_NAME} PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

# Link your Dash engine to the hardware APIs and system dependencies
target_link_libraries(${PROJECT_NAME}
        ${log-lib}
        ${android-lib}
        ${jnigraphics-lib}
        ${gles-lib}
        ${vulkan-lib}
        sqlite3                       # Android includes sqlite3 natively inside the platform system
)
