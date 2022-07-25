cmake_minimum_required(VERSION 3.0.2)
project(akd_ethercat_lib)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
    roscpp
    std_msgs
    message_generation
)

## Generate messages in the 'msg' folder
add_message_files(
    DIRECTORY msg
    FILES
    akd_fixedPDO_1725.msg
    akd_fixedPDO_1B20.msg
)

generate_messages(
    DEPENDENCIES 
    std_msgs
)

# Workaround for cmake complaining about missing include directory
file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

# Workaround for catkin_lint complaining about external include directory
list(APPEND ${PROJECT_NAME}_INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include)

catkin_package(
    CATKIN_DEPENDS message_runtime
    LIBRARIES AKD_ecat soem 
)

###########
## Build ##
###########


include(ExternalProject)
ExternalProject_Add( AKD_lib 
    SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/AKD-Ethercat-Master
    CMAKE_ARGS #-DCMAKE_ARCHIVE_OUTPUT_DIRECTORY=${CMAKE_BINARY_DIR}
               -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
)

add_library(AKD_ecat INTERFACE)
target_include_directories(AKD_ecat SYSTEM INTERFACE ${CATKIN_DEVEL_PREFIX}/include )
target_link_libraries(AKD_ecat INTERFACE ${CATKIN_DEVEL_PREFIX}/lib/libAKD_ecat.a)
add_dependencies(AKD_ecat AKD_lib)

add_library(soem INTERFACE)
target_include_directories(soem SYSTEM INTERFACE ${CATKIN_DEVEL_PREFIX}/include/soem )
target_link_libraries(soem INTERFACE ${CATKIN_DEVEL_PREFIX}/lib/libsoem.a)
add_dependencies(soem AKD_lib)

#############
## Install ##
#############




## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html

install(FILES ${CATKIN_DEVEL_PREFIX}/lib/libAKD_ecat.a ${CATKIN_DEVEL_PREFIX}/lib/libsoem.a
    DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

#Mark cpp header files for installation
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/
   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
   FILES_MATCHING PATTERN "*.h"
)

#install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/share/
#    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#)


#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_akd_ethercat_lib.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
