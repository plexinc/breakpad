add_executable(dump_syms dump_syms/dump_syms.cc)
target_link_libraries(dump_syms common)

add_executable(core2md
  core2md/core2md.cc
  ${PROJECT_SOURCE_DIR}/src/client/linux/minidump_writer/linux_core_dumper.cc
  ${PROJECT_SOURCE_DIR}/src/common/linux/elf_core_dump.cc
)
target_link_libraries(core2md breakpad_client common)

add_executable(md2core md2core/minidump-2-core.cc)
target_link_libraries(md2core common)

add_library(http_upload STATIC ${PROJECT_SOURCE_DIR}/src/common/linux/http_upload.cc)

add_executable(minidump_upload symupload/minidump_upload.cc)
target_link_libraries(minidump_upload dl http_upload)

add_executable(sym_upload symupload/sym_upload.cc)
target_link_libraries(sym_upload dl http_upload)

install(TARGETS dump_syms core2md md2core minidump_upload sym_upload DESTINATION bin)
