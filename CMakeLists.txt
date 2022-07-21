cmake_minimum_required(VERSION 3.0.2)
project(akd_ethercat_lib)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
    roscpp
    message_generation
)

################################################
## Declare ROS messages, services and actions ##
################################################

## Generate messages in the 'msg' folder
add_message_files(
    DIRECTORY msg
    FILES
    akd_fixedPDO_1725.msg
    akd_fixedPDO_1B20.msg
)

###################################
## catkin specific configuration ##
###################################


catkin_package(
    CATKIN_DEPENDS message_runtime
    INCLUDE_DIRS AKD-Ethercat-Master/include AKD-Ethercat-Master/extern/SOEM/include
    LIBRARIES AKD_ecat_pkg soem
)

###########
## Build ##
###########

add_library(AKD_ecat_pkg)

#############
## Install ##
#############

## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html
#message("Catkin include path 2: ${CATKIN_PACKAGE_INCLUDE_DESTINATION}")
#install(TARGETS AKD_ecat soem
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#)

# Mark cpp header files for installation
#install(DIRECTORY ${INSTALL_ARTIFACTS_PATH}/include/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#)

#install(DIRECTORY ${INSTALL_ARTIFACTS_PATH}/lib/pkgconfig
#    DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#)

#install(DIRECTORY ${INSTALL_ARTIFACTS_PATH}/share/akd_ethercat_lib/
#    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#)

#install(DIRECTORY ${INSTALL_ARTIFACTS_PATH}/share/soem
#    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#)
#file(REMOVE_RECURSE ${INSTALL_ARTIFACTS_PATH})

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
