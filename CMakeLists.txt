Project(breakpad)
cmake_minimum_required(VERSION 2.8)

# find some include files
include(CheckIncludeFiles)
check_include_files(a.out.h HAVE_A_OUT_H)
check_include_files(inttypes.h HAVE_INTTYPES_H)
check_include_files(memory.h HAVE_MEMORY_H)
check_include_files(stdint.h HAVE_STDINT_H)
check_include_files(stdlib.h HAVE_STDLIB_H)
check_include_files(strings.h HAVE_STRINGS_H)
check_include_files(string.h HAVE_STRING_H)
check_include_files(sys/stat.h HAVE_SYS_STAT_H)
check_include_files(sys/types.h HAVE_SYS_TYPES_H)
check_include_files(unistd.h HAVE_UNISTD_H)
check_include_files(cpuid.h HAVE_CPUID_H)

# check for threads
find_package(Threads)

configure_file(src/config.h.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/config.h)

include(CheckSymbolExists)
check_symbol_exists(ULLONG_MAX "limits.h" HAVE_ULLONG_MAX)
if(NOT HAVE_ULLONG_MAX)
  add_definitions(-DULLONG_MAX=18446744073709551615ULL)
endif(NOT HAVE_ULLONG_MAX)

add_definitions(-DHAVE_CONFIG_H)
include_directories(src ${CMAKE_CURRENT_BINARY_DIR})

if(APPLE)
	add_definitions(-DHAVE_MACH_O_NLIST_H)
	include_directories(src/common/mac src/client/apple/Framework)
endif(APPLE)

add_subdirectory(src/client)
add_subdirectory(src/common)
add_subdirectory(src/third_party)
add_subdirectory(src/google_breakpad/common)

if(APPLE)
	add_subdirectory(src/tools/mac)
elseif(UNIX)
	add_subdirectory(src/tools/linux)
endif(APPLE)

set(PACKAGE_NAME breakpad)
set(PACKAGE_VERSION 1.0)
set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix "\${prefix}/bin")
set(libdir "\${prefix}/lib")
set(includedir "\${prefix}/include")
set(PTHREAD_LIBS ${CMAKE_THREAD_LIBS_INIT})
set(PTHREAD_CFLAGS "")

configure_file(breakpad-client.pc.in breakpad-client.pc)
configure_file(breakpad.pc.in breakpad.pc)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/breakpad-client.pc ${CMAKE_CURRENT_BINARY_DIR}/breakpad.pc DESTINATION lib/pkgconfig)