add_library(breakpad_client STATIC
  linux/crash_generation/crash_generation_client.cc 
  linux/crash_generation/crash_generation_server.cc 
  linux/handler/exception_handler.cc 
  linux/handler/minidump_descriptor.cc 
  linux/log/log.cc 
  linux/minidump_writer/linux_dumper.cc 
  linux/minidump_writer/linux_ptrace_dumper.cc 
  linux/minidump_writer/minidump_writer.cc 
  minidump_file_writer.cc 

  ${PROJECT_SOURCE_DIR}/src/common/convert_UTF.c 
  ${PROJECT_SOURCE_DIR}/src/common/md5.cc 
  ${PROJECT_SOURCE_DIR}/src/common/string_conversion.cc 
  ${PROJECT_SOURCE_DIR}/src/common/linux/elfutils.cc 
  ${PROJECT_SOURCE_DIR}/src/common/linux/file_id.cc 
  ${PROJECT_SOURCE_DIR}/src/common/linux/guid_creator.cc 
  ${PROJECT_SOURCE_DIR}/src/common/linux/linux_libc_support.cc 
  ${PROJECT_SOURCE_DIR}/src/common/linux/memory_mapped_file.cc 
  ${PROJECT_SOURCE_DIR}/src/common/linux/safe_readlink.cc
)

install(TARGETS breakpad_client DESTINATION lib)

file(GLOB HEADERS linux/handler/*h)
install(FILES ${HEADERS}
        DESTINATION include/breakpad/client/linux/handler)

file(GLOB HEADERS linux/crash_generation/*h)
install(FILES ${HEADERS}
        DESTINATION include/breakpad/client/linux/crash_generation)

file(GLOB HEADERS linux/minidump_writer/*h)
install(FILES ${HEADERS}
        DESTINATION include/breakpad/client/linux/minidump_writer)

