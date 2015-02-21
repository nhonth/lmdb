project (lmdb)

set(${PROJECT_NAME}_HEADERS
    lmdb.h
    midl.h
)

set(${PROJECT_NAME}_SOURCES
  mdb.c
  midl.c
)
add_library(${PROJECT_NAME} SHARED ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_HEADERS} )
target_link_libraries(${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS "MDB_DLL_BUILD;_CRT_SECURE_NO_WARNINGS;")

# Install target
install(TARGETS ${PROJECT_NAME}
        EXPORT depends
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

# Install headers
INSTALL(FILES ${${PROJECT_NAME}_HEADERS} DESTINATION include/lmdb)
