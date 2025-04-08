cmake_minimum_required(VERSION 3.8)
project(mujoco_ament)

find_package(ament_cmake REQUIRED)

# Define directories, and create directories if they do not exist
set(MUJOCO_DOWNLOAD_DIR ${CMAKE_CURRENT_BINARY_DIR}/download)
file(MAKE_DIRECTORY ${MUJOCO_DOWNLOAD_DIR})

# Define the base URL and version for the library
set(MUJOCO_VERSION 3.2.7)
if (CMAKE_SYSTEM_PROCESSOR STREQUAL "aarch64")
    set(MUJOCO_PLATFORM linux-aarch64)
else ()
    set(MUJOCO_PLATFORM linux-x86_64)
endif ()
include(FetchContent)
FetchContent_Declare(
    mujocoDownload
    URL https://github.com/google-deepmind/mujoco/releases/download/${MUJOCO_VERSION}/mujoco-${MUJOCO_VERSION}-${MUJOCO_PLATFORM}.tar.gz
    UPDATE_COMMAND ""
    SOURCE_DIR ${MUJOCO_DOWNLOAD_DIR}
)
FetchContent_MakeAvailable(mujocoDownload)

install(
    DIRECTORY ${MUJOCO_DOWNLOAD_DIR}/include/
    DESTINATION include/
)

install(
    FILES
    ${MUJOCO_DOWNLOAD_DIR}/lib/libmujoco.so
    ${MUJOCO_DOWNLOAD_DIR}/lib/libmujoco.so.${MUJOCO_VERSION}
    DESTINATION lib
)

ament_export_include_directories(include)
ament_export_libraries(mujoco)
ament_package()
