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
  linux/dump_symbols.cc 
  linux/elf_symbols_to_module.cc 
  linux/elfutils.cc 
  linux/file_id.cc 
  linux/linux_libc_support.cc 
  linux/memory_mapped_file.cc 
  linux/safe_readlink.cc 
)

add_library(common STATIC ${common_SRC})

file(GLOB HEADERS *.h)
install(FILES ${HEADERS} DESTINATION include/breakpad/common)

file(GLOB LINUX_HEADERS linux/*.h)
install(FILES ${LINUX_HEADERS} DESTINATION include/breakpad/common/linux)

file(GLOB DWARF_HEADERS dwarf/*.h)
install(FILES ${DWARF_HEADERS} DESTINATION include/breakpad/common/dwarf)
