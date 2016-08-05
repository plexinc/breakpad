cmake_minimum_required(VERSION 3.5)
Project(breakpad)

if(WIN32)
  # Windows only recipe
  add_library(breakpad STATIC
    src/client/windows/common/auto_critical_section.h
    src/client/windows/common/ipc_protocol.h
    src/client/windows/crash_generation/client_info.h
    src/client/windows/crash_generation/client_info.cc
    src/client/windows/crash_generation/crash_generation_client.h
    src/client/windows/crash_generation/crash_generation_client.cc
    src/client/windows/crash_generation/crash_generation_server.h
    src/client/windows/crash_generation/crash_generation_server.cc
    src/client/windows/crash_generation/minidump_generator.h
    src/client/windows/crash_generation/minidump_generator.cc
    src/client/windows/sender/crash_report_sender.h
    src/client/windows/sender/crash_report_sender.cc
    src/client/windows/handler/exception_handler.h
    src/client/windows/handler/exception_handler.cc
    src/common/windows/dia_util.h
    src/common/windows/dia_util.cc
    src/common/windows/omap.h
    src/common/windows/omap.cc
    src/common/windows/omap_internal.h
    src/common/windows/guid_string.h
    src/common/windows/guid_string.cc
    src/common/windows/http_upload.h
    src/common/windows/http_upload.cc
    src/common/windows/pdb_source_line_writer.h
    src/common/windows/pdb_source_line_writer.cc
    src/common/windows/string_utils-inl.h
    src/common/windows/string_utils.cc
  )
  target_include_directories(breakpad PUBLIC src "/Program Files (x86)/Microsoft Visual Studio 14.0/DIA SDK/include")
  target_compile_definitions(breakpad PRIVATE _UNICODE UNICODE _SILENCE_STDEXT_HASH_DEPRECATION_WARNINGS)

  add_executable(dump_syms
    src/tools/windows/dump_syms/dump_syms.cc
    src/common/windows/guid_string.h
    src/common/windows/guid_string.cc
    src/common/windows/pdb_source_line_writer.h
    src/common/windows/pdb_source_line_writer.cc
    src/common/windows/string_utils-inl.h
    src/common/windows/string_utils.cc
  )
  target_include_directories(dump_syms PUBLIC src "/Program Files (x86)/Microsoft Visual Studio 14.0/DIA SDK/include")
  target_compile_definitions(dump_syms PRIVATE _UNICODE UNICODE _SILENCE_STDEXT_HASH_DEPRECATION_WARNINGS)
  
  find_library(IMAGEHLP_LIB NAMES imagehlp)
  target_link_libraries(dump_syms breakpad ${IMAGEHLP_LIB} "/Program Files (x86)/Microsoft Visual Studio 14.0/DIA SDK/lib/diaguids.lib")

else()
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
endif()
