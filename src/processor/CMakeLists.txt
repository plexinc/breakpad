add_library(breakpad_process STATIC)

target_link_libraries(breakpad_process
  PUBLIC
  breakpad_common
  breakpad_interface
)

target_sources(breakpad_process
  PRIVATE
  basic_code_modules.cc
  basic_source_line_resolver.cc
  call_stack.cc
  cfi_frame_info.cc
  convert_old_arm64_context.cc
  disassembler_x86.cc
  dump_context.cc
  dump_object.cc
  exploitability.cc
  exploitability_linux.cc
  exploitability_win.cc
  fast_source_line_resolver.cc
  logging.cc
  microdump_processor.cc
  microdump.cc
  minidump.cc
  minidump_processor.cc
  module_comparer.cc
  module_serializer.cc
  pathname_stripper.cc
  proc_maps_linux.cc
  process_state.cc
  simple_symbol_supplier.cc
  source_line_resolver_base.cc
  stack_frame_cpu.cc
  stack_frame_symbolizer.cc
  stackwalk_common.cc
  stackwalker.cc
  stackwalker_address_list.cc
  stackwalker_amd64.cc
  stackwalker_arm.cc
  stackwalker_arm64.cc
  stackwalker_mips.cc
  stackwalker_ppc.cc
  stackwalker_ppc64.cc
  stackwalker_selftest.cc
  stackwalker_sparc.cc
  stackwalker_x86.cc
  symbolic_constants_win.cc
  synth_minidump.cc
  tokenize.cc
)

add_executable(minidump_stackwalk minidump_stackwalk.cc)
target_link_libraries(minidump_stackwalk PRIVATE breakpad_process disasm)

if(BUILD_TESTING)
  macro(breakpad_add_ut NAME)
    add_executable(${NAME} ${NAME}.cc)

    target_link_libraries(${NAME}
      PRIVATE
      breakpad_interface
      breakpad_process
      gmock
      gtest
      disasm
    )
  endmacro()

  set(tests
    address_map_unittest
    basic_source_line_resolver_unittest
    cfi_frame_info_unittest
    contained_range_map_unittest
    disassembler_x86_unittest
    exploitability_unittest
    fast_source_line_resolver_unittest
    map_serializers_unittest
    microdump_processor_unittest
    minidump_processor_unittest
    minidump_unittest
    pathname_stripper_unittest
    postfix_evaluator_unittest
    range_map_truncate_lower_unittest
    range_map_truncate_upper_unittest
    range_map_unittest
    stackwalker_address_list_unittest
    stackwalker_amd64_unittest
    stackwalker_arm64_unittest
    stackwalker_arm_unittest
    stackwalker_mips_unittest
    stackwalker_mips64_unittest
    stackwalker_x86_unittest
    static_address_map_unittest
    static_contained_range_map_unittest
    static_map_unittest
    static_range_map_unittest
    synth_minidump_unittest
  )

  foreach(unittest ${tests})
    breakpad_add_ut(${unittest})
  endforeach()
endif()