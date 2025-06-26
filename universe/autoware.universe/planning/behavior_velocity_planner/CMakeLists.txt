cmake_minimum_required(VERSION 3.14)
project(behavior_velocity_planner)

find_package(autoware_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(grid_map_core REQUIRED) #KMS_250406

rosidl_generate_interfaces(
  ${PROJECT_NAME}
  "srv/LoadPlugin.srv"
  "srv/UnloadPlugin.srv"
  DEPENDENCIES
)

autoware_package()

ament_auto_add_library(${PROJECT_NAME}_lib SHARED
  src/node.cpp
  src/planner_manager.cpp
)

rclcpp_components_register_node(${PROJECT_NAME}_lib
  PLUGIN "behavior_velocity_planner::BehaviorVelocityPlannerNode"
  EXECUTABLE ${PROJECT_NAME}_node
)

# grid_map_core 헤더 디렉토리 명시적으로 추가
target_include_directories(${PROJECT_NAME}_lib
  PRIVATE
  ${grid_map_core_INCLUDE_DIRS}
  /opt/ros/humble/include/grid_map_core  #KMS_250406 [반드시 grid_map_core가 시작되는 경로를 포함해야 함]
)



if(${rosidl_cmake_VERSION} VERSION_LESS 2.5.0)
    rosidl_target_interfaces(${PROJECT_NAME}_lib
    ${PROJECT_NAME} "rosidl_typesupport_cpp")
else()
    rosidl_get_typesupport_target(
            cpp_typesupport_target ${PROJECT_NAME} "rosidl_typesupport_cpp")
    target_link_libraries(${PROJECT_NAME}_lib "${cpp_typesupport_target}")
endif()

if(BUILD_TESTING)
  ament_add_ros_isolated_gtest(test_${PROJECT_NAME}
    test/src/test_node_interface.cpp
  )
  target_link_libraries(test_${PROJECT_NAME}
    gtest_main
    ${PROJECT_NAME}_lib
  )
  target_include_directories(test_${PROJECT_NAME}
    PRIVATE
    src
    /opt/ros/humble/include/grid_map_core  #KMS_250406 테스트 타깃에 grid_map_core 경로 추가
  )
endif()


ament_auto_package(INSTALL_TO_SHARE
  launch
  config
)
