cmake_minimum_required(VERSION 2.8.3)
project(trajectory_generator)

#SET(CMAKE_BUILD_TYPE Release)
SET(CMAKE_BUILD_TYPE Debug)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
        roscpp
        std_msgs
        message_generation
        )

catkin_package(
        INCLUDE_DIRS include
        CATKIN_DEPENDS roscpp std_msgs
)

#add_message_files(
#        FILES
# put new *.msg files here
#)

include_directories( #TODO delete lms stuff
        include
        include/trajectory_generator
        external/services/phoenix_CC2016_service/include
        external/libraries/math_lib/include
        external/libraries/street_environment/include
        external/libraries/lms/include
        external/libraries/conan_cereal/include
        external/libraries/cereal/
        external/libraries/OLDtrajectory_creator/include
        external/libraries
        ${catkin_INCLUDE_DIRS}
)

## Generate added messages and services with any dependencies listed here
#generate_messages(
#        DEPENDENCIES
#        std_msgs  # Or other packages containing msgs
#        trajectory_generator
#)

################################################################################
# Trajectory Line Creator Node
################################################################################
add_executable(trajectory_line_creator_node
        src/trajectory_line_creator.cpp
        src/trajectory_line_creator_node.cpp
        )

add_dependencies(trajectory_line_creator_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(trajectory_line_creator_node
        ${catkin_LIBRARIES}
        )

install(TARGETS trajectory_line_creator_node
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        )

################################################################################
# Install other files
################################################################################
install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h"
        PATTERN ".svn" EXCLUDE
        )

install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
        FILES_MATCHING PATTERN "*.launch"
        )

install(DIRECTORY config/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
        FILES_MATCHING PATTERN "*.yaml"
        )