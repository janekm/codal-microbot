project(codal-microbot)

include("${CODAL_UTILS_LOCATION}")

# find sources and headers
RECURSIVE_FIND_DIR(TOP_LEVEL_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/inc" "*.h")
RECURSIVE_FIND_FILE(TOP_LEVEL_SOURCE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/source" "*.c??")

RECURSIVE_FIND_DIR(MODEL_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/model" "*.h")
RECURSIVE_FIND_FILE(MODEL_SOURCE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/model" "*.c??")

set(CMAKE_LINKER_FLAGS "${CMAKE_LINKER_FLAGS} -T\"${CMAKE_CURRENT_LIST_DIR}/ld/nrf52832.ld\"" PARENT_SCOPE)
set(CMAKE_EXE_LINKER_FLAGS    "${CMAKE_EXE_LINKER_FLAGS} -T\"${CMAKE_CURRENT_LIST_DIR}/ld/nrf52832.ld\"" PARENT_SCOPE)
set(CMAKE_SYSTEM_PROCESSOR "armv7-m" PARENT_SCOPE)

# add them
include_directories(${INCLUDE_DIRS} ${TOP_LEVEL_INCLUDE_DIRS} ${MODEL_INCLUDE_DIRS})

# create our target
add_library(codal-microbot ${TOP_LEVEL_SOURCE_FILES} ${MODEL_SOURCE_FILES})

target_link_libraries(
    codal-microbot
    codal-core
    codal-mbedos
    codal-nrf52
)

# expose it to parent cmake.
target_include_directories(codal-microbot PUBLIC ${MODEL_INCLUDE_DIRS} ${TOP_LEVEL_INCLUDE_DIRS})