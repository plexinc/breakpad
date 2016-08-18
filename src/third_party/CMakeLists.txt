if(INCLUDE_LIB)
  file(GLOB H lss/*h)
  install(FILES ${H} DESTINATION include/breakpad/third_party/lss)
endif()