cmake_minimum_required(VERSION 2.8.3)
project(supervisor)

include(FindPkgConfig)
pkg_check_modules(CURLPP REQUIRED curlpp)

set( CMAKE_CXX_STANDARD 11 )
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  nav_msgs
  roscpp
  std_msgs
  tf
  behavior_tree_core
  room_analysis
  message_generation
  roslib
)

find_package(Boost)


###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need

add_service_files(
  FILES
  SendRule.srv
)

generate_messages()

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES
  CATKIN_DEPENDS
    geometry_msgs
    nav_msgs roscpp
    std_msgs
    tf
    behavior_tree_cpp
  DEPENDS
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

# add_executable(fake_navigation tests/fake_navigation.cpp )
# target_link_libraries(fake_navigation  ${catkin_LIBRARIES} )

#add_executable(fake_manipulation tests/fake_manipulation.cpp )
#target_link_libraries(fake_manipulation  ${catkin_LIBRARIES} )

add_executable(supervisor
    src/supervisor.cpp
    src/supervisor_main.cpp
    src/movebase_action.cpp
    src/collectwaypoints_action.cpp
    src/explore_action.cpp
    )
add_dependencies(supervisor room_analysis_generate_messages_cpp)
target_link_libraries(supervisor ${catkin_LIBRARIES} ${CURLPP_LDFLAGS})

add_executable(test_supervisor src/test_supervisor_main.cpp)
target_link_libraries(test_supervisor ${catkin_LIBRARIES} ${CURLPP_LDFLAGS})

add_executable(simple_action_supervisor
    src/full_supervisor.cpp
    src/full_supervisor_main.cpp
    )
add_dependencies(simple_action_supervisor room_analysis_generate_messages_cpp)
target_link_libraries(simple_action_supervisor ${catkin_LIBRARIES} ${CURLPP_LDFLAGS})

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS ${PROJECT_NAME} fake_navigation
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_fake_navigation.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
