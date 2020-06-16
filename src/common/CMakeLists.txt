add_library(breakpad_common STATIC)

target_link_libraries(breakpad_common PRIVATE breakpad_interface)

target_sources(breakpad_common
  PRIVATE
  convert_UTF.cc
  dwarf_cfi_to_module.cc
  dwarf_cu_to_module.cc
  dwarf_line_to_module.cc
  dwarf_range_list_handler.cc
  language.cc
  long_string_dictionary.cc
  md5.cc
  module.cc
  simple_string_dictionary.cc
  string_conversion.cc
  test_assembler.cc
  path_helper.cc
)

if(ANDROID)
  target_sources(breakpad_common
    PRIVATE
    android/include/elf.h
    android/include/link.h
    android/include/stab.h
    android/include/sys/procfs.h
    android/include/sys/user.h
    android/testing/include/wchar.h
    android/testing/mkdtemp.h
    android/testing/pthread_fixes.h
    android/ucontext_constants.h
  )
endif()

if(UNIX)
  target_sources(breakpad_common
    PRIVATE
    dwarf/bytereader.cc
    dwarf/cfi_assembler.cc
    dwarf/dwarf2diehandler.cc
    dwarf/dwarf2reader.cc
    dwarf/elf_reader.cc
    dwarf/functioninfo.cc
    stabs_reader.cc
    stabs_to_module.cc
  )
endif()

if(APPLE)
  target_sources(breakpad_common
    PRIVATE
    mac/arch_utilities.cc
    mac/bootstrap_compat.cc
    mac/dump_syms.cc
    mac/file_id.cc
    mac/GTMLogger.m
    mac/HTTPMultipartUpload.m
    mac/MachIPC.mm
    mac/macho_id.cc
    mac/macho_reader.cc
    mac/macho_utilities.cc
    mac/macho_walker.cc
    mac/string_utilities.cc
  )
  target_include_directories(breakpad_common PUBLIC mac)
endif()

if(MSVC)
  target_sources(breakpad_common
    PRIVATE
    windows/common_windows.gyp
    windows/dia_util.cc
    windows/guid_string.cc
    windows/http_upload.cc
    windows/omap.cc
    windows/pdb_source_line_writer.cc
    windows/string_utils-inl.h
    windows/string_utils.cc
  )
endif()

if(UNIX AND NOT APPLE)
  target_sources(breakpad_common
    PRIVATE
    linux/breakpad_getcontext.S
    linux/crc32.cc
    linux/dump_symbols.cc
    linux/elf_core_dump.cc
    linux/elf_gnu_compat.h
    linux/elf_symbols_to_module.cc
    linux/elfutils.cc
    linux/file_id.cc
    linux/google_crashdump_uploader.cc
    linux/guid_creator.cc
    linux/http_upload.cc
    linux/libcurl_wrapper.cc
    linux/linux_libc_support.cc
    linux/memory_mapped_file.cc
    linux/safe_readlink.cc
    linux/symbol_collector_client.cc
    linux/synth_elf.cc
)
endif()

if(BUILD_TESTING)

  add_executable(breakpad_common_test
    byte_cursor_unittest.cc
    long_string_dictionary_unittest.cc
    memory_allocator_unittest.cc
    memory_range_unittest.cc
    module_unittest.cc
    simple_string_dictionary_unittest.cc
    stabs_reader_unittest.cc
    stabs_to_module_unittest.cc
    string_conversion_unittest.cc
    test_assembler_unittest.cc
    tests/auto_tempdir.h
    tests/file_utils.cc
    tests/file_utils.h
  )

  target_link_libraries(breakpad_common_test
    PRIVATE
    breakpad_common
    breakpad_interface
    gmock
    gmock_main
    gtest
  )

  if(UNIX AND NOT APPLE)
    target_sources(breakpad_common_test
      PRIVATE
      linux/breakpad_getcontext_unittest.cc
      linux/dump_symbols_unittest.cc
      linux/elf_core_dump_unittest.cc
      linux/elf_symbols_to_module_unittest.cc
      linux/file_id_unittest.cc
      linux/google_crashdump_uploader_test.cc
      linux/linux_libc_support_unittest.cc
      linux/memory_mapped_file_unittest.cc
      linux/safe_readlink_unittest.cc
      linux/synth_elf_unittest.cc
      linux/tests/auto_testfile.h
      linux/tests/crash_generator.cc
      linux/tests/crash_generator.h
    )
  endif()

endif()
