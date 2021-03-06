cmake_minimum_required (VERSION 3.15)
project(nlohmann_json VERSION 3.9.1 LANGUAGES CXX)


set(NLOHMANN_JSON_TARGET_NAME               ${PROJECT_NAME})
set(NLOHMANN_JSON_CONFIG_INSTALL_DIR        "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}" CACHE INTERNAL "")
set(NLOHMANN_JSON_INCLUDE_INSTALL_DIR       "${CMAKE_INSTALL_INCLUDEDIR}")
set(NLOHMANN_JSON_TARGETS_EXPORT_NAME       "${PROJECT_NAME}Targets")
set(NLOHMANN_JSON_CMAKE_CONFIG_DIR          "${CMAKE_CURRENT_BINARY_DIR}")

if (JSON_MultipleHeaders)
    set(NLOHMANN_JSON_INCLUDE_BUILD_DIR "${PROJECT_SOURCE_DIR}/include/")
    message(STATUS "Using the multi-header code from ${NLOHMANN_JSON_INCLUDE_BUILD_DIR}")
else()
    set(NLOHMANN_JSON_INCLUDE_BUILD_DIR "${PROJECT_SOURCE_DIR}/single_include/")
    message(STATUS "Using the single-header code from ${NLOHMANN_JSON_INCLUDE_BUILD_DIR}")
endif()

if (NOT JSON_ImplicitConversions)
    message(STATUS "Implicit conversions are disabled")
endif()

##
## TARGET
## create target and add include path
##
add_library(${NLOHMANN_JSON_TARGET_NAME} INTERFACE)
add_library(${PROJECT_NAME}::${NLOHMANN_JSON_TARGET_NAME} ALIAS ${NLOHMANN_JSON_TARGET_NAME})
target_compile_features(${NLOHMANN_JSON_TARGET_NAME} INTERFACE cxx_std_11)

target_compile_definitions(
    ${NLOHMANN_JSON_TARGET_NAME}
    INTERFACE
    JSON_USE_IMPLICIT_CONVERSIONS=$<BOOL:${JSON_ImplicitConversions}>
)

target_include_directories(
    ${NLOHMANN_JSON_TARGET_NAME}
    INTERFACE
    $<BUILD_INTERFACE:${NLOHMANN_JSON_INCLUDE_BUILD_DIR}>
    $<INSTALL_INTERFACE:include>
)
