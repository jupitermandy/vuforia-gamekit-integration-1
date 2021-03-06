# ---------------------------------------------------------
cmake_minimum_required(VERSION 2.6)

PROJECT(ogrekit)

include_directories(
	${OGREKIT_INCLUDE}
)

link_libraries(
	${OGREKIT_LIB}	
	log
	GLESv2		
)

set(HiddenCMakeLists ../CMakeLists.txt)
source_group(ParentCMakeLists FILES ${HiddenCMakeLists})

ADD_LIBRARY(${PROJECT_NAME}
	SHARED
	Main.cpp
	AndroidLogListener.h
	AndroidLogListener.cpp
)

if (1) #cp && strip

set (OUTPUT_NAME ${CMAKE_SHARED_LIBRARY_PREFIX}${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX}) #lib<name>.so
set (STRIP_TOOL arm-linux-androideabi-strip)

#message(STATUS ${PROJECT_NAME} "-------" ${OUTPUT_NAME} "-------" ${STRIP_TOOL})

add_custom_command(	TARGET ${PROJECT_NAME}
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} 
	ARGS -E copy_if_different 
		${OUTPUT_NAME}	
		${CMAKE_CURRENT_SOURCE_DIR}/../Demo/libs/${ANDROID_ABI}/${OUTPUT_NAME}
)		
#		${CMAKE_CURRENT_SOURCE_DIR}/../Demo/libs/armeabi/${OUTPUT_NAME}

add_custom_command(
	TARGET ${PROJECT_NAME}
	POST_BUILD
	COMMAND ${CMAKE_STRIP}
	ARGS ${CMAKE_CURRENT_SOURCE_DIR}/../Demo/libs/${ANDROID_ABI}/${OUTPUT_NAME})	
endif()

message (STATUS "android-outputdir  ${CMAKE_CURRENT_SOURCE_DIR}/../Demo/libs/${ANDROID_ABI}/${OUTPUT_NAME}")