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

install(FILES linux/handler/exception_handler.h
              linux/handler/minidump_descriptor.h
        DESTINATION include/breakpad/linux/handler)

install(FILES linux/crash_generation/crash_generation_server.h
              linux/crash_generation/crash_generation_client.h
        DESTINATION include/breakpad/linux/crash_generation)

install(FILES linux/minidump_writer/minidump_writer.h
        DESTINATION include/breakpad/linux/minidump_write)

