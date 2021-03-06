cmake_minimum_required(VERSION 2.8.3)
project(controller)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS roslib roscpp rospy dynamic_reconfigure geometry_msgs sensor_msgs nav_msgs tf genmsg actionlib_msgs actionlib message_generation )

## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED COMPONENTS system thread)

## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()


generate_dynamic_reconfigure_options(
  cfg/PIDConstant.cfg
)

add_service_files(
  DIRECTORY srv
  FILES
  drive_x.srv
)
#add_action_files(DIRECTORY action FILES BarrelRoll.action)
generate_messages(DEPENDENCIES std_msgs)

include_directories(include ${catkin_INCLUDE_DIRS})

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
    INCLUDE_DIRS include
    LIBRARIES
    CATKIN_DEPENDS roslib roscpp dynamic_reconfigure geometry_msgs sensor_msgs nav_msgs std_msgs  tf message_runtime
    DEPENDS
)

###########
## Build ##
###########



add_executable(Controller src/Controller.cpp)
add_dependencies(Controller ${PROJECT_NAME}_gencfg controller_gencpp)
target_link_libraries(Controller ${catkin_LIBRARIES})

# add_executable(TranformFrameBroadcaster src/tf-broadcaster.cpp)
# target_link_libraries(TranformFrameBroadcaster ${catkin_LIBRARIES})

# add_executable(TranformFrameListener src/tf-listener.cpp)
# target_link_libraries(TranformFrameListener ${catkin_LIBRARIES})

# add_library(hector_quadrotor_motor_controller src/motor_controller.cpp)
# target_link_libraries(hector_quadrotor_motor_controller hector_quadrotor_controller)

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executables and/or libraries for installation
# install(TARGETS hector_quadrotor_controller hector_quadrotor_pose_controller hector_quadrotor_twist_controller hector_quadrotor_motor_controller
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(DIRECTORY launch params DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
# install(FILES
#   plugin.xml
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )
