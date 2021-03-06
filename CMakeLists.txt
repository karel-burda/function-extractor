cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

project(function-loader VERSION 1.2.6 LANGUAGES CXX)

add_library(${PROJECT_NAME} INTERFACE)

target_sources(${PROJECT_NAME}
                   INTERFACE
                       ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/function_loader.hpp
                       ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/exceptions.hpp

                       ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/function_loader_base.hpp
                       ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/function_loader_platform_specific.hpp
                       $<$<PLATFORM_ID:Windows>:
                           ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/function_loader_platform_specific_windows.hpp>
                       $<$<PLATFORM_ID:Unix>:
                           ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/function_loader_platform_specific_unix.hpp>

                       ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/library_loader.hpp
                       ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/library_loader_base.hpp
                       ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/library_loader_platform_specific.hpp
                       $<$<PLATFORM_ID:Windows>:
                           ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/library_loader_platform_specific_windows.hpp>
                       $<$<PLATFORM_ID:Unix>:
                           ${CMAKE_CURRENT_LIST_DIR}/include/function_loader/detail/library_loader_platform_specific_unix.hpp>)

target_include_directories(${PROJECT_NAME}
                                INTERFACE
                                    ${CMAKE_CURRENT_LIST_DIR}/include)

if (CMAKE_DL_LIBS)
    message(STATUS "Will add ${CMAKE_DL_LIBS}")
    set_target_properties(${PROJECT_NAME}
                              PROPERTIES
                                  INTERFACE_LINK_LIBRARIES "${CMAKE_DL_LIBS}")
endif()

target_compile_features(${PROJECT_NAME}
                            INTERFACE
                                cxx_std_11)

install(TARGETS ${PROJECT_NAME}
        EXPORT _targets)

export(EXPORT _targets
       NAMESPACE burda::
       FILE "${PROJECT_NAME}-config.cmake")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake
                                 COMPATIBILITY ExactVersion)
