PROJECT(engine)
if(CMAKE_HOST_APPLE)
	MESSAGE(STATUS "APPLE")
	set(CMAKE_SHARED_LIBRARY_CREATE_CXX_FLAGS "${CMAKE_SHARED_LIBRARY_CREATE_CXX_FLAGS} -undefined dynamic_lookup")
endif()
file(GLOB src CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp")
add_library(engine MODULE ${src})
set_target_properties(engine PROPERTIES PREFIX "")
set_target_properties(engine PROPERTIES DEBUG_POSTFIX "")
set_target_properties(engine PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${ROOT_DIR}/bin)
target_include_directories(engine INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(engine PRIVATE lua)
if (CMAKE_HOST_WIN32)
target_link_libraries(engine PRIVATE Dijiang)
endif()

