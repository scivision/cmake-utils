cmake_minimum_required(VERSION 3.14...3.21)

project(FetchContent_Demo
  LANGUAGES Fortran
  DESCRIPTION "Demonstrate that CMake SSL is installed and working with FetchContent")

enable_testing()
# --- sub-projects
include(FetchContent)

FetchContent_Declare(child
  GIT_REPOSITORY https://github.com/scivision/cmake-fetchcontent.git
  GIT_TAG ffe65778a55af69253885d47330c5b1a68a331c3
  GIT_SHALLOW true
  UPDATE_DISCONNECTED true  # avoid constant rebuild
)

FetchContent_MakeAvailable(child)

# -- main project
add_executable(test_timestwo_fc test_timestwo.f90)
target_link_libraries(test_timestwo_fc times_two)
add_test(NAME TimesTwoFetch COMMAND test_timestwo_fc)

# -- data
# This MUST be a .zip, .tar, .tar.gz or similar archive, NOT just a file itself!
# you will find the extracted data files under bin/_deps/demodata-src/
FetchContent_Declare(demodata
  URL https://github.com/scivision/cmake-fetchcontent/raw/main/octo.zip
  URL_HASH SHA256=8a1df15d6ed7ae28ad872e5425adbcb31573ede1e84c21121a88de38a293bd97
)
if(NOT demodata_POPULATED)
  FetchContent_Populate(demodata)
endif()
