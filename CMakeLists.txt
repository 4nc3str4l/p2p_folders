cmake_minimum_required(VERSION 3.11.4 FATAL_ERROR)
include(FetchContent)


# Project Name
set(PROJECT_NAME "p2p_folders")
project(${PROJECT_NAME})

# USEFULL PATHS AND DEFINITIONS
set(CMAKE_BUILD_TYPE RelWithDebInfo)

set(SRC_PATH ${CMAKE_SOURCE_DIR}/src)

# CPP 17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")


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

# CREATE THE EXECUTABLE
add_executable(${PROJECT_NAME}
    ${src_files}
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