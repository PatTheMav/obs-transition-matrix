cmake_minimum_required(VERSION 3.16)

project(obs-transition-matrix)

set(MACOSX_PLUGIN_GUI_IDENTIFIER "com.exeldro.obs-transition-matrix")
set(MACOSX_PLUGIN_BUNDLE_VERSION "0.0.1")
set(MACOSX_PLUGIN_SHORT_VERSION_STRING "1")

find_package(libobs REQUIRED)
find_package(obs-frontend-api REQUIRED)
include(external/ObsPluginHelpers.cmake)

find_package(Qt5Core REQUIRED)
find_package(Qt5Widgets REQUIRED)

add_library(obs-transition-matrix MODULE)

target_sources(obs-transition-matrix
	PRIVATE
		obs-transition-matrix.cpp
		obs-transition-matrix.hpp
		obs-transition-matrix-dialog.cpp
		obs-transition-matrix-dialog.hpp
		obs-transition-matrix-model.cpp
		obs-transition-matrix-model.hpp)


target_link_libraries(obs-transition-matrix
	PRIVATE
		OBS::libobs
		OBS::obs-frontend-api
		Qt5::Core
		Qt5::Widgets)

set_target_properties(obs-transition-matrix PROPERTIES
	AUTOMOC ON
	AUTOUIC ON
	AUTORCC ON)

target_compile_features(obs-transition-matrix
	PRIVATE cxx_std_17)

if(OS_WINDOWS)
    if (MSVC)
        target_compile_definitions(obs-transition-matrix
        	PRIVATE
        		/MP
        		/d2FH4-)
    endif()
elseif(OS_MACOS)
	target_compile_options(obs-transition-matrix
		PRIVATE
			-Wall
			-Wextra
			-Werror-implicit-function-declaration
			-stdlib=libc++
			-fvisibility=default)


	set_target_properties(obs-transition-matrix PROPERTIES PREFIX "")
else()
	target_compile_options(obs-transition-matrix
		PRIVATE
			-Wall
			-Wextra)

	set_target_properties(obs-transition-matrix PROPERTIES PREFIX "")
endif()

setup_plugin_target(obs-transition-matrix)
