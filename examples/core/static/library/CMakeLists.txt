cmake_minimum_required(VERSION 3.13)

project(calculator-static VERSION 0.0.1 LANGUAGES CXX)

add_library(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PRIVATE src/calculator.cpp)

include(GNUInstallDirs)

# sets the search paths for the include files after installation
# as well as during when building the library (as these may differ)
# this allows the library itself and users to #include the library headers
target_include_directories(
    ${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

# specify the target to install (calculator-static library defined above)
# set the export name <name>-config (does not need to match target name)
# also specify where the .lib file should be installed
install(
    TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}-config
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(
    EXPORT ${PROJECT_NAME}-config
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

install(
    DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})
