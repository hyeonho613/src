cmake_minimum_required(VERSION 3.14)  #KMS_250317
project(behavior_path_planner_common)  #KMS_250317

find_package(autoware_cmake REQUIRED)  #KMS_250317
autoware_package()  #KMS_250317

find_package(OpenCV REQUIRED)  #KMS_250317
find_package(magic_enum CONFIG REQUIRED)  #KMS_250317
find_package(Eigen3 REQUIRED)  #KMS_250317

ament_auto_add_library(${PROJECT_NAME} SHARED
  src/turn_signal_decider.cpp                    #KMS_250317
  src/interface/steering_factor_interface.cpp      #KMS_250317
  src/interface/scene_module_visitor.cpp           #KMS_250317
  src/utils/utils.cpp                              #KMS_250317
  src/utils/path_utils.cpp                         #KMS_250317
  src/utils/traffic_light_utils.cpp                #KMS_250317
  src/utils/path_safety_checker/safety_check.cpp     #KMS_250317
  src/utils/path_safety_checker/objects_filtering.cpp  #KMS_250317
  src/utils/path_shifter/path_shifter.cpp          #KMS_250317
  src/utils/drivable_area_expansion/static_drivable_area.cpp       #KMS_250317
  src/utils/drivable_area_expansion/drivable_area_expansion.cpp      #KMS_250317
  src/utils/drivable_area_expansion/map_utils.cpp    #KMS_250317
  src/utils/drivable_area_expansion/footprints.cpp     #KMS_250317
  src/utils/parking_departure/geometric_parallel_parking.cpp  #KMS_250317
  src/utils/parking_departure/utils.cpp             #KMS_250317
  src/utils/occupancy_grid_based_collision_detector/occupancy_grid_based_collision_detector.cpp  #KMS_250317
  src/marker_utils/utils.cpp                         #KMS_250317
)  #KMS_250317

target_include_directories(${PROJECT_NAME} SYSTEM PUBLIC
  ${EIGEN3_INCLUDE_DIR}  #KMS_250317
)  #KMS_250317

target_link_libraries(${PROJECT_NAME}
  ${OpenCV_LIBRARIES}      #KMS_250317
  magic_enum::magic_enum   #KMS_250317
)  #KMS_250317

if(BUILD_TESTING)  #KMS_250317
  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_utilities
    test/test_drivable_area_expansion.cpp  #KMS_250317
  )  #KMS_250317

  target_link_libraries(test_${PROJECT_NAME}_utilities
    ${PROJECT_NAME}  #KMS_250317
  )  #KMS_250317

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_safety_check
    test/test_safety_check.cpp  #KMS_250317
  )  #KMS_250317

  target_link_libraries(test_${PROJECT_NAME}_safety_check
    ${PROJECT_NAME}  #KMS_250317
  )  #KMS_250317

  ament_add_ros_isolated_gmock(test_${PROJECT_NAME}_turn_signal
    test/test_turn_signal.cpp  #KMS_250317
  )  #KMS_250317

  target_link_libraries(test_${PROJECT_NAME}_turn_signal
    ${PROJECT_NAME}  #KMS_250317
  )  #KMS_250317
endif()  #KMS_250317

ament_auto_package()  #KMS_250317
