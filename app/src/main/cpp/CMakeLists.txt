# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

#if(NOT ${CMAKE_VERSION} LESS 3.2)
#    set(CMAKE_CXX_STANDARD 11)
#    set(CMAKE_CXX_STANDARD_REQUIRED ON)
#else()
#    message(STATUS "Checking compiler flags for C++11 support.")
#    # Set C++11 support flags for various compilers
#    include(CheckCXXCompilerFlag)
#    check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
#    check_cxx_compiler_flag("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
#    if(COMPILER_SUPPORTS_CXX11)
#        message(STATUS "C++11 is supported.")
#        if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
#            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -stdlib=libc++")
#        else()
#            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
#        endif()
#    elseif(COMPILER_SUPPORTS_CXX0X)
#        message(STATUS "C++0x is supported.")
#        if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
#            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x -stdlib=libc++")
#        else()
#            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
#        endif()
#    else()
#        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
#    endif()
#endif()

add_library( # Sets the name of the library.
        native-lib

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        native-lib.cpp)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.
set(OPENSSL_ROOT_DIR /usr/local/opt/openssl@1.1)
set(OPENSSL_SSL_LIBRARY /usr/local/opt/openssl@1.1/lib)
set(OPENSSL_CRYPTO_LIBRARY /usr/local/opt/openssl@1.1/lib)
set(OPENSSL_INCLUDE_DIR /usr/local/opt/openssl@1.1/include)
add_subdirectory(opt)

#add_executable(example example.cpp)
target_link_libraries(native-lib ${CPR_LIBRARIES})
include_directories(${CPR_INCLUDE_DIRS} ${JSON_INCLUDE_DIRS})

target_link_libraries( # Specifies the target library.
        native-lib

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})