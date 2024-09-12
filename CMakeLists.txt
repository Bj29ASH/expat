cmake_minimum_required(VERSION 3.27)
project(expat VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(expat INTERFACE)

target_include_directories(expat
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include/expat>
)

target_link_directories(expat
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib>
  $<INSTALL_INTERFACE:lib/expat>
)

target_link_libraries(expat
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/libexpat.lib>
  $<INSTALL_INTERFACE:libexpat.lib>
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/expatConfig.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/expatConfig.cmake"
  INSTALL_DESTINATION share/expat
)

install(TARGETS expat
  EXPORT expatTargets
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

file(GLOB EXPAT_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)
install(FILES
  ${EXPAT_HEADERS}
  DESTINATION include/expat
)

install(FILES
  "${CMAKE_CURRENT_SOURCE_DIR}/lib/libexpat.lib"
  DESTINATION lib/expat
)

install(DIRECTORY
  "${CMAKE_CURRENT_SOURCE_DIR}/bin"
  DESTINATION /
)

install(
  EXPORT expatTargets
  DESTINATION share/expat
  NAMESPACE expat::
)

install(
  FILES
  "${CMAKE_CURRENT_BINARY_DIR}/expatConfig.cmake"
  DESTINATION share/expat
)