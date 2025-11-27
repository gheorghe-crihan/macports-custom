cmake_minimum_required(VERSION 3.0...3.10)

project(zip-fix-filename-encoding VERSION 1.0 LANGUAGES C)

include(CheckSymbolExists)
include(CheckIncludeFile)

check_include_file(unistd.h HAVE_UNISTD_H)

set(CMAKE_REQUIRED_LIBRARIES "c") 
check_symbol_exists(mkstemp "unistd.h" HAVE_MKSTEMP)
check_symbol_exists(fseeko "stdio.h" HAVE_FSEEKO)
unset(CMAKE_REQUIRED_LIBRARIES)

configure_file(config.h.cm.in config.h)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

#noinst_LTLIBRARIES = libzip.la
#noinst_HEADERS = zipint.h zip.h

#libzip_la_LDFLAGS=-version-info 1:0
#libzip_la_LIBADD=@LIBOBJS@

add_custom_command(
        OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/libzip-0.7.1/lib/zip_err_str.c
        COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/libzip-0.7.1/lib/make_zip_err_str.sh ${CMAKE_CURRENT_SOURCE_DIR}/libzip-0.7.1/lib/zip.h ${CMAKE_CURRENT_SOURCE_DIR}/libzip-0.7.1/lib/zip_err_str.c
        DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/libzip-0.7.1/lib/zip.h
        COMMENT "Generating zip_err_str.c..."
    )

add_library(libzip STATIC
	libzip-0.7.1/lib/zip_add.c
	libzip-0.7.1/lib/zip_close.c
	libzip-0.7.1/lib/zip_delete.c
	libzip-0.7.1/lib/zip_dirent.c
	libzip-0.7.1/lib/zip_entry_free.c
	libzip-0.7.1/lib/zip_entry_new.c
	libzip-0.7.1/lib/zip_err_str.c
	libzip-0.7.1/lib/zip_error.c
	libzip-0.7.1/lib/zip_error_get.c
	libzip-0.7.1/lib/zip_error_get_sys_type.c
	libzip-0.7.1/lib/zip_error_strerror.c
	libzip-0.7.1/lib/zip_error_to_str.c
	libzip-0.7.1/lib/zip_fclose.c
	libzip-0.7.1/lib/zip_file_error_get.c
	libzip-0.7.1/lib/zip_file_get_offset.c
	libzip-0.7.1/lib/zip_file_strerror.c
	libzip-0.7.1/lib/zip_fopen.c
	libzip-0.7.1/lib/zip_fopen_index.c
	libzip-0.7.1/lib/zip_fread.c
	libzip-0.7.1/lib/zip_free.c
	libzip-0.7.1/lib/zip_get_archive_comment.c
	libzip-0.7.1/lib/zip_get_file_comment.c
	libzip-0.7.1/lib/zip_get_num_files.c
	libzip-0.7.1/lib/zip_get_name.c
	libzip-0.7.1/lib/zip_memdup.c
	libzip-0.7.1/lib/zip_name_locate.c
	libzip-0.7.1/lib/zip_new.c
	libzip-0.7.1/lib/zip_open.c
	libzip-0.7.1/lib/zip_rename.c
	libzip-0.7.1/lib/zip_replace.c
	libzip-0.7.1/lib/zip_set_archive_comment.c
	libzip-0.7.1/lib/zip_set_file_comment.c
	libzip-0.7.1/lib/zip_source_buffer.c
	libzip-0.7.1/lib/zip_source_file.c
	libzip-0.7.1/lib/zip_source_filep.c
	libzip-0.7.1/lib/zip_source_free.c
	libzip-0.7.1/lib/zip_source_function.c
	libzip-0.7.1/lib/zip_source_zip.c
	libzip-0.7.1/lib/zip_set_name.c
	libzip-0.7.1/lib/zip_stat.c
	libzip-0.7.1/lib/zip_stat_index.c
	libzip-0.7.1/lib/zip_strerror.c
	libzip-0.7.1/lib/zip_unchange.c
	libzip-0.7.1/lib/zip_unchange_all.c
	libzip-0.7.1/lib/zip_unchange_archive.c
	libzip-0.7.1/lib/zip_unchange_data.c
)

if (NOT HAVE_MKSTEMP)
        target_sources(libzip PRIVATE libzip-0.7.1/lib/mkstemp.c)
endif()

target_include_directories(libzip PUBLIC 
    "${CMAKE_CURRENT_SOURCE_DIR}/libzip-0.7.1/lib")

add_executable(runzip src/runzip.c)

target_link_libraries(runzip PRIVATE libzip iconv z)

enable_testing()

add_subdirectory(tests)
