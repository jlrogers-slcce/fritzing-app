cmake_minimum_required(VERSION 3.24)
project(Fritzing)

# Qt6 Configuration
set(CMAKE_PREFIX_PATH ${Qt6_PREFIX_PATH})
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)
set(QT_VERSION_MINIMUM 6.5.3)

find_package(Qt6 COMPONENTS 
    Core 
    Gui 
    Widgets 
    Network 
    Sql 
    Svg 
    SerialPort 
    PrintSupport 
    Xml
    REQUIRED
)

# Find other dependencies
find_package(Git REQUIRED)
find_package(quazip1-qt6 REQUIRED)
find_package(OpenSSL REQUIRED)

# Version detection from git
execute_process(
    COMMAND ${GIT_EXECUTABLE} describe --tags
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    OUTPUT_VARIABLE FRITZING_VERSION
    OUTPUT_STRIP_TRAILING_WHITESPACE
)

# Source files
file(GLOB_RECURSE SOURCES "src/*.cpp")
file(GLOB_RECURSE HEADERS "src/*.h")
file(GLOB_RECURSE RESOURCES "phoenixresources.qrc" "resources/*.xml")

add_executable(Fritzing WIN32 MACOSX_BUNDLE)

target_sources(Fritzing PRIVATE
    ${SOURCES}
    ${HEADERS}
    ${RESOURCES}
)

target_include_directories(Fritzing PRIVATE
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_SOURCE_DIR}
)

target_link_libraries(Fritzing PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Widgets
    Qt6::Network
    Qt6::Sql
    Qt6::Svg
    Qt6::SerialPort
    Qt6::PrintSupport
    Qt6::Xml
    quazip1-qt6
    OpenSSL::SSL
    OpenSSL::Crypto
)

# Windows specific settings
if(WIN32)
    set_target_properties(Fritzing PROPERTIES
        WIN32_EXECUTABLE ON
    )
    if(MSVC)
        target_compile_options(Fritzing PRIVATE /std:c++17 /permissive-)
    else()
        target_compile_options(Fritzing PRIVATE -std=c++17)
    endif()
endif()

# Installation
install(TARGETS Fritzing DESTINATION bin)
