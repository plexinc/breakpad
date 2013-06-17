find_library(FOUNDATION_LIB Foundation)

add_executable(dump_syms dump_syms/dump_syms_tool.mm $<TARGET_OBJECTS:common>)
target_link_libraries(dump_syms ${FOUNDATION_LIB})

install(TARGETS dump_syms DESTINATION bin)
