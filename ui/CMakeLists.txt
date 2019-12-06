cmake_minimum_required(VERSION 3.0)
#set(CMAKE_VERBOSE_MAKEFILE ON)

project(cross2dui)

#################
# libcross2dui
#################
set(CROSS2DUI_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR})
file(GLOB CROSS2DUI_SOURCE ${CMAKE_CURRENT_SOURCE_DIR}/*.c*)

set(FLAGS -D__SSCRAP__)

if (UPDATE_CONFIG)
    list(APPEND FLAGS -D__UPDATE_CONFIG__)
endif ()

if (OPTION_FULLSCREEN)
    list(APPEND FLAGS -D__FULLSCREEN__)
endif ()

if (MPV_PLAYER)
    list(APPEND FLAGS -D__MPV__)
    list(APPEND LDFLAGS mpv)
endif ()

if (OPTION_BUILTIN_MINIZIP)
    list(APPEND CROSS2DUI_INCLUDE source)
    list(APPEND CROSS2DUI_SOURCE
            ${CMAKE_CURRENT_SOURCE_DIR}/minizip/ioapi.c
            ${CMAKE_CURRENT_SOURCE_DIR}/minizip/unzip.c)
	if (PLATFORM_WINDOWS)
		list(APPEND CROSS2DUI_SOURCE  ${CMAKE_CURRENT_SOURCE_DIR}/minizip/iowin32.c)
	endif ()
endif ()

##########################
# libcross2dui
##########################
add_library(${PROJECT_NAME} STATIC ${CROSS2DUI_SOURCE})
target_include_directories(${PROJECT_NAME} PUBLIC ${CROSS2DUI_INCLUDE})
target_compile_options(${PROJECT_NAME} PUBLIC ${FLAGS})
target_link_libraries(${PROJECT_NAME} cross2d sscrap ${LDFLAGS})