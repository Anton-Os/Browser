cmake_minimum_required(VERSION 2.8)
project(Ao-Browser)

include(ExternalProject)

set(CURL_FLAGS -DBUILD_CURL_EXE=OFF -DBUILD_CURL_TESTS=OFF -DCURL_STATICLIB=ON -DCMAKE_USE_OPENSSL=OFF -DCURL_ZLIB=OFF -DHTTP_ONLY=ON)

ExternalProject_Add(Curl
    GIT_REPOSITORY "https://github.com/curl/curl.git"
    GIT_TAG 0fd6221f8c457e7bf5e5e7bb142676927a6a6709

    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_INSTALL_PREFIX}

    PREFIX "${CMAKE_BINARY_DIR}/Extlib/Curl"
)

find_package(Curl REQUIRED CONFIG)
message(STATUS "Curl headers: ${CURL_INCLUDE_DIRS}")
message(STATUS "Curl libs: ${CURL_LIBRARIES}")

add_executable(Browser_Demo
    example.cpp
)

target_include_directories(Browser_Demo
    PUBLIC
    Browser
    ${CURL_INCLUDE_DIRS}
)

target_link_libraries(Browser_Demo
    ${CURL_LIBRARIES}
)