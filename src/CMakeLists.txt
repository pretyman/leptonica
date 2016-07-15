#
# lept
#

################################################################################

########################################
# SHARED LIBRARY lept
########################################

file(GLOB hdr "*.h")
file(GLOB src "*.c")
if (MSVC)
    set_source_files_properties(${src} PROPERTIES LANGUAGE CXX)
endif()

add_library                     (lept ${LIBRARY_TYPE} ${src} ${hdr})
set_target_properties           (lept PROPERTIES VERSION   ${VERSION_MAJOR}.${VERSION_MINOR})
set_target_properties           (lept PROPERTIES SOVERSION ${VERSION_MAJOR}.${VERSION_MINOR})
if (WIN32)
set_target_properties           (lept PROPERTIES OUTPUT_NAME       lept${VERSION_MAJOR}${VERSION_MINOR})
set_target_properties           (lept PROPERTIES DEBUG_OUTPUT_NAME lept${VERSION_MAJOR}${VERSION_MINOR}d)
endif()

if (NOT STATIC)
    target_compile_definitions  (lept PRIVATE -DLIBLEPT_EXPORTS)
endif()

if (GIF_LIBRARY)
    target_link_libraries       (lept ${GIF_LIBRARY})
endif()
if (JPEG_LIBRARY)
    target_link_libraries       (lept ${JPEG_LIBRARY})
endif()
if (JP2K_FOUND)
    target_link_libraries       (lept ${JP2K_LIBRARIES})
endif()
if (PNG_LIBRARY)
    target_link_libraries       (lept ${PNG_LIBRARY})
endif()
if (TIFF_LIBRARY)
    target_link_libraries       (lept ${TIFF_LIBRARY})
endif()
if (WEBP_FOUND)
    target_link_libraries       (lept ${WEBP_LIBRARIES})
endif()
if (ZLIB_LIBRARY)
    target_link_libraries       (lept ${ZLIB_LIBRARY})
endif()

if (UNIX)
    target_link_libraries       (lept m)
endif()

if (NOT USES_CPPAN)
    export(TARGETS lept FILE ${CMAKE_BINARY_DIR}/LeptonicaTargets.cmake)
else()
    target_link_libraries       (lept cppan)
    file(WRITE ${CMAKE_BINARY_DIR}/LeptonicaTargets.cmake "include(${CMAKE_BINARY_DIR}/cppan.cmake)\n")
    export(TARGETS lept APPEND FILE ${CMAKE_BINARY_DIR}/LeptonicaTargets.cmake)
endif()

################################################################################

