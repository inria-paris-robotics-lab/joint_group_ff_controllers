cmake_minimum_required(VERSION 3.0.2)
project(custom_effort_controllers)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

set(DISABLE_PAL_FLAGS True) # Prevent some compilation flags (e.g -Wshadow)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED COMPONENTS
  angles
  controller_interface
  control_msgs
  control_toolbox
  forward_command_controller
  realtime_tools
  urdf
  pluginlib
)

include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})

# Declare catkin package
catkin_package(
  CATKIN_DEPENDS
    angles
    controller_interface
    control_msgs
    control_toolbox
    forward_command_controller
    realtime_tools
    urdf
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  )

add_library(${PROJECT_NAME}
  src/joint_group_position_controller.cpp
)

target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

# Install
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

# Install library
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
  )

install(FILES custom_effort_controllers_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})