cmake_minimum_required(VERSION 3.20)

project("QCustomPlot" VERSION 2.1.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 COMPONENTS Widgets PrintSupport REQUIRED)
qt_standard_project_setup()

include(GNUInstallDirs)

# Create library target
add_library(QCustomPlot SHARED "qcustomplot.h" "qcustomplot.cpp")
target_include_directories(QCustomPlot INTERFACE $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
target_link_libraries(QCustomPlot PUBLIC Qt6::Widgets Qt6::PrintSupport)
target_compile_definitions(QCustomPlot PRIVATE QCUSTOMPLOT_COMPILE_LIBRARY)
target_compile_definitions(QCustomPlot INTERFACE $<INSTALL_INTERFACE:QCUSTOMPLOT_USE_LIBRARY>)

# Install
install(FILES "qcustomplot.h"
    DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "QCustomPlotConfigVersion.cmake"
    VERSION "${PACKAGE_VERSION}"
    COMPATIBILITY SameMajorVersion)

configure_file("QCustomPlotConfig.cmake.in" "QCustomPlotConfig.cmake" @ONLY)
install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/QCustomPlotConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/QCustomPlotConfigVersion.cmake"
    DESTINATION "lib/cmake/QCustomPlot")

install(TARGETS QCustomPlot EXPORT QCustomPlotTargets)
install(EXPORT QCustomPlotTargets
    FILE "QCustomPlotTargets.cmake"
    NAMESPACE "QCustomPlot::"
    DESTINATION "lib/cmake/QCustomPlot")
