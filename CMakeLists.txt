cmake_minimum_required(VERSION 3.20)

project(pFUnit_bug_test
  VERSION 0.1.0
  LANGUAGES Fortran
)

# To trigger the bug, set CMAKE_RUNTIME_OUTPUT_DIRECTORY
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")

option(USE_PLASMAFAIR "Use PlasmaFAIR fix repo instead of the original" OFF)
option(USE_FETCHCONTENT "Download pFUnit on-the-fly using FetchContent" OFF)

if(USE_FETCHCONTENT)

  if(USE_PLASMAFAIR)
    set(PFUNIT_REPO "https://github.com/PlasmaFAIR/pFUnit.git")
    set(PFUNIT_TAG "bugfix/posix_predefined_not_found")
  else()
    set(PFUNIT_REPO "https://github.com/Goddard-Fortran-Ecosystem/pFUnit.git")
    set(PFUNIT_TAG "v4.8.0")
  endif()

  include(FetchContent)
  FetchContent_Declare(pfunit GIT_REPOSITORY ${PFUNIT_REPO} GIT_TAG ${PFUNIT_TAG})
  FetchContent_MakeAvailable(pfunit)

else()

  if(USE_PLASMAFAIR)
    add_subdirectory("external/pFUnit_PlasmaFAIR")
  else()
    add_subdirectory("external/pFUnit")
  endif()

endif()

include(CTest)
enable_testing()

add_pfunit_ctest(my_test TEST_SOURCES "my_test.pf")
