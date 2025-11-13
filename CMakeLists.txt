cmake_minimum_required(VERSION 3.23.1)

set(PROJECT_NAME "RedivoxTest")
set(CURRENT_VERSION "0.0.1")
set(PRODUCT_NAME "Redivox")
set(COMPANY_NAME "Feedforfools")

project(${PROJECT_NAME} VERSION ${CURRENT_VERSION})
add_subdirectory(modules/JUCE)
set(CMAKE_CXX_STANDARD 20)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/modules/cmake-includes/sudara-cmake")

juce_add_console_app("${PROJECT_NAME}"
    COMPANY_NAME "${COMPANY_NAME}"
    PRODUCT_NAME "${PRODUCT_NAME}")

add_compile_definitions(JUCE_PROJECT_NAME="${PROJECT_NAME}")

# `target_sources` adds source files to a target. We pass the target that needs the sources as the
# first argument, then a visibility parameter for the sources which should normally be PRIVATE.
# Finally, we supply a list of source files that will be built into the target. This is a standard
# CMake command.
target_sources(${PROJECT_NAME}
    PRIVATE
        src/main.cpp)

# `target_compile_definitions` adds some preprocessor definitions to our target. In a Projucer
# project, these might be passed in the 'Preprocessor Definitions' field. JUCE modules also make use
# of compile definitions to switch certain features on/off, so if there's a particular feature you
# need that's not on by default, check the module header for the correct flag to set here. These
# definitions will be visible both to your code, and also the JUCE module code, so for new
# definitions, pick unique names that are unlikely to collide! This is a standard CMake command.

target_compile_definitions(${PROJECT_NAME}
    PRIVATE
        # JUCE_WEB_BROWSER and JUCE_USE_CURL would be on by default, but you might not need them.
        JUCE_WEB_BROWSER=0  # If you remove this, add `NEEDS_WEB_BROWSER TRUE` to the `juce_add_console_app` call
        JUCE_USE_CURL=0)    # If you remove this, add `NEEDS_CURL TRUE` to the `juce_add_console_app` call

# If the target needs extra binary assets, they can be added here. The first argument is the name of
# a new static library target that will include all the binary resources. There is an optional
# `NAMESPACE` argument that can specify the namespace of the generated binary data class. Finally,
# the SOURCES argument should be followed by a list of source files that should be built into the
# static library. These source files can be of any kind (wav data, images, fonts, icons etc.).
# Conversion to binary-data will happen when the target is built.

# juce_add_binary_data(ConsoleAppData SOURCES ...)

# `target_link_libraries` links libraries and JUCE modules to other libraries or executables. Here,
# we're linking our executable target to the `juce::juce_core` module. Inter-module dependencies are
# resolved automatically. If you'd generated a binary data target above, you would need to link to
# it here too. This is a standard CMake command.

target_link_libraries(${PROJECT_NAME}
    PRIVATE
        juce::juce_core
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_warning_flags)

target_include_directories("${PROJECT_NAME}" PRIVATE
    "${CMAKE_CURRENT_SOURCE_DIR}/src")
