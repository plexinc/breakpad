cmake_minimum_required(VERSION 3.16)
project(breakpad C CXX ASM)

if(APPLE)
  enable_language(OBJC OBJCXX)
endif()

find_package(Threads REQUIRED)

add_library(breakpad_interface INTERFACE)
target_include_directories(breakpad_interface INTERFACE src)
if(UNIX AND NOT APPLE)
  target_include_directories(breakpad_interface INTERFACE plex)
endif()

set(CMAKE_CXX_STANDARD 11)

if(CMAKE_CXX_COMPILER_ID STREQUAL "Clang" AND NOT WIN32)
  target_compile_options(breakpad_interface
    INTERFACE
    -ffunction-sections
    -fdata-sections
    -fno-exceptions
    -Wno-unused-parameter
    -Wno-missing-field-initializers
    -fvisibility=hidden
    -fvisibility-inlines-hidden
    -frtti
  )

  if(UNIX AND NOT APPLE)
    target_link_options(breakpad_interface
      INTERFACE
      -Wl,-gc-sections
      -Wl,-z,noexecstack
    )
  elseif(APPLE)
    target_link_options(breakpad_interface
      INTERFACE
      -Wl,-dead_strip
    )
    target_compile_definitions(breakpad_interface
      INTERFACE
      HAVE_MACH_O_NLIST_H
    )
  endif()

  target_compile_definitions(breakpad_interface
    INTERFACE
    _FILE_OFFSET_BITS=64
  )
endif()

if(WIN32)
  target_compile_definitions(breakpad_interface
    INTERFACE

    _WIN32_WINNT=0x0600
    WINVER=0x0600
    WIN32
    _WINDOWS
    _HAS_EXCEPTIONS=0
    NOMINMAX
    _CRT_RAND_S
    CERT_CHAIN_PARA_HAS_EXTRA_FIELDS
    WIN32_LEAN_AND_MEAN
    _SECURE_ATL
    _HAS_TR1=0
    _CRT_SECURE_NO_DEPRECATE
    _CRT_NONSTDC_NO_WARNINGS
    _CRT_NONSTDC_NO_DEPRECATE
    _USING_V110_SDK71_
    UNICODE

  )

  target_include_directories(breakpad_interface
    INTERFACE
    "$ENV{VSINSTALLDIR}/DIA SDK/include"
  )
endif()

add_subdirectory(src)