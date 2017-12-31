cmake_minimum_required(VERSION 3.2)

project(alohalytics C CXX)

set(CMAKE_CXX_STANDART 11)

enable_testing()

get_filename_component(ALOHA_ROOT . ABSOLUTE)

find_package(Threads REQUIRED)
find_package(ZLIB REQUIRED)

add_subdirectory(tests)

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

add_executable(${PROJECT_NAME} ${SRC} ${PLATFORM_SRC})

target_link_libraries(${PROJECT_NAME} ZLIB::ZLIB Threads::Threads)
