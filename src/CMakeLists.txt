cmake_minimum_required(VERSION 3.0.0)
project(sdl2_demo VERSION 0.1.0)

add_executable(sdl2_demo main.cpp)

find_package(Git QUIET)
if(GIT_FOUND AND EXISTS "${PROJECT_SOURCE_DIR}/.git")
    option(GIT_SUBMODULE "Check submodules during build" ON)
    if(GIT_SUBMODULE)
        message(STATUS "Submodule update")
        execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
                      WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                      RESULT_VARIABLE GIT_SUBMOD_RESULT)  
        if(NOT GIT_SUBMOD_RESULT EQUAL "0")
            message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}")
        endif()
    endif()
endif()

if(NOT EXISTS "${PROJECT_SOURCE_DIR}/external/glfw/CMakeLists.txt")
    message(FATAL_ERROR "The glfw submodules was not downloaded!")
endif()

add_subdirectory(external/glfw)

target_include_directories(${PROJECT_NAME} PUBLIC external/glfw/include)

target_link_directories(${PROJECT_NAME} PRIVATE external/glfw/src)

target_link_libraries(${PROJECT_NAME} glfw)