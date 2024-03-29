file(GLOB_RECURSE ${P_NAME}_SRCS
    src/*.cc
    src/*.cpp
)

file(GLOB_RECURSE ${P_NAME}_HEADERS
    include/*.h
)

file(GLOB_RECURSE ${P_NAME}_UIS
    UI/*.ui
)

file(GLOB_RECURSE ${P_NAME}_RES
    res/*.qrc
)

set(CMAKE_AUTOUIC_SEARCH_PATHS ./UI)

set(PROJECT_SOURCES
    ${${P_NAME}_SRCS}
    ${${P_NAME}_HEADERS}
    ${${P_NAME}_UIS}
    ${${P_NAME}_RES}
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(${P_NAME}
        MANUAL_FINALIZATION
        MACOSX_BUNDLE   
        ${PROJECT_SOURCES}
    )
else()
    if(ANDROID)
        add_library(${P_NAME} SHARED
            ${PROJECT_SOURCES}
        )
    else()
        add_executable(${P_NAME}
            ${PROJECT_SOURCES}
        )
    endif() 
endif()

target_link_libraries(${P_NAME} PRIVATE
    Model
    Qt${QT_VERSION_MAJOR}::Widgets
    Qt${QT_VERSION_MAJOR}::PrintSupport
)

target_include_directories(${P_NAME} PUBLIC 
    ./include
)
set_target_properties(${P_NAME} PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER ${P_NAME}_esi
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
)
