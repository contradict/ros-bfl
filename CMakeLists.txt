cmake_minimum_required(VERSION 2.8.3)
project(bfl)

include(ExternalProject)

catkin_package(
    LIBRARIES
        orocos-bfl
    CATKIN_DEPENDS
        roscpp
    CFG_EXTRAS
        includedirs.cmake
)

set(SOURCE_FILENAME bfl-31655.tar.gz)
set(SOURCE_URL http://pr.willowgarage.com/downloads/${SOURCE_FILENAME})
set(SOURCE_FILE_MD5SUMFILE ${FILENAME}.md5sum)
set(SOURCE_FILE_MD5SUM 7a47aa842eb4369a483d312f3f0ab789)

ExternalProject_Add(
    orocos-bfl-extproject
    URL ${SOURCE_URL}
    PATCH_COMMAND patch -p0 < ${PROJECT_SOURCE_DIR}/total.patch
    CMAKE_ARGS
        -DBUILD_TESTS=OFF
	    -DMATRIX_LIB=boost -DRNG_LIB=boost
        -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
        -DCMAKE_BUILD_TYPE="Release"
)

#add_library(orocos-bfl SHARED IMPORTED GLOBAL
#    IMPORTED_LOCATION ${CATKIN_DEVEL_PREFIX}/lib/liborocos-bfl.so)
add_library(orocos-bfl src/dummy.c)

install(FILES ${CATKIN_DEVEL_PREFIX}/lib/liborocos-bfl.so
    DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/bfl/
    DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
    )
