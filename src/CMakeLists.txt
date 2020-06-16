
option(BUILD_TOOLS OFF "Want to build programs or not dude?")
option(BUILD_TESTING OFF "Want to build tests")

add_subdirectory(client)
add_subdirectory(common)
add_subdirectory(third_party)

if(BUILD_TOOLS)
  add_subdirectory(processor)
  add_subdirectory(tools)
endif()

if(BUILD_TESTING)
  add_library(gtest STATIC testing/googletest/src/gtest-all.cc)
  target_include_directories(gtest PUBLIC testing/googletest testing/googletest/include)
  add_library(gtest_main STATIC testing/googletest/src/gtest_main.cc)
  target_include_directories(gtest_main PUBLIC testing/googletest testing/googletest/include)

  add_library(gmock STATIC testing/googlemock/src/gmock-all.cc)
  target_include_directories(gmock PUBLIC testing/googlemock testing/googlemock/include)
  target_link_libraries(gmock PRIVATE gtest)
  add_library(gmock_main STATIC testing/googlemock/src/gmock_main.cc)
  target_include_directories(gmock_main PUBLIC testing/googlemock testing/googlemock/include)
  target_link_libraries(gmock_main PRIVATE gtest)
endif()