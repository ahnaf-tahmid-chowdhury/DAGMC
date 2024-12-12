cmake_minimum_required(VERSION 3.12)

project(DAGMC)

# Set DAGMC version
set(DAGMC_MAJOR_VERSION 3)
set(DAGMC_MINOR_VERSION 2)
set(DAGMC_PATCH_VERSION 3)
set(DAGMC_VERSION ${DAGMC_MAJOR_VERSION}.${DAGMC_MINOR_VERSION}.${DAGMC_PATCH_VERSION})

enable_language(CXX)

if(MSVC)
  set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
endif()


# Set DAGMC Git SHA

# Set git SHA1 hash as a compile definition
find_package(Git)
if(GIT_FOUND AND EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/.git")
  execute_process(COMMAND ${GIT_EXECUTABLE} rev-parse HEAD
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    RESULT_VARIABLE DAGMC_GIT_SHA_SUCCESS
    OUTPUT_VARIABLE DAGMC_GIT_SHA
    ERROR_QUIET OUTPUT_STRIP_TRAILING_WHITESPACE)
  if(NOT DAGMC_GIT_SHA_SUCCESS EQUAL "0")
    message(WARNING "Could not determine the commit SHA for DAGMC.")
    set(DAGMC_GIT_SHA "")
  endif()

  option(GIT_SUBMODULE "Check submodules during build" ON)
  if(GIT_SUBMODULE)
    message(STATUS "Submodule update")
    execute_process(COMMAND ${GIT_EXECUTABLE} "submodule" "update" "--init" "--recursive"
                    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                    RESULT_VARIABLE GIT_SUBMOD_RESULT)
    if(NOT GIT_SUBMOD_RESULT EQUAL 0)
      message(FATAL_ERROR "git submodule update --init --recursive failed with \
        ${GIT_SUBMOD_RESULT}, please checkout submodules")
    endif()
  endif()
endif()

# Check to see if submodules exist (by checking one)
if(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/src/pyne/pyne/readme.rst")
  message(FATAL_ERROR "The git submodules were not downloaded! GIT_SUBMODULE was \
    turned off or failed. Please update submodules and try again.")
endif()

# Make the scripts in the "cmake" directory available to CMake
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)
include(DAGMC_macros)

dagmc_setup_build()

dagmc_setup_options()

if(BUILD_MCNP5 OR BUILD_MCNP6)
  enable_language(Fortran)
endif()

# Find HDF5
find_package(HDF5 REQUIRED COMPONENTS C HL)  

set(HDF5_LIBRARIES_SHARED ${HDF5_LIBRARIES})
# CMake doesn't let you find_package(HDF5) twice so we have to do this instead
if (BUILD_STATIC_LIBS)
  string(REPLACE ${CMAKE_SHARED_LIBRARY_SUFFIX} ${CMAKE_STATIC_LIBRARY_SUFFIX}
         HDF5_LIBRARIES_STATIC "${HDF5_LIBRARIES_SHARED}")
endif ()
if (NOT BUILD_SHARED_LIBS)
  set(HDF5_LIBRARIES_SHARED)
endif ()

message(STATUS "HDF5_INCLUDE_DIRS: ${HDF5_INCLUDE_DIRS}")
message(STATUS "HDF5_LIBRARIES_SHARED: ${HDF5_LIBRARIES_SHARED}")
message(STATUS "HDF5_LIBRARIES_STATIC: ${HDF5_LIBRARIES_STATIC}")

include_directories(${HDF5_INCLUDE_DIRS})

# Find CURL for MOAB
find_package(CURL REQUIRED)

# Find MOAB
find_package(MOAB REQUIRED)

if(BUILD_SHARED_LIBS)
  set(MOAB_LIBRARIES_SHARED MOAB)
  if(WIN32)
    add_definitions(-DMOAB_EXPORTS)
  endif()
else()
  set(MOAB_LIBRARIES_STATIC MOAB)
endif()

message(STATUS "MOAB_INCLUDE_DIRS: ${MOAB_INCLUDE_DIRS}")
message(STATUS "MOAB_LIBRARIES_SHARED: ${MOAB_LIBRARIES_SHARED}")
message(STATUS "MOAB_LIBRARIES_STATIC: ${MOAB_LIBRARIES_STATIC}")

include_directories(${MOAB_INCLUDE_DIRS})

# Find OpenMP
find_package(OpenMP)

dagmc_setup_flags()

if (BUILD_TESTS)
  enable_testing()
endif ()

add_subdirectory(src)

dagmc_make_configure_files()

message("")
