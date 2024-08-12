cmake_minimum_required(VERSION 3.28)
project(HiQE)


set(CMAKE_CXX_STANDARD 20)

set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -Wall -Werror")
message("cmake using: USE_CPP17=${USE_CPP17}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")


# Write the export set for build and install tree
install(EXPORT "${HIQE_EXPORT_SET}" DESTINATION "${INSTALL_CMAKE_DIR}")
export(EXPORT "${HIQE_EXPORT_SET}"
        FILE "${PROJECT_BINARY_DIR}/${HIQE_EXPORT_SET}.cmake")


if(DEFINED ENV{CXX})
    set(CMAKE_CXX_COMPILER $ENV{CXX})
    message("cmake using: CXX=${CMAKE_CXX_COMPILER}")
endif()

if(DEFINED ENV{CC})
    set(CMAKE_C_COMPILER $ENV{CC})
    message("cmake using: CC=${CMAKE_C_COMPILER}")
endif()

message("cmake using: DEBUG_SE=${DEBUG_SE}")
if (${DEBUG_SE})
    add_definitions(-DDEBUG_SE=1)
    message("add_definitions -DDEBUG_SE=1")
endif()

message("build type: ${BUILD_TYPE}")
if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -g")
    set(CMAKE_CXX_FLAGS_DEBUG" ${CMAKE_CXX_FLAGS} -O0 -g")
elseif (CMAKE_BUILD_TYPE STREQUAL "Release")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -g") # disable -g
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS}")
elseif (CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -g")
    set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS}")
    add_executable(new_target CMakeLists.txt
            src/HiQE.cpp)
elseif (CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -ffunction-sections -fdata-sections -Os")
    set(LDFLAGS "-Wl,--gc-sections")
    set(CMAKE_CXX_FLAGS_MINSIZEREL "${CMAKE_CXX_FLAGS}")
endif()
message("CMAKE DEBUG: CMAKE_CXX_FLAGS=${CMAKE_CXX_FLAGS}")

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
set(PROJECT_INCLUDE_DIR ${PROJECT_INCLUDE_DIR} src/include ${PROJECT_SOURCE_DIR}/src)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
include_directories(${PROJECT_INCLUDE_DIR})

include_directories(src/include)
include_directories(third_party/fsst)
include_directories(third_party/fmt/include)
include_directories(third_party/hyperloglog)
include_directories(third_party/fastpforlib)
include_directories(third_party/skiplist)
include_directories(third_party/fast_float)
include_directories(third_party/re2)
include_directories(third_party/miniz)
include_directories(third_party/utf8proc/include)
include_directories(third_party/concurrentqueue)
include_directories(third_party/pcg)
include_directories(third_party/tdigest)
include_directories(third_party/mbedtls/include)
include_directories(third_party/jaro_winkler)
include_directories(third_party/yyjson/include)

add_subdirectory(src/include)
add_subdirectory(src)
add_subdirectory(third_party)

#add_executable(HiQE main.cpp)
