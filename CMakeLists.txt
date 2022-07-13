cmake_minimum_required(VERSION 2.8.12)
project(tinyfiledialogs)

add_library(tinyfiledialogs STATIC
            tinyfiledialogs.c
	    tinyfiledialogs.h)
set_target_properties(tinyfiledialogs PROPERTIES PUBLIC_HEADER "tinyfiledialogs.h")
include_directories(AFTER .)

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  install(TARGETS tinyfiledialogs
	  PUBLIC_HEADER DESTINATION $ENV{COMMONFILES}
	  LIBRARY DESTINATION $ENV{COMMONFILES})
	  
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(local_include_dir /usr/local/include)
  set(local_lib_dir /usr/local/lib)

  set_target_properties(tinyfiledialogs PROPERTIES
    FRAMEWORK TRUE
    FRAMEWORK_VERSION C
    MACOSX_FRAMEWORK_IDENTIFIER com.cmake.tinyfiledialogs
    # "current version" in semantic format in Mach-O binary file
    VERSION 16.4.0
    # "compatibility version" in semantic format in Mach-O binary file
    SOVERSION 1.0.0
    PUBLIC_HEADER tinyfiledialogs.h
    XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY "tinyfiledialogs"
  )

else()
  install(TARGETS tinyfiledialogs
	  PUBLIC_HEADER DESTINATION /usr/local/include
	  LIBRARY DESTINATION /usr/local/lib)

endif()

add_executable(hello hello.c)
target_link_libraries(hello tinyfiledialogs)

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  add_executable(hello_wchar_t hello_wchar_t.c)
  target_link_libraries(hello_wchar_t tinyfiledialogs)
endif()
