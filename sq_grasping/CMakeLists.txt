cmake_minimum_required(VERSION 2.8.3)
project(sq_grasping)


find_package(catkin REQUIRED COMPONENTS sq_fitting grasp_execution cmake_modules roscpp message_generation geometry_msgs )


find_package(Boost REQUIRED COMPONENTS system)


## Generate services in the 'srv' folder
 add_service_files(
   FILES
   getGrasps.srv

 )

generate_messages(DEPENDENCIES geometry_msgs std_msgs grasp_execution)

catkin_package(
  INCLUDE_DIRS
      include
  LIBRARIES
      sq_grasping
  CATKIN_DEPENDS roscpp
  DEPENDS system_lib
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
)


add_library(sq_grasping  src/sq_grasping/sq_grasping.cpp)

target_link_libraries(sq_grasping ${catkin_LIBRARIES})

install(TARGETS sq_grasping
 ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
 LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)


add_executable(sq_grasping_node src/nodes/sq_grasping_node.cpp)
add_dependencies(sq_grasping_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(sq_grasping_node sq_grasping  ${catkin_LIBRARIES})

add_executable(execute_grasps_node src/nodes/execute_grasps_node.cpp)
add_dependencies(execute_grasps_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(execute_grasps_node sq_grasping  ${catkin_LIBRARIES})
