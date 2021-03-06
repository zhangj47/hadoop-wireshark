# CMakeLists.txt
#
# $Id$
#


set(DISSECTOR_SRC
	packet-hdfsdata2.cpp
)

set(PLUGIN_FILES
	plugin.c
	${DISSECTOR_SRC}
)

set(CLEAN_FILES
	${PLUGIN_FILES}
)

if (WERROR)
	set_source_files_properties(
		${CLEAN_FILES}
		PROPERTIES
		COMPILE_FLAGS -Werror
	)
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

register_dissector_files(plugin.c
	plugin
	${DISSECTOR_SRC}
)

add_library(hadoop ${LINK_MODE_MODULE}
	${PLUGIN_FILES}
)
set_target_properties(hdfsdata2 PROPERTIES PREFIX "")
set_target_properties(hdfsdata2 PROPERTIES LINK_FLAGS "${WS_LINK_FLAGS}")

target_link_libraries(hdfsdata2 epan)

install(TARGETS hdfsdata2
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/@CPACK_PACKAGE_NAME@/plugins/${CPACK_PACKAGE_VERSION} NAMELINK_SKIP
	RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR}/@CPACK_PACKAGE_NAME@/plugins/${CPACK_PACKAGE_VERSION}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/@CPACK_PACKAGE_NAME@/plugins/${CPACK_PACKAGE_VERSION}
)

