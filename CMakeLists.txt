cmake_minimum_required(VERSION 3.15)
cmake_policy(SET CMP0091 NEW)

#set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

project(b-em VERSION 2.2)

#set(CPACK_PACKAGING_INSTALL_PREFIX "/usr/local")

add_compile_definitions(
	_GNU_SOURCE
)


macro(print_all_variables)
	message(STATUS "print_all_variables------------------------------------------{")
	get_cmake_property(_variableNames VARIABLES)
	foreach (_variableName ${_variableNames})
		message(STATUS "${_variableName}=${${_variableName}}")
	endforeach()
	message(STATUS "print_all_variables------------------------------------------}")
endmacro()


add_subdirectory(src)

install(
	FILES
		${CPACK_RESOURCE_FILE_README}
		${CPACK_RESOURCE_FILE_LICENSE}
	DESTINATION share/doc/b-em
)

install(
	DIRECTORY
		ddnoise
		discs
		fonts
		roms
		tapes
	DESTINATION share/b-em
)


#print_all_variables()

include(InstallRequiredSystemLibraries)
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/COPYING")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
set(CPACK_PACKAGE_VERSION_MAJOR "${b-em_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${b-em_VERSION_MINOR}")
set(CPACK_SOURCE_GENERATOR "TGZ")
set(CPACK_BINARY_TZ OFF)
set(CPACK_BINARY_STGZ OFF)
set(CPACK_GENERATOR "TGZ")

include(CPack)

