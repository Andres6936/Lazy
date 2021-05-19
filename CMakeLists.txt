CMAKE_MINIMUM_REQUIRED(VERSION 3.14)

PROJECT(Lazy)

### Support to Command <make install>

# Must use GNUInstallDirs to install libraries into correct
# locations on all platforms.
INCLUDE(GNUInstallDirs)

#[[ This sets an header-only project.

Don't worry, no actual binaries will be generated if you should try and build it.
The command <SET_TARGET_PROPERTIES> is there because CMake will otherwise complain
that it cannot infer the target language from .hpp files only.
]]
ADD_LIBRARY(Lazy INTERFACE)
# Define headers for this library. PUBLIC headers are used for
# compiling the library, and will be added to consumers' build
# paths.
TARGET_INCLUDE_DIRECTORIES(Lazy INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
        )

# Support to compiler on mode C++ 17 standard.
TARGET_COMPILE_FEATURES(Lazy INTERFACE cxx_std_17)

FIND_PACKAGE(Threads REQUIRED)
# Link the library with libpthread
TARGET_LINK_LIBRARIES(Lazy INTERFACE Threads::Threads)

# 'make install' to the correct locations (provided by GNUInstallDirs).
INSTALL(TARGETS Lazy
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        )

# This is for install the headers in correct location
INSTALL(DIRECTORY ${PROJECT_SOURCE_DIR}/Include/ DESTINATION include)

ADD_EXECUTABLE(Lazy.Example.1 Example/example-1.cc)
TARGET_LINK_LIBRARIES(Lazy.Example.1 PRIVATE Lazy)

ADD_EXECUTABLE(Lazy.Example.2 Example/example-2.cc)
TARGET_LINK_LIBRARIES(Lazy.Example.2 PRIVATE Lazy)

ADD_EXECUTABLE(Lazy.Example.3 Example/example-3.cc)
TARGET_LINK_LIBRARIES(Lazy.Example.3 PRIVATE Lazy)