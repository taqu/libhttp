cmake_minimum_required(VERSION 3.9)

set(THIRDPARTY_ROOT "${CMAKE_CURRENT_SOURCE_DIR}")

# OpenSSL
set(OPENSSL_ROOT "${THIRDPARTY_ROOT}/openssl")
set(OPENSSL_LIB_DIR "${OPENSSL_ROOT}/lib")
add_library(OPENSSL MODULE IMPORTED)
set_target_properties(OPENSSL PROPERTIES IMPORTED_LOCATION "${OPENSSL_LIB_DIR}/libssl.lib")

add_library(OPENSSL_CRYPT MODULE IMPORTED)
set_target_properties(OPENSSL_CRYPT PROPERTIES IMPORTED_LOCATION "${OPENSSL_LIB_DIR}/libcrypto.lib")

include_directories(AFTER "${OPENSSL_ROOT}/include")
link_directories(AFTER "${OPENSSL_LIB_DIR}")

# zlib
set(ZLIB_ROOT "${THIRDPARTY_ROOT}/zlib")
set(ZLIB_LIB_DIR "${ZLIB_ROOT}/lib")
add_library(ZLIB MODULE IMPORTED)
set_target_properties(ZLIB PROPERTIES IMPORTED_LOCATION_DEBUG "${ZLIB_LIB_DIR}/zsd.lib" IMPORTED_LOCATION_RELEASE "${ZLIB_LIB_DIR}/zs.lib")

include_directories(AFTER "${ZLIB_ROOT}/include")
link_directories(AFTER "${ZLIB_LIB_DIR}")

# curl
set(CURL_ROOT "${THIRDPARTY_ROOT}/curl")
set(CURL_LIB_DIR "${CURL_ROOT}/lib")
add_library(CURL MODULE IMPORTED)
set_target_properties(CURL PROPERTIES IMPORTED_LOCATION_DEBUG "${CURL_LIB_DIR}/libcurl-d.lib" IMPORTED_LOCATION_RELEASE "${CURL_LIB_DIR}/libcurl.lib")
include_directories(AFTER "${CURL_ROOT}/include")
link_directories(AFTER "${CURL_LIB_DIR}")

