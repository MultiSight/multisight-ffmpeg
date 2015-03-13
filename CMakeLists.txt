cmake_minimum_required(VERSION 2.8)
project(ffmpeg)

include(common.cmake NO_POLICY_SCOPE)

include(ExternalProject)

IF(CMAKE_SYSTEM MATCHES "Windows")
    # no build on windows...
ELSE()
    IF(CMAKE_SYSTEM MATCHES "Darwin")
        ExternalProject_Add(
          ffmpeg_linux
          URL ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-1.2.0.tar.bz2
          SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources
          CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources/configure --extra-cflags=-I${devel_artifacts_path}/include/x264 --extra-ldflags=-L${devel_artifacts_path}/lib --prefix=${CMAKE_CURRENT_BINARY_DIR}/artifacts --enable-gpl --enable-libx264 --disable-static --enable-shared --enable-pthreads --enable-runtime-cpudetect
          PREFIX ${CMAKE_CURRENT_BINARY_DIR}/artifacts
        )
    ELSE()
        ExternalProject_Add(
          ffmpeg_linux
          URL ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-1.2.0.tar.bz2
          SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources
          CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources/configure --extra-cflags=-I${devel_artifacts_path}/include/x264 --extra-ldflags=-L${devel_artifacts_path}/lib --prefix=${CMAKE_CURRENT_BINARY_DIR}/artifacts --enable-gpl --enable-libx264 --disable-static --enable-shared --enable-pthreads --enable-runtime-cpudetect --enable-vaapi --enable-hwaccel=h264_vaapi
          PREFIX ${CMAKE_CURRENT_BINARY_DIR}/artifacts
        )
    ENDIF()
ENDIF()

SET(CMAKE_INSTALL_PREFIX ${devel_artifacts_path})

IF(CMAKE_SYSTEM MATCHES "Windows")

  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavcodec DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavdevice DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavfilter DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavformat DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavutil DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libpostproc DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libswresample DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libswscale DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/lib/ DESTINATION lib FILES_MATCHING PATTERN "*")

ELSE()

  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavcodec DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavdevice DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavfilter DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavformat DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavutil DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libpostproc DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libswresample DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libswscale DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/lib/ DESTINATION lib FILES_MATCHING PATTERN "*")

ENDIF()
