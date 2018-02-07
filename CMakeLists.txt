cmake_minimum_required(VERSION 3.9)
set(CMAKE_BUILD_TYPE RELEASE)
project(exhal VERSION 2.1 DESCRIPTION "HAL Laboratory NES/SNES/GB (de)compression tools")

add_library(hal_compress SHARED compress.c)
add_library(hal_compress_static STATIC compress.c)
add_executable(exhal exhal.c)
target_link_libraries(exhal hal_compress_static)
add_executable(inhal inhal.c)
target_link_libraries(inhal hal_compress_static)
add_executable(hal-sniff sniff.c)
target_link_libraries(hal-sniff hal_compress_static)

target_compile_definitions(hal_compress PUBLIC EXTRA_OUT)
target_compile_definitions(hal_compress_static PUBLIC EXTRA_OUT)

include(GNUInstallDirs)
configure_file(exhal.pc.in exhal.pc @ONLY)
install(TARGETS hal_compress
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(DIRECTORY include DESTINATION ${CMAKE_INSTALL_INCLUDEDIRS}/exhal)

install(FILES ${CMAKE_BINARY_DIR}/exhal.pc DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)
