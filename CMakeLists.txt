cmake_minimum_required(VERSION 2.8)
PROJECT(snappy)

include(CheckIncludeFile)

# version 
set(SNAPPY_MAJOR 1)
set(SNAPPY_MINOR 0)
set(SNAPPY_PATCHLEVEL 4)


set(SRCS 
    snappy.cc snappy-sinksource.cc snappy-stubs-internal.cc snappy-c.cc)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

CHECK_INCLUDE_FILE(stdint.h HAVE_STDINT_H)
CHECK_INCLUDE_FILE(stddef.h HAVE_STDDEF_H)
CHECK_INCLUDE_FILE(sys/uio.h HAVE_UIO_H)

if(HAVE_STDINT_H)
    set(ac_cv_have_stdint_h 1)
else()
    set(ac_cv_have_stdint_h 0)
endif()

if(HAVE_STDDEF_H)
    set(ac_cv_have_stddef_h 1)
else()
    set(ac_cv_have_stddef_h 0)
endif()

if(HAVE_UIO_H)
    set(ac_cv_have_sys_uio_h 1)
else()
    set(ac_cv_have_sys_uio_h 0)
endif()

if(WIN32)
    add_definitions(-DHAVE_WINDOWS_H)
endif()

configure_file(snappy-stubs-public.h.in 
    ${CMAKE_CURRENT_BINARY_DIR}/snappy-stubs-public.h
    @ONLY)

add_library(snappy ${SRCS})

if ( MSVC )
    set_target_properties(snappy PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/../../../libs)
    set_target_properties(snappy PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${PROJECT_BINARY_DIR}/../../../libs)
    set_target_properties(snappy PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${PROJECT_BINARY_DIR}/../../../libs)
endif ( MSVC )