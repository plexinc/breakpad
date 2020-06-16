add_executable(dump_syms dump_syms/dump_syms.cc)
target_link_libraries(dump_syms
  PRIVATE
  breakpad_common
  breakpad_interface
)

add_executable(core2md core2md/core2md.cc)
target_link_libraries(core2md
  PRIVATE
  breakpad_client
  breakpad_common
  breakpad_interface
)

add_executable(md2core md2core/minidump-2-core.cc)
target_link_libraries(md2core
  PRIVATE
  breakpad_client
  breakpad_common
  breakpad_interface
)
