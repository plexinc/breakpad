set(MSVC_DIA_SDK_DIR "$ENV{VSINSTALLDIR}DIA SDK")
find_library(DIAGUIDS NAME diaguids.lib HINTS ${MSVC_DIA_SDK_DIR}/lib/amd64)

add_executable(dump_syms 
	dump_syms/dump_syms.cc
	../../common/windows/guid_string.cc
	../../common/windows/pdb_source_line_writer.cc
	../../common/windows/string_utils.cc)
	
target_link_libraries(dump_syms ${DIAGUIDS} imagehlp.lib breakpad_client)

install(TARGETS dump_syms DESTINATION bin)
