# Copyright © 2013 Canonical Ltd.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 3 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
#
# Authored by: Thomas Voss <thomas.voss@canonical.com>

if(IS_DIRECTORY /usr/src/googletest/googlemock)
  set(gmock_srcdir /usr/src/googletest/googlemock)
  set(gtest_srcdir /usr/src/googletest/googletest)
elseif(IS_DIRECTORY /usr/src/gmock)
  set(gmock_srcdir /usr/src/gmock)
  set(gtest_srcdir /usr/src/gmock/gtest)
else()
  message(FATAL_ERROR "Can't find gmock. Where is it?")
endif()

# Build with system gmock and embedded gtest
set (GMOCK_INCLUDE_DIR "${gmock_srcdir}/include" CACHE PATH "gmock source include directory")
set (GMOCK_SOURCE_DIR "${gmock_srcdir}" CACHE PATH "gmock source directory")
set (GTEST_INCLUDE_DIR "${gtest_srcdir}/include" CACHE PATH "gtest source include directory")

# Google Test 1.8.0 release includes commit 794ef03 that added an
# extra variadic argument to macro INSTANTIATE_TEST_CASE_P.
# See https://github.com/google/googletest/commit/794ef030ebad671b699abdfd8e0474f93045be91
execute_process(
  COMMAND sh -c "test -n \"\$(cat ${GTEST_INCLUDE_DIR}/gtest/gtest-param-test.h | grep '^# *define \\+INSTANTIATE_TEST_CASE_P(.*\\.\\.\\.)')\" && echo YES || true"
  OUTPUT_VARIABLE gtest_HAVE_NAMED_VP_TESTS
  OUTPUT_STRIP_TRAILING_WHITESPACE
  ERROR_QUIET
)
if(gtest_HAVE_NAMED_VP_TESTS)
  add_definitions(-DGTEST_HAVE_NAMED_VP_TESTS)
endif()

add_subdirectory(${GMOCK_SOURCE_DIR} "${CMAKE_CURRENT_BINARY_DIR}/gmock" EXCLUDE_FROM_ALL)

set(GTEST_BOTH_LIBRARIES gmock gtest gtest_main)
find_package(Threads)

add_definitions(-DCORE_DBUS_ENABLE_GOOGLE_TEST_FIXTURE)

configure_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/test_data.h.in
  ${CMAKE_CURRENT_BINARY_DIR}/test_data.h @ONLY)

include_directories(
  ${CMAKE_CURRENT_BINARY_DIR}
)

include_directories(
  ${DBUS_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${GTEST_INCLUDE_DIRS}
  ${PROCESS_CPP_INCLUDE_DIRS}
)

add_executable(
  async_execution_load_test
  async_execution_load_test.cpp
  )

add_executable(
  bus_test
  bus_test.cpp
  )

add_executable(
  cache_test
  cache_test.cpp
  )

add_executable(
  dbus_test
  dbus_test.cpp
  )

add_executable(
  codec_test
  codec_test.cpp
  )

add_executable(
  compiler_test
  compiler_test.cpp
  )

add_executable(
  executor_test
  executor_test.cpp
  )

add_executable(
  stl_codec_test
  stl_codec_test.cpp
  )

add_executable(
  message_test
  message_test.cpp   
  )

add_executable(
  match_rule_test
  match_rule_test.cpp
  )

add_executable(
  message_router_test
  message_router_test.cpp
  )

add_executable(
  service_test
  service_test.cpp
  )

add_executable(
  service_watcher_test
  service_watcher_test.cpp
  )

add_executable(
  types_test
  types_test.cpp
  )

add_executable(
  signal_delivery_test
  signal_delivery_test.cpp
  )

target_link_libraries(
  async_execution_load_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  ${PROCESS_CPP_LIBRARIES}
)

target_link_libraries(
  bus_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  ${PROCESS_CPP_LIBRARIES}
)

target_link_libraries(
  cache_test

  dbus-cpp

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  ${PROCESS_CPP_LIBRARIES}
)

target_link_libraries(
  dbus_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  executor_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  codec_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  compiler_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${LIBXML2_LIBRARIES}
  ${GMOCK_LIBRARY}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  stl_codec_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  message_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  match_rule_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  message_router_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  service_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  service_watcher_test

  dbus-cpp
  dbus-cppc-helper

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  types_test

  dbus-cpp

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

target_link_libraries(
  signal_delivery_test

  dbus-cpp

  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
  ${DBUS_LIBRARIES}
  ${GTEST_BOTH_LIBRARIES}
  )

add_test(async_execution_load_test ${CMAKE_CURRENT_BINARY_DIR}/async_execution_load_test)
add_test(bus_test ${CMAKE_CURRENT_BINARY_DIR}/bus_test)
add_test(cache_test ${CMAKE_CURRENT_BINARY_DIR}/cache_test)
add_test(dbus_test ${CMAKE_CURRENT_BINARY_DIR}/dbus_test)
add_test(executor_test ${CMAKE_CURRENT_BINARY_DIR}/executor_test)
add_test(codec_test ${CMAKE_CURRENT_BINARY_DIR}/codec_test)
add_test(compiler_test ${CMAKE_CURRENT_BINARY_DIR}/compiler_test)
add_test(stl_codec_test ${CMAKE_CURRENT_BINARY_DIR}/stl_codec_test)
add_test(types_test ${CMAKE_CURRENT_BINARY_DIR}/types_test)
add_test(message_test ${CMAKE_CURRENT_BINARY_DIR}/message_test)
add_test(match_rule_test ${CMAKE_CURRENT_BINARY_DIR}/match_rule_test)
add_test(message_router_test ${CMAKE_CURRENT_BINARY_DIR}/message_router_test)
add_test(service_test ${CMAKE_CURRENT_BINARY_DIR}/service_test)
add_test(service_watcher_test ${CMAKE_CURRENT_BINARY_DIR}/service_watcher_test)
add_test(signal_delivery_test ${CMAKE_CURRENT_BINARY_DIR}/signal_delivery_test)
