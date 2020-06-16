add_executable(dump_syms dump_syms/dump_syms_tool.cc)
target_link_libraries(dump_syms
  PRIVATE
  breakpad_common
  breakpad_interface
)

add_executable(macho_dump dump_syms/macho_dump.cc)
target_link_libraries(macho_dump
  PRIVATE
  breakpad_common
  breakpad_interface
)
