cmake_minimum_required(VERSION 2.8)

set(MINFS_SRC_DIR
    ${CMAKE_CURRENT_SOURCE_DIR}
)

set( MINFS_INCLUDE_DIRS
    "${MINFS_SRC_DIR}/include"
)

file(GLOB MINFS_HDR_FILES
    "${MINFS_SRC_DIR}/include/*.h"
)

file(GLOB MINFS_SRC_FILES
    "${MINFS_SRC_DIR}/src/*.c"
)

if (PLATFORM_WINDOWS)
    set(MINFS_LIBS Shlwapi)
    list(REMOVE_ITEM MINFS_SRC_FILES "${MINFS_SRC_DIR}/src/minfs_linux.c")
endif()
if (PLATFORM_LINUX)
    list(REMOVE_ITEM MINFS_SRC_FILES "${MINFS_SRC_DIR}/src/minfs_win.c")
endif()

source_group(include FILES ${MINFS_HDR_FILES})
source_group(source  FILES ${MINFS_SRC_FILES})

include_directories(${MINFS_INCLUDE_DIRS})
add_library(minfs ${MINFS_SRC_FILES} ${MINFS_HDR_FILES})
target_link_libraries(minfs ${MINFS_LIBS})