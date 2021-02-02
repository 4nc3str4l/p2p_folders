cmake_minimum_required(VERSION 3.11.4 FATAL_ERROR)
include(FetchContent)


# Project Name
set(PROJECT_NAME "p2p_folders")
project(${PROJECT_NAME})


set(SRC_PATH ${CMAKE_SOURCE_DIR}/src)

# CPP 20
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


#######################################################################
#                                FMT                                  #
#######################################################################
set(FMT_DOC     OFF CACHE INTERNAL "")
set(FMT_TEST    OFF CACHE INTERNAL "")

FetchContent_Declare(
    fmt
    GIT_REPOSITORY https://github.com/fmtlib/fmt
    GIT_TAG        6.1.2
)

FetchContent_GetProperties(fmt)
if(NOT fmt_POPULATED)
    FetchContent_Populate(fmt)
    add_subdirectory(${fmt_SOURCE_DIR} ${fmt_BINARY_DIR})
endif()

#######################################################################
#                             SOURCE                                  #
#######################################################################
file(GLOB src_files
    "src/**.h"
    "src/**.cpp"
)

#######################################################################
#                         EXECUTABLE                                  #
#######################################################################
add_executable(${PROJECT_NAME}
    ${src_files}
)

#######################################################################
#                             C++ 20                                  #
#######################################################################
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_20)
# Make sure that visual studio uses multiple threads
if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(${PROJECT_NAME} PUBLIC /MP /Zc:__cplusplus)
endif()



#######################################################################
#                            INCLUDES                                 #
#######################################################################

target_include_directories(${PROJECT_NAME} PUBLIC Source
    ${CMAKE_CURRENT_SOURCE_DIR}/externals/brynet-1.0.9/include
)


# LINK THE LIBARRIES
target_link_libraries(${PROJECT_NAME}
  PRIVATE fmt
  ${CMAKE_THREAD_LIBS_INIT}
  ${OPENSSL_LIBRARIES}
  ${Boost_LIBRARIES}
  ${CURL_LIBRARIES}
  -lstdc++fs
  -lpthread
)

