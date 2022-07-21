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
)#########################################

## Generate messages in the 'msg' folder
add_message_files(
    DIRECTORY msg
    FILES
    akd_fixedPDO_1725.msg
    akd_fixedPDO_1B20.msg
)

set(WRAPPER_INSTALL_PATH ${CMAKE_CURRENT_LIST_DIR}/wrapper_install)
make_directory(${CMAKE_CURRENT_LIST_DIR}/include)
catkin_package(
    CATKIN_DEPENDS message_runtime
    INCLUDE_DIRS include
    LIBRARIES AKD_ecat soem #Macro assumes these will be in CATKIN_PACKAGE_LIB_DESTINATION. Might break older catkin stuff.
                                # In that case, maybe add an imported library? The imporated path might be taken.
)

###########
## Build ##
###########


include(ExternalProject)
ExternalProject_Add( AKD_lib 
    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${WRAPPER_INSTALL_PATH}
    SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/AKD-Ethercat-Master
)

#add_library(AKD_ecat STATIC IMPORTED IMPORTED_LOCATION ${WRAPPER_INSTALL_PATH}/lib)
#add_library(soem STATIC IMPORTED IMPORTED_LOCATION ${WRAPPER_INSTALL_PATH}/lib)

#############
## Install ##
#############




## Mark libraries for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_libraries.html

install(DIRECTORY ${WRAPPER_INSTALL_PATH}/lib/
    DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

#Mark cpp header files for installation
install(DIRECTORY ${WRAPPER_INSTALL_PATH}/include/
   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
   FILES_MATCHING PATTERN "*.h"
)
install(DIRECTORY ${WRAPPER_INSTALL_PATH}/include/
   DESTINATION ${CMAKE_CURRENT_LIST_DIR}/include
   FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY ${WRAPPER_INSTALL_PATH}/share/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)


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
