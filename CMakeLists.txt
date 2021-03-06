cmake_minimum_required(VERSION 2.8)
Project(nvpro-samples)

# setting the base directory will also inform sub-cmake files
# that they are included and not the first entry point
SET(BASE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/..)
INCLUDE(${CMAKE_CURRENT_SOURCE_DIR}/../shared_sources/CMakeLists_include.txt)

macro(_addFolder _PATH)
  FILE (GLOB samples "${_PATH}/*")
  FOREACH( sample ${samples} )
      if( IS_DIRECTORY ${sample} )
        if( EXISTS ${sample}/CMakeLists.txt )
          # extract the name from the path
          string( REGEX REPLACE "^.*/([^/]*)$" "\\1" SAMPLE_NAME ${sample} )
            if( (NOT (${SAMPLE_NAME} MATCHES "^__")) AND (NOT(${SAMPLE_NAME} STREQUAL "build_all")) AND (NOT(${SAMPLE_NAME} STREQUAL "shared_sources")) AND (NOT(${SAMPLE_NAME} STREQUAL "downloaded_resources")))
              # build the option to include the project or not
              set( "BUILD_${SAMPLE_NAME}" TRUE CACHE BOOL "Include the ${SAMPLE_NAME} sample" )
              # add the project
              if( BUILD_${SAMPLE_NAME})
                add_subdirectory( ${_PATH}/${SAMPLE_NAME} ./${SAMPLE_NAME})
              endif()
            endif()
        endif()
      endif()
  ENDFOREACH( sample ${samples} )
endmacro(_addFolder)
##############################
#### projects ###
##############################
_addFolder("..")

add_subdirectory(../shared_sources ${CMAKE_BINARY_DIR}/shared_sources)
