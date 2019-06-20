cmake_minimum_required(VERSION 3.0)

set(libBasename JKQTFastPlotter)

if(BUILD_SHARED_LIBS)
    set(libsh_name ${libBasename}SharedLib)
    set(${libsh_name}_LIBRARY ${libsh_name} PARENT_SCOPE)
    message( STATUS "-- Building ${libsh_name}" )
endif(BUILD_SHARED_LIBS)

if(BUILD_STATIC_LIBS)
    set(lib_name ${libBasename}Lib)
    set(${lib_name}_LIBRARY ${lib_name} PARENT_SCOPE)
    message( STATUS "-- Building ${lib_name}" )
endif(BUILD_STATIC_LIBS)


# Set up source files
set(SOURCES
  jkqtfastplotter.cpp
)

set(HEADERS
  jkqtfastplotter.h
)



if(BUILD_SHARED_LIBS)
    add_library(${libsh_name} SHARED  ${SOURCES} ${HEADERS} ${RESOURCES})
    set_property(TARGET ${libsh_name} PROPERTY VERSION "${PROJECT_VERSION}")
    target_link_libraries(${libsh_name} Qt5::Core Qt5::Gui Qt5::Widgets Qt5::PrintSupport Qt5::OpenGL JKQTCommonSharedLib)
    target_compile_definitions(${libsh_name} PUBLIC JKQTP_LIB_IN_DLL)
    target_compile_definitions(${libsh_name} PRIVATE JKQTP_LIB_EXPORT_LIBRARY)
endif()

if(BUILD_STATIC_LIBS)
    add_library(${lib_name} STATIC  ${SOURCES} ${HEADERS} ${RESOURCES})
    set_property(TARGET ${lib_name} PROPERTY VERSION "${PROJECT_VERSION}")
    target_link_libraries(${lib_name} Qt5::Core Qt5::Gui Qt5::Widgets Qt5::PrintSupport Qt5::OpenGL JKQTCommonLib)
endif()



# Installation
if(LIB_INSTALL)

    if(BUILD_SHARED_LIBS)
        install(TARGETS ${libsh_name} EXPORT ${libsh_name}_TARGETS
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
            ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
            LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
            INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/jkqtfastplotter
        )
    endif(BUILD_SHARED_LIBS)

    if(BUILD_STATIC_LIBS)
        install(TARGETS ${lib_name} EXPORT ${lib_name}_TARGETS
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
            ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
            LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
            INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/jkqtfastplotter
        )
    endif(BUILD_STATIC_LIBS)

    install(FILES ${HEADERS}
            DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/jkqtfastplotter
            COMPONENT Headers)

endif(LIB_INSTALL)