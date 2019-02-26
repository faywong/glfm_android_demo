cmake_minimum_required(VERSION 3.6.0) # Might run on earlier versions. Probably requires 3.4 or 3.5

# build native_app_glue as a static lib
set(${CMAKE_C_FLAGS}, "${CMAKE_C_FLAGS}")
add_library(native_app_glue STATIC
    ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c)

# now build app's shared lib
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wall -Werror")

# Export ANativeActivity_onCreate(),
# Refer to: https://github.com/android-ndk/ndk/issues/381.
set(CMAKE_SHARED_LINKER_FLAGS
    "${CMAKE_SHARED_LINKER_FLAGS} -u ANativeActivity_onCreate")

add_library(glfm SHARED src/main/cpp/glfm_platform_android.c)

target_include_directories(glfm PRIVATE
    ${ANDROID_NDK}/sources/android/native_app_glue
    src/main/cpp/include)

target_link_libraries(glfm
    android
    native_app_glue
    EGL
    GLESv2
    log)