cmake_minimum_required(VERSION 3.11)

project(raqm)

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR};${CMAKE_MODULE_PATH}")

find_package(Freetype REQUIRED)
find_package(Fribidi REQUIRED)
find_package(harfbuzz CONFIG REQUIRED)

find_path(HARFBUZZ_INCLUDE_DIRS
          NAMES hb.h
          PATH_SUFFIXES harfbuzz)
find_path(FREETYPE_ADDITIONAL_INCLUDE_DIRS NAMES ft2build.h PATH_SUFFIXES freetype2)

configure_file(src/raqm-version.h.in src/raqm-version.h)

add_library(raqm src/raqm.c src/raqm.h src/raqm-version.h)

target_include_directories(raqm PUBLIC ${CMAKE_CURRENT_BINARY_DIR}/src)
target_include_directories(raqm SYSTEM PUBLIC ${FREETYPE_ADDITIONAL_INCLUDE_DIRS})
target_include_directories(raqm SYSTEM PUBLIC ${FREETYPE_INCLUDE_DIRS})
target_include_directories(raqm SYSTEM PUBLIC ${HARFBUZZ_INCLUDE_DIRS})
target_include_directories(raqm SYSTEM PUBLIC ${FRIBIDI_INCLUDE_DIR})

target_link_libraries(raqm PRIVATE Freetype::Freetype)
target_link_libraries(raqm PRIVATE harfbuzz::harfbuzz ${FRIBIDI_LIBRARY})

install(TARGETS raqm 
    RUNTIME DESTINATION bin
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

set(RAQM_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(RAQM_LIBRARY raqm)
set(RAQM_LIBRARIES ${HARFBUZZ_LIBRARY} ${FRIBIDI_LIBRARY} ${RAQM_LIBRARY})
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/raqm.h ${CMAKE_CURRENT_BINARY_DIR}/src/raqm-version.h DESTINATION ${CURRENT_PACKAGES_DIR}/include)
