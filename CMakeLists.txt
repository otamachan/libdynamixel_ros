cmake_minimum_required(VERSION 2.8.3)
project(libdynamixel_ros)

add_compile_options(-std=c++11)

find_package(catkin REQUIRED)

find_package(Boost REQUIRED COMPONENTS program_options)

catkin_package(
  INCLUDE_DIRS libdynamixel/src
  DEPENDS Boost
  )

include_directories(
  ${Boost_INCLUDE_DIR}
  )

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE libdynamixel/src)

add_executable(${PROJECT_NAME}_tool libdynamixel/src/tools/dynamixel.cpp)
target_compile_definitions(${PROJECT_NAME}_tool PRIVATE PROTOCOL1)
target_link_libraries(${PROJECT_NAME}_tool LINK_PUBLIC ${Boost_LIBRARIES} ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME}_tool PROPERTIES OUTPUT_NAME dynamixel_tool PREFIX "")

install(TARGETS ${PROJECT_NAME}_tool
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY libdynamixel/src/dynamixel
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)
