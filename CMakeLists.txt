cmake_minimum_required(VERSION 3.22)

project(zBot VERSION 3.0.1 LANGUAGES CXX)

find_package(Geode REQUIRED)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

file(GLOB SOURCES
    src/framecounting.cpp
    src/gui.cpp
    src/keybinds.cpp
    src/physicsbypass.cpp
    src/playbackmanager.cpp
    src/recordmanager.cpp
    src/speedhackaudio.cpp
    src/clickbetweenframescompat.cpp
    src/zClickbot/clickbot.cpp
    src/zKeyChecker/keychecker.cpp
)

add_geode_mod(${PROJECT_NAME} ${SOURCES})

target_include_directories(${PROJECT_NAME} PRIVATE src)
target_compile_definitions(${PROJECT_NAME} PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

# Memasukkan library replay format external
CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")

if (GEODE_TARGET_PLATFORM_BINARY EQUAL 1) # Windows
    CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")
    target_link_libraries(${PROJECT_NAME} imgui-cocos libGDR)
elseif (GEODE_TARGET_PLATFORM_BINARY EQUAL 2) # MacOS
    CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")
    target_link_libraries(${PROJECT_NAME} imgui-cocos libGDR "-framework IOKit")
else() # Android & iOS (Menggunakan ImGui bawaan Geode SDK 5+)
    target_link_libraries(${PROJECT_NAME} libGDR)
endif()
