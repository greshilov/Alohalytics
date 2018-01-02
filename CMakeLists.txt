cmake_minimum_required(VERSION 3.2)

project(alohalytics C CXX)

set(CMAKE_CXX_STANDART 11)
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

# Usage:
# make test ARGS="-V"
enable_testing()

get_filename_component(ALOHA_ROOT . ABSOLUTE)

find_package(Threads REQUIRED)
find_package(ZLIB REQUIRED)

include_directories(${ALOHA_ROOT})

if(NOT ANDROID)
  add_subdirectory(examples/cpp)
  add_subdirectory(examples/server)
  add_subdirectory(server)
  add_subdirectory(tests)
endif()

set(
  SRC
  src/alohalytics.h
  src/event_base.h
  src/file_manager.h
  src/gzip_wrapper.h
  src/http_client.h
  src/location.h
  src/logger.h
  src/messages_queue.h
  src/cpp/alohalytics.cc
  examples/cpp/example.cc
)

if (UNIX)
  set(
    PLATFORM_SRC
    src/posix/file_manager_posix_impl.cc
    src/posix/http_client_curl.cc
  )
endif()

if (ANDROID)
  add_subdirectory(src/android)
endif()

if(NOT ANDROID)
  add_executable(${PROJECT_NAME} ${SRC} ${PLATFORM_SRC})
  target_link_libraries(${PROJECT_NAME} ZLIB::ZLIB Threads::Threads)
endif()
