cmake_minimum_required (VERSION 3.24)
project( ConvertTest C )


set( PROJECT_HEADERS 
    ConvertTest/AppDelegate.h
    ConvertTest/GameViewController.h
    ConvertTest/ShaderTypes.h
    ConvertTest/Renderer.h
)

set( SOURCES
    ConvertTest/AppDelegate.m
    ConvertTest/GameViewController.m
    ConvertTest/Renderer.m
    ConvertTest/main.m
)

set( STATIC_DEPENDENCIES
)

if (APPLE)
    find_library( APPKIT AppKit )
    find_library( METAL Metal )
    find_library( METALKIT MetalKit )
    find_library( MODELIO ModelIO )
endif()

set( DEPENDENCIES
    ${APPKIT}
    ${METAL}
    ${METALKIT}
    ${MODELIO}
)

set( GLOBAL_RESOURCES
    ConvertTest/Assets.xcassets
)

set( BASE_RESOURCES
    ConvertTest/Base.lproj/Main.storyboard
    ConvertTest/Shaders.metal
)

set( RESOURCES
    ${GLOBAL_RESOURCES}
    ${BASE_RESOURCES}
)


##
## ConvertTest
##

add_executable( ConvertTest MACOSX_BUNDLE
    ${SOURCES}
    ${PUBLIC_HEADERS}
    ${PROJECT_HEADERS}
    ${PRIVATE_HEADERS}
    ${RESOURCES}
)

target_link_libraries( ConvertTest
    ${STATIC_DEPENDENCIES}
    ${DEPENDENCIES}
)

set_source_files_properties(
    ${RESOURCES}
    PROPERTIES
    MACOSX_PACKAGE_LOCATION
    Resources
)

if (APPLE)
    target_compile_options( ConvertTest PUBLIC "-fobjc-arc" )
    
    set( MACOSX_BUNDLE_GUI_IDENTIFIER ConvertTest )
    set( MACOSX_BUNDLE_BUNDLE_NAME Organization.ConvertTest )
    
    set_target_properties( ConvertTest
        PROPERTIES
        MACOSX_BUNDLE_INFO_PLIST
        "${CMAKE_CURRENT_SOURCE_DIR}/ConvertTest-Info.plist.in"
    )
endif()
