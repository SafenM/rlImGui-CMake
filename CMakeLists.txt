cmake_minimum_required(VERSION 3.10)
project(rlImGuiProject)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add raylib
include(FetchContent)
FetchContent_Declare(
  raylib
  GIT_REPOSITORY https://github.com/raysan5/raylib.git
  GIT_TAG master
)
FetchContent_MakeAvailable(raylib)

# Add ImGui
FetchContent_Declare(
  imgui
  GIT_REPOSITORY https://github.com/ocornut/imgui.git
  GIT_TAG docking
)
FetchContent_MakeAvailable(imgui)

# Define sources and headers
file(GLOB_RECURSE SOURCES "rlImGui.cpp")
file(GLOB_RECURSE HEADERS "rlImGui.h" "extras/*.h")

# Add ImGui source files
set(IMGUI_SOURCES
    ${imgui_SOURCE_DIR}/imgui.cpp
    ${imgui_SOURCE_DIR}/imgui_demo.cpp
    ${imgui_SOURCE_DIR}/imgui_draw.cpp
    ${imgui_SOURCE_DIR}/imgui_tables.cpp
    ${imgui_SOURCE_DIR}/imgui_widgets.cpp
)

# Create library
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
if(BUILD_SHARED_LIBS)
  add_library(rlImGui SHARED ${SOURCES} ${HEADERS} ${IMGUI_SOURCES})
else()
  add_library(rlImGui STATIC ${SOURCES} ${HEADERS} ${IMGUI_SOURCES})
endif()

# Link libraries
target_link_libraries(rlImGui raylib)

# Define preprocessor macros
target_compile_definitions(rlImGui PRIVATE IMGUI_DISABLE_OBSOLETE_FUNCTIONS IMGUI_DISABLE_OBSOLETE_KEYIO)

# Export include directories
target_include_directories(rlImGui PUBLIC
    ${raylib_SOURCE_DIR}/src
    ${imgui_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
)
