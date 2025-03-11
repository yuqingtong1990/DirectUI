# cmake file for direct_ui
#Author Qi Gao(monkgau@gmail.com)
#Created: 2012/09/16

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR} root_src)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/control control_src)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/core core_src)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/layout layout_src)
aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/utils utils_src)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/control)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/core)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/layout)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/utils)

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
add_library(direct_ui SHARED ${control_src} ${core_src} ${layout_src} ${utils_src} ${root_src})

add_definitions(-direct_ui_EXPORTS)
target_link_libraries(direct_ui comctl32)
target_include_directories(direct_ui PUBLIC $<INSTALL_INTERFACE:include>)
target_compile_definitions(direct_ui PRIVATE UILIB_EXPORTS)
set_target_properties(direct_ui PROPERTIES OUTPUT_NAME "direct_ui")

# Install headers
file(GLOB direct_ui_PUBLIC_HDRS "${CMAKE_CURRENT_SOURCE_DIR}/*.h")
file(GLOB direct_ui_CORE_HDRS "${CMAKE_CURRENT_SOURCE_DIR}/core/*.h")
file(GLOB direct_ui_UTILS_HDRS "${CMAKE_CURRENT_SOURCE_DIR}/utils/*.h")
file(GLOB direct_ui_CONTROL_HDRS "${CMAKE_CURRENT_SOURCE_DIR}/control/*.h")
file(GLOB direct_ui_LAYOUT_HDRS "${CMAKE_CURRENT_SOURCE_DIR}/layout/*.h")

install(FILES ${direct_ui_PUBLIC_HDRS}     DESTINATION include)
install(FILES ${direct_ui_CORE_HDRS}       DESTINATION include/core)
install(FILES ${direct_ui_UTILS_HDRS}      DESTINATION include/utils)
install(FILES ${direct_ui_CONTROL_HDRS}    DESTINATION include/control)
install(FILES ${direct_ui_LAYOUT_HDRS}     DESTINATION include/layout)

# Install binaries
install(
  TARGETS direct_ui
  EXPORT  direct_ui-config
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)

# Install cmake configure files
install(EXPORT direct_ui-config
    NAMESPACE dui::
    DESTINATION share/direct_ui
)
