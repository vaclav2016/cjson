cmake_minimum_required(VERSION 2.8)
configure_file(config.h.in "${CMAKE_CURRENT_BINARY_DIR}/config.h" @ONLY)

add_library(cjson STATIC json.c json.h json-builder.c json-builder.h config.h.in)

set_target_properties(cjson PROPERTIES
  PUBLIC_HEADER "json.h;json-builder.h;${CMAKE_CURRENT_BINARY_DIR}/config.h")

install(TARGETS cjson
  # IMPORTANT: Add the foo library to the "export-set"
  EXPORT cjsonTargets
  RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}"
  ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/cjson"
    COMPONENT dev)
