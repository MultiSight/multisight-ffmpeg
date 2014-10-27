cmake_minimum_required(VERSION 2.8)
project(ffmpeg)

include(common.cmake NO_POLICY_SCOPE)

include(ExternalProject)

  # --prefix below is used to tell the project where to install to. We install to a directory under build for 2 reasons:
  # 1) by having the external project install to a directory in build, we can support rebuilds that just install
  # the existing libraries. In effect, we have a 2 step install: the projects internal build system installs to --prefix
  # and our build installs FROM --prefix to ${devel_artifacts_path}
  # 2) the other reason to have the external project install to a directory under prefix is that a clean can be
  # achieved by simply deleting build directory.

IF(BUILD_WINDOWS)

  ExternalProject_Add(
    ffmpeg_linux
    URL ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-1.2.0.tar.bz2
    SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources
    CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources/configure --extra-cflags=-I${devel_artifacts_path}/include --extra-ldflags=-L${devel_artifacts_path}/lib --prefix=${CMAKE_CURRENT_BINARY_DIR}/artifacts --enable-memalign-hack --arch=x86 --target-os=mingw32 --cross-prefix=i686-w64-mingw32- --enable-gpl --enable-libx264 --disable-static --enable-shared --enable-pthreads --enable-runtime-cpudetect
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/artifacts
  )

ELSE()

  IF(CMAKE_SYSTEM MATCHES "Linux-")

    ExternalProject_Add(
      ffmpeg_linux
      URL ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-1.2.0.tar.bz2
      SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources
      CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources/configure --extra-cflags=-I${devel_artifacts_path}/include/x264 --extra-ldflags=-L${devel_artifacts_path}/lib --prefix=${CMAKE_CURRENT_BINARY_DIR}/artifacts --enable-gpl --enable-libx264 --disable-static --enable-shared --enable-pthreads --enable-runtime-cpudetect
      PREFIX ${CMAKE_CURRENT_BINARY_DIR}/artifacts
    )

  ENDIF()

ENDIF()

SET(CMAKE_INSTALL_PREFIX ${devel_artifacts_path})

IF(CMAKE_SYSTEM MATCHES "Linux-")

  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavcodec DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavdevice DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavfilter DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavformat DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavutil DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libpostproc DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libswresample DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libswscale DESTINATION include)

  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/lib/ DESTINATION lib FILES_MATCHING PATTERN "*")

ELSE()

  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavcodec DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavdevice DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavfilter DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavformat DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libavutil DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libpostproc DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libswresample DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/include/libswscale DESTINATION include)

  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-Win64-artifacts/lib/ DESTINATION lib FILES_MATCHING PATTERN "*")

ENDIF()