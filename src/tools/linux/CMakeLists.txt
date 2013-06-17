add_executable(dump_syms dump_syms/dump_syms.cc $<TARGET_OBJECTS:common>)
target_link_libraries(dump_syms)

install(TARGETS dump_syms DESTINATION bin)
