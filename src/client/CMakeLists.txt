
set(COMMON_SRC minidump_file_writer.cc)

if(APPLE)
  add_library(breakpad_client STATIC
    ${COMMON_SRC}
    $<TARGET_OBJECTS:common>
    mac/handler/breakpad_nlist_64.cc
    mac/handler/dynamic_images.cc
    mac/handler/exception_handler.cc
    mac/handler/minidump_generator.cc
    mac/handler/protected_memory_allocator.cc
    mac/crash_generation/crash_generation_client.cc
    mac/crash_generation/crash_generation_server.cc
    mac/crash_generation/ConfigFile.mm
    mac/crash_generation/InspectorMain.mm
    mac/crash_generation/Inspector.mm
  )
  file(GLOB HEADERS mac/handler/*h)
  install(FILES ${HEADERS} DESTINATION include/breakpad/client/mac/handler)

  file(GLOB HEADERS mac/crash_generation/*h)
  install(FILES ${HEADERS} DESTINATION include/breakpad/client/mac/crash_generation)

  file(GLOB HEADERS mac/crash_generation/*h)
  install(FILES ${HEADERS} DESTINATION include/breakpad/client/mac/crash_generation)
else(APPLE)
  add_library(breakpad_client STATIC
    linux/crash_generation/crash_generation_client.cc 
    linux/crash_generation/crash_generation_server.cc 
    linux/handler/exception_handler.cc 
    linux/handler/minidump_descriptor.cc 
    linux/log/log.cc 
    linux/minidump_writer/linux_dumper.cc 
    linux/minidump_writer/linux_ptrace_dumper.cc 
    linux/minidump_writer/minidump_writer.cc 
    ${COMMON_SRC}

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
  file(GLOB HEADERS linux/handler/*h)
  install(FILES ${HEADERS}
          DESTINATION include/breakpad/client/linux/handler)

  file(GLOB HEADERS linux/crash_generation/*h)
  install(FILES ${HEADERS}
          DESTINATION include/breakpad/client/linux/crash_generation)

  file(GLOB HEADERS linux/minidump_writer/*h)
  install(FILES ${HEADERS}
          DESTINATION include/breakpad/client/linux/minidump_writer)

endif(APPLE)

install(TARGETS breakpad_client DESTINATION lib)

