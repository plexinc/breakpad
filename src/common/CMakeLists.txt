if(UNIX)
  set(common_SRC
    dwarf_cfi_to_module.cc 
    dwarf_cu_to_module.cc 
    dwarf_line_to_module.cc 
    language.cc 
    module.cc 
    stabs_reader.cc 
    stabs_to_module.cc 
    dwarf/bytereader.cc 
    dwarf/dwarf2diehandler.cc 
    dwarf/dwarf2reader.cc 
  )
  
  file(GLOB DWARF_HEADERS dwarf/*.h)
  install(FILES ${DWARF_HEADERS} DESTINATION include/breakpad/common/dwarf)
endif(UNIX)

if(APPLE)
  list(APPEND common_SRC
    convert_UTF.c
    md5.cc
    string_conversion.cc
    mac/arch_utilities.cc
    mac/GTMLogger.m
    mac/file_id.cc
    mac/HTTPMultipartUpload.m
    mac/MachIPC.mm
    mac/bootstrap_compat.cc
    mac/macho_id.cc
    mac/macho_utilities.cc
    mac/macho_walker.cc
    mac/macho_reader.cc
    mac/string_utilities.cc
    mac/dump_syms.mm
  )

  file(GLOB MAC_HEADERS mac/*.h)
  install(FILES ${MAC_HEADERS} DESTINATION include/breakpad/common/mac)

elseif(UNIX)
  list(APPEND common_SRC
    linux/crc32.cc
    linux/dump_symbols.cc 
    linux/elf_symbols_to_module.cc 
    linux/elfutils.cc 
    linux/file_id.cc 
    linux/linux_libc_support.cc 
    linux/memory_mapped_file.cc 
    linux/safe_readlink.cc 
  )
  file(GLOB LINUX_HEADERS linux/*.h)
  install(FILES ${LINUX_HEADERS} DESTINATION include/breakpad/common/linux)
elseif(WIN32)
  list(APPEND common_SRC
    windows/dia_util.cc
    windows/guid_string.cc
    windows/omap.cc
    windows/pdb_source_line_writer.cc
    windows/string_utils.cc
    windows/http_upload.cc
  )
  file(GLOB WIN_HEADERS windows/*.h)
  install(FILES ${WIN_HEADERS} DESTINATION include/breakpad/common/windows)
endif(APPLE)

add_library(common OBJECT ${common_SRC})

file(GLOB HEADERS *.h)
install(FILES ${HEADERS} DESTINATION include/breakpad/common)

