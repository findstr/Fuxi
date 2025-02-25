cmake_minimum_required(VERSION 3.15)

set(TARGET_NAME Dijiang)
# 获取OS信息
if (CMAKE_HOST_WIN32)
	MESSAGE(STATUS "WIN32")
	set(__WINDOWS__ 1)
	add_compile_options(-D__WINDOWS__ -DLUA_BUILD_AS_DLL)
elseif(CMAKE_HOST_APPLE)
	MESSAGE(STATUS "APPLE")
	add_compile_options(-Wno-nullability-completeness -D__MACOSX__)
	set(__MAXOSX__ 1)
elseif(CMAKE_HOST_UNIX)
	MESSAGE(STATUS "UNIX")
	set(__LINUX__ 1)
endif()

project(${TARGET_NAME})

include_directories(./)
include_directories(./render)
include_directories(./render/rhi)
include_directories(./render/vulkan)

aux_source_directory(./ SOURCES)
aux_source_directory(./editor SOURCES)
aux_source_directory(./math SOURCES)
aux_source_directory(./utils SOURCES)
aux_source_directory(./render SOURCES)
aux_source_directory(./resource SOURCES)
aux_source_directory(./render/vulkan SOURCES)
aux_source_directory(./components SOURCES)
aux_source_directory(./animation SOURCES)
aux_source_directory(./system SOURCES)

add_executable(${TARGET_NAME} ${SOURCES})

find_package(Vulkan REQUIRED FATAL_ERROR)
if (Vulkan_FOUND)
	include_directories(${Vulkan_INCLUDE_DIRS})
	target_link_libraries(${TARGET_NAME} PRIVATE ${Vulkan_LIBRARIES})
endif(Vulkan_FOUND)

set_target_properties(${TARGET_NAME} PROPERTIES ENABLE_EXPORTS 1)
set_target_properties(${TARGET_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${ROOT_DIR}/bin)

target_link_libraries(${TARGET_NAME} PRIVATE glfw)
target_link_libraries(${TARGET_NAME} PRIVATE glm)
target_link_libraries(${TARGET_NAME} PRIVATE yaml-cpp)
target_link_libraries(${TARGET_NAME} PRIVATE assimp::assimp)
target_link_libraries(${TARGET_NAME} PRIVATE vk_mem)
target_link_libraries(${TARGET_NAME} PRIVATE stb)
target_link_libraries(${TARGET_NAME} PRIVATE lua)
target_link_libraries(${TARGET_NAME} PRIVATE ktx)
target_link_libraries(${TARGET_NAME} PUBLIC imgui)
target_link_libraries(${TARGET_NAME} PRIVATE spirv-cross-core)
target_link_libraries(${TARGET_NAME} PRIVATE spirv-cross-cpp)
target_link_libraries(${TARGET_NAME} PRIVATE spirv-cross-reflect)
target_link_libraries(${TARGET_NAME} PRIVATE spirv-cross-hlsl)
 
add_subdirectory(lualib)


