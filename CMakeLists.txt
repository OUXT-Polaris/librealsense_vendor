cmake_minimum_required(VERSION 3.5)
project(librealsense_vendor)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)

macro(build_librealsense)
  set(cmake_commands)
  set(cmake_configure_args -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_TOOLS=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_EXAMPLES=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_GLSL_EXTENSIONS=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_EASYLOGGINGPP=false)
  set(cmake_configure_args ${cmake_configure_args};-DIMPORT_DEPTH_CAM_FW=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_WITH_TM2=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_GRAPHICAL_EXAMPLES=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_EXAMPLES=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_WITH_STATIC_CRT=false)
  set(cmake_configure_args ${cmake_configure_args};-DHWM_OVER_XU=false)
  set(cmake_configure_args ${cmake_configure_args};-DENABLE_CCACHE=false)
  set(cmake_configure_args ${cmake_configure_args};-DBUILD_SHARED_LIBS=true)

  include(ExternalProject)
  externalproject_add(librealsense-ext
    GIT_REPOSITORY https://github.com/IntelRealSense/librealsense.git
    GIT_TAG master
    TIMEOUT 6000
    ${cmake_commands}
    CMAKE_ARGS
      ${cmake_configure_args}
  )
  install(
    DIRECTORY
      ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install/
    DESTINATION
      ${CMAKE_INSTALL_PREFIX}
  )
endmacro()

build_librealsense()
ament_export_include_directories(include)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
