cmake_minimum_required(VERSION 2.8)
project (sqlite3 C)

#set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
#set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

set (SQLITE_SOURCE_DIR sqlite-autoconf-3071502)
set (SQLITE_REGEXP_DIR sqlite-regexp)
set (PREC_SOURCE_DIR pcre-8.32)

# PCRE
#add_definitions(-DPCRE_STATIC -DPCRE_SHARED)
#add_definitions(-DPCRE_SUPPORT_UTF:bool=ON)
add_subdirectory (${PREC_SOURCE_DIR})
include_directories(${CMAKE_BINARY_DIR}/${PREC_SOURCE_DIR})

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# SQLITE
set (SOURCES ${SQLITE_SOURCE_DIR}/sqlite3.c)
set (HEADERS ${SQLITE_SOURCE_DIR}/sqlite3.h)
include_directories(${SQLITE_SOURCE_DIR})

# SQLITE with REGEXP
set (SOURCE_SQLITE_REGEXP ${SQLITE_REGEXP_DIR}/regexp.c)
add_library (sqlite3_regexp SHARED ${SOURCE_SQLITE_REGEXP})
target_link_libraries(sqlite3_regexp pcre)

add_library (sqlite3-shared SHARED ${SOURCES} ${HEADERS})
target_link_libraries(sqlite3-shared) 
set_target_properties(sqlite3-shared PROPERTIES OUTPUT_NAME sqlite3)
add_library (sqlite3-static STATIC ${SOURCES} ${HEADERS})
target_link_libraries(sqlite3-static pthread dl)
set_target_properties(sqlite3-static PROPERTIES OUTPUT_NAME sqlite3)
add_executable(sqlite3-shell ${SQLITE_SOURCE_DIR}/shell.c ${SOURCES} ${HEADERS})
target_link_libraries(sqlite3-shell pcre pthread dl)
set_target_properties(sqlite3-shell PROPERTIES OUTPUT_NAME sqlite3)

#install ( TARGETS sqlite3-shared sqlite3-static sqlite3-shell
#	RUNTIME DESTINATION bin 
#	LIBRARY DESTINATION lib
#	ARCHIVE DESTINATION lib)

