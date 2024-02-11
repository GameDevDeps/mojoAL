# YOU PROBABLY DON'T HAVE TO USE THIS CMAKE PROJECT, IT'S JUST A SINGLE
#  C FILE. COPY IT INTO YOUR PROJECT INSTEAD.

cmake_minimum_required(VERSION 3.12.0)
project(mojoAL)

set(CPM_DOWNLOAD_LOCATION "${CMAKE_BINARY_DIR}/cmake/CPM.cmake")
if(NOT (EXISTS ${CPM_DOWNLOAD_LOCATION}))
    file(DOWNLOAD https://github.com/cpm-cmake/CPM.cmake/releases/download/v0.38.3/CPM.cmake ${CPM_DOWNLOAD_LOCATION}
        EXPECTED_HASH SHA256=cc155ce02e7945e7b8967ddfaff0b050e958a723ef7aad3766d368940cb15494)
endif()
include(${CPM_DOWNLOAD_LOCATION})

CPMAddPackage(
    NAME sdl
    GITHUB_REPOSITORY GameDevDeps/SDL
    GIT_TAG SDL/release-2.30.0)

#find_package(SDL2 REQUIRED)
find_package(sdl REQUIRED)

#add_library(mojoal SHARED mojoal.c)
#target_include_directories(mojoal PRIVATE ${SDL2_INCLUDE_DIRS} AL)
#target_link_libraries(mojoal ${SDL2_LIBRARIES})

add_library(mojoal.mojoal STATIC ${CMAKE_CURRENT_SOURCE_DIR}/mojoal.c)
target_include_directories(mojoal.mojoal PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/AL>)
target_link_libraries(mojoal.mojoal PRIVATE sdl2.SDL2-static)
target_compile_definitions(mojoal.mojoal PUBLIC AL_LIBTYPE_STATIC)
set_target_properties(mojoal.mojoal PROPERTIES OUTPUT_NAME mojoal EXPORT_NAME mojoal)

#macro(add_test_executable _NAME)
#    add_executable(${_NAME} tests/${_NAME}.c)
#    target_link_libraries(${_NAME} mojoal)
#    target_include_directories(${_NAME} PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")
#    target_include_directories(${_NAME} PRIVATE ${SDL2_INCLUDE_DIRS})
#    target_link_libraries(${_NAME} ${SDL2_LIBRARIES})
#endmacro()
#
#add_test_executable(testopenalinfo)
#add_test_executable(testqueueing)
#add_test_executable(testcapture)
#add_test_executable(testposition)
