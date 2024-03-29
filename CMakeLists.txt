cmake_minimum_required(VERSION 3.0.2)
project(joint_group_ff_controllers)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

set(DISABLE_PAL_FLAGS True) # Prevent some compilation flags (e.g -Wshadow)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED COMPONENTS
  controller_interface
  control_msgs
  forward_command_controller
  realtime_tools
  pluginlib
  std_msgs
  message_generation
)

include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})

add_message_files( FILES
  setpoint.msg
)

generate_messages(DEPENDENCIES std_msgs)

# Declare catkin package
catkin_package(
  CATKIN_DEPENDS
    controller_interface
    control_msgs
    forward_command_controller
    realtime_tools
    message_runtime
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  )

add_library(${PROJECT_NAME}
  src/joint_group_effort_ff_controller.cpp
  src/joint_group_velocity_ff_controller.cpp
)

add_dependencies(${PROJECT_NAME} joint_group_ff_controllers_generate_messages_cpp)

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

install(FILES joint_group_ff_controllers_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
