add_library(breakpad_client STATIC)

target_link_libraries(breakpad_client
  PRIVATE
  breakpad_common
  breakpad_interface
)

if(UNIX)
  target_sources(breakpad_client PRIVATE
    minidump_file_writer.cc
  )
endif()

if(APPLE)
  target_sources(breakpad_client
    PRIVATE
    mac/crash_generation/ConfigFile.mm
    mac/crash_generation/crash_generation_client.cc
    mac/crash_generation/crash_generation_server.cc
    mac/crash_generation/Inspector.mm
    mac/crash_generation/InspectorMain.mm
    mac/handler/breakpad_nlist_64.cc
    mac/handler/dynamic_images.cc
    mac/handler/exception_handler.cc
    mac/handler/minidump_generator.cc
    mac/handler/protected_memory_allocator.cc
  )

  target_include_directories(breakpad_client PRIVATE apple/Framework)
endif()

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