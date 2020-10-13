# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             #ffmpeg_hello.cpp
             native-lib.cpp )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

set(PROJ_PLATFORM armeabi-v7a)
set(LIBS_PATH ${CMAKE_SOURCE_DIR}/libs/${PROJ_PLATFORM})
set(FF_INC ${LIBS_PATH}/ffmpeg/include)
set(FF_LIB ${LIBS_PATH}/ffmpeg/lib)

include_directories(${FF_INC})

add_library(clang_rt.ubsan_standalone-arm-android SHARED IMPORTED)
add_library(postproc SHARED IMPORTED)
add_library(avcodec SHARED IMPORTED)
add_library(avfilter SHARED IMPORTED)
add_library(avformat SHARED IMPORTED)
add_library(avutil SHARED IMPORTED)
add_library(swresample SHARED IMPORTED)
add_library(swscale SHARED IMPORTED)

set_target_properties(clang_rt.ubsan_standalone-arm-android
        PROPERTIES IMPORTED_LOCATION
        ${LIBS_PATH}/libclang_rt.ubsan_standalone-arm-android.so)
set_target_properties(postproc PROPERTIES IMPORTED_LOCATION ${FF_LIB}/libpostproc.so)
set_target_properties(avcodec PROPERTIES IMPORTED_LOCATION ${FF_LIB}/libavcodec.so)
set_target_properties(avfilter PROPERTIES IMPORTED_LOCATION ${FF_LIB}/libavfilter.so)
set_target_properties(avformat PROPERTIES IMPORTED_LOCATION ${FF_LIB}/libavformat.so)
set_target_properties(avutil PROPERTIES IMPORTED_LOCATION ${FF_LIB}/libavutil.so)
set_target_properties(swresample PROPERTIES IMPORTED_LOCATION ${FF_LIB}/libswresample.so)
set_target_properties(swscale PROPERTIES IMPORTED_LOCATION ${FF_LIB}/libswscale.so)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       native-lib

                       clang_rt.ubsan_standalone-arm-android

                       # Specifies ffmpeg libraries.
                       postproc
                       avcodec
                       avfilter
                       avformat
                       avutil
                       swresample
                       swscale

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )