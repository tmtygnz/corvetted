cmake_minimum_required(VERSION 3.16)

project(corvetted VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(Qt6 REQUIRED COMPONENTS Quick)

file(GLOB_RECURSE CPP_FILES CONFIGURE_DEPENDS
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp"
)

file(GLOB_RECURSE QML_FILES CONFIGURE_DEPENDS
    RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    "src/pages/*.qml"
)

qt_standard_project_setup(REQUIRES 6.10)

qt_add_executable(appcorvetted
    ${CPP_FILES}
)

qt_add_qml_module(appcorvetted
    URI corvetted
    QML_FILES
        ${QML_FILES}
)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
set_target_properties(appcorvetted PROPERTIES
#    MACOSX_BUNDLE_GUI_IDENTIFIER com.example.appcorvetted
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

target_link_libraries(appcorvetted
    PRIVATE Qt6::Quick
)

install(TARGETS appcorvetted
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
