set(Python_EXECUTABLE ".venv/Scripts/python.exe")
cmake_minimum_required(VERSION 3.15...3.27)
project(consgfx LANGUAGES CXX)

# Try to import all Python components potentially needed by nanobind
find_package(Python 3.8
  REQUIRED COMPONENTS Interpreter Development.Module
  OPTIONAL_COMPONENTS Development.SABIModule)

# Import nanobind through CMake's find_package mechanism
find_package(nanobind CONFIG REQUIRED)

nanobind_add_module(consgfx_ext
  NB_STATIC
  STABLE_ABI

  src/consgfx_ext.cpp
#   src/c/imgui/imgui.cpp
#   src/c/imgui/imgui_demo.cpp
#   src/c/imgui/imgui_draw.cpp
#   src/c/imgui/imgui_tables.cpp
#   src/c/imgui/imgui_widgets.cpp
)

nanobind_add_stub(
  consgfx_ext_stub
  MODULE consgfx_ext
  OUTPUT "${CMAKE_SOURCE_DIR}/src/consgfx/consgfx_ext.pyi"
  PYTHON_PATH $<TARGET_FILE_DIR:consgfx_ext>
  DEPENDS consgfx_ext
  INCLUDE_PRIVATE
)

#include_directories(${CMAKE_SOURCE_DIR}/src/c/imgui)

install(TARGETS consgfx_ext LIBRARY DESTINATION consgfx)
install(FILES "${CMAKE_SOURCE_DIR}/src/consgfx/consgfx_ext.pyi" DESTINATION consgfx)