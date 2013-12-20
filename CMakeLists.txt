cmake_minimum_required(VERSION 2.8.3)
project(nlopt)

set(CMAKE_BUILD_TYPE Release)

find_package(catkin REQUIRED)

include(ExternalProject)

set(VERSION 2.4.1)

catkin_package(
    DEPENDS
    CATKIN_DEPENDS
    INCLUDE_DIRS include
    LIBRARIES nlopt_wrap
)

ExternalProject_Add(nlopt_src
  DOWNLOAD_COMMAND wget http://ab-initio.mit.edu/nlopt/nlopt-${VERSION}.tar.gz
  PATCH_COMMAND tar -xzf ../nlopt-${VERSION}.tar.gz && rm -rf ../nlopt_src-build/* && mv nlopt-${VERSION} ../nlopt_src-build/
  CONFIGURE_COMMAND nlopt-${VERSION}/configure --with-cxx --without-matlab --prefix=${CATKIN_DEVEL_PREFIX}
  BUILD_COMMAND make
  INSTALL_COMMAND make install
)

add_library(nlopt_wrap src/wrap_lib.cc)
target_link_libraries(nlopt_wrap ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libnlopt_cxx.a)
add_dependencies(nlopt_wrap nlopt_src)

install(DIRECTORY include
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)
install(DIRECTORY lib
  DESTINATION ${CATKIN_GLOBAL_LIB_DESTINATION}
)
install(DIRECTORY share
  DESTINATION ${CATKIN_GLOBAL_SHARE_DESTINATION}
)

