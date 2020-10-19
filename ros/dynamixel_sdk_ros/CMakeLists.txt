################################################################################
# Set minimum required version of cmake, project name and compile options
################################################################################
cmake_minimum_required(VERSION 2.8.3)
project(dynamixel_sdk_ros)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

################################################################################
# Find catkin packages and libraries for catkin and system dependencies
################################################################################
find_package(catkin REQUIRED COMPONENTS roscpp rospy)

################################################################################
# Setup for python modules and scripts
################################################################################
catkin_python_setup()

################################################################################
# Declare ROS messages, services and actions
################################################################################

################################################################################
# Declare ROS dynamic reconfigure parameters
################################################################################

################################################################################
# Declare catkin specific configuration to be passed to dependent projects
################################################################################
catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp rospy
  LIBRARIES ${PROJECT_NAME}
)

################################################################################
# Build
################################################################################
include_directories(
  include/${PROJECT_NAME}
  ${catkin_INCLUDE_DIRS}
)

if(APPLE)
  add_library(${PROJECT_NAME}
    src/${PROJECT_NAME}/packet_handler.cpp
    src/${PROJECT_NAME}/protocol1_packet_handler.cpp
    src/${PROJECT_NAME}/protocol2_packet_handler.cpp
    src/${PROJECT_NAME}/group_sync_read.cpp
    src/${PROJECT_NAME}/group_sync_write.cpp
    src/${PROJECT_NAME}/group_bulk_read.cpp
    src/${PROJECT_NAME}/group_bulk_write.cpp
    src/${PROJECT_NAME}/port_handler.cpp
    src/${PROJECT_NAME}/port_handler_mac.cpp
  )
else()
  add_library(${PROJECT_NAME}
    src/${PROJECT_NAME}/packet_handler.cpp
    src/${PROJECT_NAME}/protocol1_packet_handler.cpp
    src/${PROJECT_NAME}/protocol2_packet_handler.cpp
    src/${PROJECT_NAME}/group_sync_read.cpp
    src/${PROJECT_NAME}/group_sync_write.cpp
    src/${PROJECT_NAME}/group_bulk_read.cpp
    src/${PROJECT_NAME}/group_bulk_write.cpp
    src/${PROJECT_NAME}/port_handler.cpp
    src/${PROJECT_NAME}/port_handler_linux.cpp
  )
endif()

add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

################################################################################
# Install
################################################################################
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

catkin_install_python(PROGRAMS
  src/${PROJECT_NAME}/group_bulk_read.py
  src/${PROJECT_NAME}/group_bulk_write.py
  src/${PROJECT_NAME}/group_sync_read.py
  src/${PROJECT_NAME}/group_sync_write.py
  src/${PROJECT_NAME}/packet_handler.py
  src/${PROJECT_NAME}/port_handler.py
  src/${PROJECT_NAME}/protocol1_packet_handler.py
  src/${PROJECT_NAME}/protocol2_packet_handler.py
  src/${PROJECT_NAME}/robotis_def.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

################################################################################
# Test
################################################################################
