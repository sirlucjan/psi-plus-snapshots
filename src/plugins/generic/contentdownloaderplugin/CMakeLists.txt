unset(_HDRS)
unset(_UIS)
unset(_SRCS)
unset(_RSCS)
unset(PLUGIN)

set( PLUGIN contentdownloaderplugin )
project(${PLUGIN})
cmake_minimum_required(VERSION 3.1.0)
set( CMAKE_AUTOMOC TRUE )

set(PLUGINS_ROOT_DIR "${CMAKE_CURRENT_LIST_DIR}/../.." CACHE STRING "Plugins root path. Path where include directory placed")

if(EXISTS "${PLUGINS_ROOT_DIR}/variables.cmake")
    include("${PLUGINS_ROOT_DIR}/variables.cmake")
elseif(EXISTS "${CMAKE_CURRENT_LIST_DIR}/../../variables.cmake")
    include("${CMAKE_CURRENT_LIST_DIR}/../../variables.cmake")
else()
    message(FATAL_ERROR "No variables.cmake file found.
        Please set path to this file to PLUGINS_ROOT_DIR variable")
endif()

include_directories(
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${PLUGINS_ROOT_DIR}/include
    ${CMAKE_CURRENT_LIST_DIR}
)
set( _HDRS
    contentdownloader.h
    cditemmodel.h
    form.h
    contentitem.h
)
set( _SRCS
    contentdownloader.cpp
    cditemmodel.cpp
    form.cpp
    contentitem.cpp
)
set( _UIS
    form.ui
)
set( _RSCS
    resources.qrc
)

find_package( Qt5 COMPONENTS Widgets Xml Network REQUIRED )
set(QT_DEPLIBS
    Qt5::Widgets
    Qt5::Xml
    Qt5::Network
)
qt5_wrap_ui(UIS ${_UIS})
qt5_add_resources(RSCS ${_RSCS})

add_library(
    ${PLUGIN}
    MODULE
    ${_SRCS}
    ${UIS}
    ${RSCS}
)
target_link_libraries(
    ${PLUGIN}
    ${QT_DEPLIBS}
)
if( UNIX AND NOT( APPLE OR CYGWIN ) )
    install(
        TARGETS
        ${PLUGIN}
        LIBRARY
        DESTINATION
        ${CMAKE_INSTALL_PREFIX}/${PLUGINS_PATH}
        RUNTIME DESTINATION
        ${CMAKE_INSTALL_PREFIX}/${PLUGINS_PATH}
    )
endif()
if( WIN32 )
    install(
        TARGETS
        ${PLUGIN}
        LIBRARY
        DESTINATION
        ${CMAKE_INSTALL_PREFIX}/${PLUGINS_PATH}
        RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/${PLUGINS_PATH}
    )
endif()
