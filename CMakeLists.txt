cmake_minimum_required(VERSION 2.8.3)
project(robot_localization_utils)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  robot_localization
  tf2
  tf
  tf2_ros
  tf2_geometry_msgs
  message_filters
  std_msgs
  std_srvs
  geographic_msgs
  geometry_msgs
  nav_msgs
  sensor_msgs
  robotnik_msgs
  mavros_msgs
  robot_localization
)
find_package(Eigen REQUIRED)

###################################
## catkin specific configuration ##
###################################

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS
    robot_localization
    roscpp
    tf
    tf2
    tf2_ros
    message_filters
    std_msgs
    std_srvs
    geometry_msgs
    nav_msgs
    sensor_msgs
    robotnik_msgs
    mavros_msgs
  DEPENDS Eigen
)

###########
## Build ##
###########

include_directories(include   
		    ${catkin_INCLUDE_DIRS}
		    ${Eigen_INCLUDE_DIRS})

add_executable(rl_utils src/robot_localization_utils.cpp)
add_dependencies(rl_utils robotnik_msgs_generate_messages_cpp)
target_link_libraries(rl_utils ${catkin_LIBRARIES})

# Library definition
add_library(navsat_transform_new src/navsat_transform_new.cpp)
# Dependencies
add_dependencies(navsat_transform_new robot_localization_gencpp)

# Executables
add_executable(navsat_transform_new_node src/navsat_transform_new_node.cpp)

# Linking
target_link_libraries(navsat_transform_new ${catkin_LIBRARIES})
target_link_libraries(navsat_transform_new_node navsat_transform_new ${catkin_LIBRARIES})

install(
  TARGETS rl_utils
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  TARGETS navsat_transform_new_node
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  TARGETS navsat_transform_new
  DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(
  DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
