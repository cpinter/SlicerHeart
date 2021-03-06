set(MODULE_NAME ConformalTextureMapping)

#-----------------------------------------------------------------------------

#
# SlicerExecutionModel
#
# find_package(SlicerExecutionModel REQUIRED)
# include(${SlicerExecutionModel_USE_FILE})

#
# LSCM
#
# find_package(LSCM REQUIRED)

#-----------------------------------------------------------------------------
set(MODULE_INCLUDE_DIRECTORIES
  )

set(MODULE_SRCS
  )

set(MODULE_TARGET_LIBRARIES
  )

#-----------------------------------------------------------------------------
# SEMMacroBuildCLI(
  # NAME ${MODULE_NAME}
  # TARGET_LIBRARIES ${MODULE_TARGET_LIBRARIES}
  # INCLUDE_DIRECTORIES ${MODULE_INCLUDE_DIRECTORIES}
  # ADDITIONAL_SRCS ${MODULE_SRCS}
  # RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${Slicer_CLIMODULES_BIN_DIR}"
  # LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${Slicer_CLIMODULES_LIB_DIR}"
  # ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${Slicer_CLIMODULES_LIB_DIR}"
  # )

#-----------------------------------------------------------------------------
add_custom_target(${MODULE_NAME} ALL
                  # COMMAND ${CMAKE_COMMAND} -E make_directory ${SlicerExecutionModel_DEFAULT_CLI_RUNTIME_OUTPUT_DIRECTORY}
                  COMMAND "${CMAKE_COMMAND}" -E copy "${CMAKE_CURRENT_SOURCE_DIR}/${MODULE_NAME}.xml" "${SlicerExecutionModel_DEFAULT_CLI_RUNTIME_OUTPUT_DIRECTORY}/$<CONFIG>"
                  DEPENDS lscm
                  VERBATIM
                  )

add_custom_command(TARGET ${MODULE_NAME} POST_BUILD
                   COMMAND "${CMAKE_COMMAND}" -E copy "${CMAKE_CURRENT_SOURCE_DIR}/${MODULE_NAME}.xml" "${SlicerExecutionModel_DEFAULT_CLI_RUNTIME_OUTPUT_DIRECTORY}/$<CONFIG>"
                   # COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/${MODULE_NAME}.xml ${SlicerExecutionModel_DEFAULT_CLI_RUNTIME_OUTPUT_DIRECTORY}/Debug
                   # COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/${MODULE_NAME}.xml ${SlicerExecutionModel_DEFAULT_CLI_RUNTIME_OUTPUT_DIRECTORY}/Release
                   COMMENT "Copying ${MODULE_NAME}.xml file to ${SlicerExecutionModel_DEFAULT_CLI_RUNTIME_OUTPUT_DIRECTORY}" 
                   DEPENDS lscm
                   VERBATIM
                   )

install(
  FILES
    ${SlicerExecutionModel_DEFAULT_CLI_RUNTIME_OUTPUT_DIRECTORY}/${MODULE_NAME}${CMAKE_EXECUTABLE_SUFFIX}
    ${CMAKE_CURRENT_SOURCE_DIR}/${MODULE_NAME}.xml
  DESTINATION ${Slicer_INSTALL_CLIMODULES_BIN_DIR}
  COMPONENT Runtime
  )

#-----------------------------------------------------------------------------
if(BUILD_TESTING)
  # add_subdirectory(Testing)
endif()
