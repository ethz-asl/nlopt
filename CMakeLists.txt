cmake_minimum_required(VERSION 2.8.3)
project(nlopt)

set(CMAKE_BUILD_TYPE Release)

find_package(catkin REQUIRED)

include(ExternalProject)

set(VERSION 2.4.1)

ExternalProject_Add(nlopt
  DOWNLOAD_COMMAND wget http://ab-initio.mit.edu/nlopt/nlopt-${VERSION}.tar.gz
  PATCH_COMMAND tar -xzf ../nlopt-${VERSION}.tar.gz && mv nlopt-${VERSION} ../nlopt-build/
  CONFIGURE_COMMAND nlopt-${VERSION}/configure --prefix=${CMAKE_CURRENT_SOURCE_DIR}
  BUILD_COMMAND make && cp ${CMAKE_CURRENT_SOURCE_DIR}/lib/libnlopt.a ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libnlopt_cxx.a
)

install(DIRECTORY include
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)
install(DIRECTORY lib
  DESTINATION ${CATKIN_GLOBAL_LIB_DESTINATION}
)
install(DIRECTORY share
  DESTINATION ${CATKIN_GLOBAL_SHARE_DESTINATION}
)

catkin_package(
    DEPENDS
    CATKIN_DEPENDS
    INCLUDE_DIRS include
    LIBRARIES nlopt_cxx
    CFG_EXTRAS export_flags.cmake
)

