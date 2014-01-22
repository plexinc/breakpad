set(HEADERS
  breakpad_types.h
  minidump_cpu_ppc64.h
  minidump_cpu_x86.h
  minidump_exception_solaris.h
  minidump_size.h
  minidump_cpu_amd64.h
  minidump_cpu_ppc.h
  minidump_exception_linux.h
  minidump_exception_win32.h
  minidump_exception_ps3.h
  minidump_cpu_arm.h
  minidump_cpu_sparc.h
  minidump_cpu_arm64.h
  minidump_cpu_mips.h
  minidump_exception_mac.h
  minidump_format.h
)

install(FILES ${HEADERS} DESTINATION include/breakpad/google_breakpad/common)
