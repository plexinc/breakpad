add_library(breakpad_client STATIC)

target_link_libraries(breakpad_client
  PRIVATE
  breakpad_common
  breakpad_interface
)

#target_sources(breakpad_client PRIVATE
#  minidump_file_writer.cc
#)

if(UNIX AND NOT APPLE)
  target_sources(breakpad_client
    PRIVATE
    linux/crash_generation/crash_generation_client.cc
    linux/crash_generation/crash_generation_server.cc
    linux/dump_writer_common/thread_info.cc
    linux/dump_writer_common/ucontext_reader.cc
    linux/handler/exception_handler.cc
    linux/handler/minidump_descriptor.cc
    linux/log/log.cc
    linux/microdump_writer/microdump_writer.cc
    linux/minidump_writer/linux_core_dumper.cc
    linux/minidump_writer/linux_dumper.cc
    linux/minidump_writer/linux_ptrace_dumper.cc
    linux/minidump_writer/minidump_writer.cc
  )
endif()

if(WIN32)
  target_sources(breakpad_client
    PRIVATE
    windows/crash_generation/client_info.cc
    windows/crash_generation/crash_generation_client.cc
    windows/crash_generation/crash_generation_server.cc
    windows/crash_generation/minidump_generator.cc
    windows/handler/exception_handler.cc

  )
endif()