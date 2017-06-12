cmake_minimum_required(VERSION 2.8)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build (Debug or Release)" FORCE)
endif()

#project configuration
project(SFMLTest)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
set(VERSION_MAJOR 21)
set(VERSION_MINOR 37)
if(CMAKE_BUILD_TYPE MATCHES Debug)
	set(SFML_LIBS sfml-system-d sfml-window-d sfml-graphics-d)
else()
	if (WIN32)
		set(SFML_LIBS sfml-main sfml-system sfml-window sfml-graphics)
	else()
		set(SFML_LIBS sfml-system sfml-window sfml-graphics)
	endif()
endif()

configure_file(
    "${PROJECT_SOURCE_DIR}/cmake_config.hpp.in"
    "${PROJECT_SOURCE_DIR}/cmake_config.hpp"
)

# Detect and add SFML
if(WIN32)
	set(SFML_INCLUDE_DIRECTORY "" CACHE PATH "SFML include directory")
	set(SFML_LIB_DIRECTORY "" CACHE PATH "SFML libraries directory")
elseif(UNIX)
	set (SFML_INCLUDE_DIRECTORY "/usr/include" CACHE PATH "SFML include directory")
	set (SFML_LIB_DIRECTORY "/usr/lib" CACHE PATH "SFML libraries directory")
endif()

include_directories(${SFML_INCLUDE_DIRECTORY} ${PROJECT_SOURCE_DIR})
link_directories(${SFML_LIB_DIRECTORY})
if(WIN32)
	if(CMAKE_BUILD_TYPE MATCHES Debug)
		add_executable(${PROJECT_NAME} ${PROJECT_SOURCE_DIR}/main.cpp)
	else()
		add_executable(${PROJECT_NAME} WIN32 ${PROJECT_SOURCE_DIR}/main.cpp)
	endif()
else()
	add_executable(${PROJECT_NAME} ${PROJECT_SOURCE_DIR}/main.cpp)
endif()
target_link_libraries(${PROJECT_NAME} ${SFML_LIBS})
install(TARGETS ${PROJECT_NAME} DESTINATION bin)

include(InstallRequiredSystemLibraries)
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/LICENSE")
set(CPACK_PACKAGE_VERSION_MAJOR "${myproject_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${myproject_VERSION_MINOR}")
include(CPack)
