cmake_minimum_required(VERSION 2.8)
project(ffmpeg)

include(common.cmake NO_POLICY_SCOPE)

include(ExternalProject)

if(CMAKE_SYSTEM MATCHES "Linux-")

  # --prefix below is used to tell the project where to install to. We install to a directory under build for 2 reasons:
  # 1) by having the external project install to a directory in build, we can support rebuilds that just install
  # the existing libraries. In effect, we have a 2 step install: the projects internal build system installs to --prefix
  # and our build installs FROM --prefix to ${devel_artifacts_path}
  # 2) the other reason to have the external project install to a directory under prefix is that a clean can be
  # achieved by simply deleting build directory.

  ExternalProject_Add(
    ffmpeg_linux
    URL ${CMAKE_CURRENT_SOURCE_DIR}/ffmpeg-2014-10-12.tar.bz2
    SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources
    CONFIGURE_COMMAND ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg_sources/configure --extra-cflags=-I${devel_artifacts_path}/include/x264 --extra-ldflags=-L${devel_artifacts_path}/lib --prefix=${CMAKE_CURRENT_BINARY_DIR}/artifacts --enable-gpl --enable-libx264 --disable-static --enable-shared --enable-pthreads --enable-runtime-cpudetect
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/artifacts
    )

  set(CMAKE_INSTALL_PREFIX ${devel_artifacts_path})

  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavcodec DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavdevice DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavfilter DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavformat DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libavutil DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libpostproc DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libswresample DESTINATION include)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/include/libswscale DESTINATION include)

  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/artifacts/lib/ DESTINATION lib FILES_MATCHING PATTERN "*")

elseif(CMAKE_SYSTEM MATCHES "Windows")

endif(CMAKE_SYSTEM MATCHES "Linux-")
