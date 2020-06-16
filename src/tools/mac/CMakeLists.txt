add_executable(dump_syms dump_syms/dump_syms_tool.cc)
target_link_libraries(dump_syms
  PRIVATE
  breakpad_common
  breakpad_interface
)
