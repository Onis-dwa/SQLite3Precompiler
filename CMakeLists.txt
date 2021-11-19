cmake_minimum_required (VERSION 3.8)
project (sqlite3 LANGUAGES C)

# https://www.sqlite.org/compile.html
add_definitions(-DSQLITE_ENABLE_RTREE)
add_definitions(-DSQLITE_ENABLE_STAT4)
add_definitions(-DSQLITE_ENABLE_FTS5)
#add_definitions(-DSQLITE_ENABLE_QPSG)
#add_definitions(-DSQLITE_ENABLE_SESSION)
#add_definitions(-DSQLITE_ENABLE_NULL_TRIM)
#add_definitions(-DSQLITE_ENABLE_ICU)
#add_definitions(-DSQLITE_ENABLE_JSON1)
#add_definitions(-DSQLITE_ENABLE_RBU)
add_definitions(-D)
if(${CMAKE_BUILD_TYPE} STREQUAL "Debug")
	add_definitions(-DSQLITE_ENABLE_DEBUG)
	#add_definitions(-DSQLITE_ENABLE_MEMDEBUG)
	#add_definitions(-DSQLITE_ENABLE_SQLLOG)
endif()


set(libName "sqlite3")
if(${CMAKE_BUILD_TYPE} STREQUAL "Release")
	set(CMAKE_CXX_FLAGS "/O2")
else()
	set(libName "sqlite3d")
endif()


include_directories(${CMAKE_SOURCE_DIR}/src)
option(CompileStatic "STATIC==ON SHARED==OFF" ON)
if(${CompileStatic})
	add_library(${libName} STATIC src/sqlite3.c src/sqlite3.h src/sqlite3ext.h)
else()
	add_library(${libName} SHARED src/sqlite3.c src/sqlite3.h src/sqlite3ext.h)
	set_target_properties(${libName} PROPERTIES DEFINE_SYMBOL "SQLITE_API=__declspec(dllexport)")
endif()


install(FILES src/sqlite3.h src/sqlite3ext.h DESTINATION ${CMAKE_SOURCE_DIR}/include)
install(TARGETS ${libName} LIBRARY DESTINATION ${CMAKE_SOURCE_DIR}/libs ARCHIVE DESTINATION ${CMAKE_SOURCE_DIR}/libs)