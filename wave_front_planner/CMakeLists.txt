cmake_minimum_required(VERSION 2.8.3)
project(wave_front_planner)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(catkin REQUIRED COMPONENTS
  mbf_mesh_core
  mbf_utility
  mesh_map
)

find_package(PkgConfig REQUIRED)
pkg_check_modules(JSONCPP jsoncpp)

generate_dynamic_reconfigure_options(
  cfg/WaveFrontPlanner.cfg
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES mesh_planner
  CATKIN_DEPENDS mbf_mesh_core mesh_map
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}
  src/wave_front_planner.cpp
)

add_dependencies(${PROJECT_NAME}
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
  ${PROJECT_NAME}_gencfg
)

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  ${JSONCPP_LIBRARIES}
)

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(FILES wave_front_planner.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

