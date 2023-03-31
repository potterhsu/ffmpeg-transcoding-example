cmake_minimum_required(VERSION 3.22)
project(ffmpeg-transcoding-example)

set(CMAKE_C_STANDARD 11)
set(ENV{PKG_CONFIG_PATH} ${CMAKE_SOURCE_DIR}/3rdparty/ffmpeg/lib/pkgconfig:${CMAKE_SOURCE_DIR}/3rdparty/opus/lib/pkgconfig)

find_package(PkgConfig REQUIRED)
pkg_search_module(PKG_AVCODEC REQUIRED libavcodec)
pkg_search_module(PKG_AVDEVICE REQUIRED libavdevice)
pkg_search_module(PKG_AVFILTER REQUIRED libavfilter)
pkg_search_module(PKG_AVFORMAT REQUIRED libavformat)
pkg_search_module(PKG_AVUTIL REQUIRED libavutil)
pkg_search_module(PKG_SWRESAMPLE REQUIRED libswresample)
pkg_search_module(PKG_SWSCALE REQUIRED libswscale)
pkg_search_module(PKG_OPUS REQUIRED opus)

include_directories(
        ${PKG_AVCODEC_INCLUDE_DIRS}
        ${PKG_AVDEVICE_INCLUDE_DIRS}
        ${PKG_AVFILTER_INCLUDE_DIRS}
        ${PKG_AVFORMAT_INCLUDE_DIRS}
        ${PKG_AVUTIL_INCLUDE_DIRS}
        ${PKG_SWRESAMPLE_INCLUDE_DIRS}
        ${PKG_SWSCALE_INCLUDE_DIRS}
        ${PKG_OPUS_INCLUDE_DIRS}
)

link_directories(
        ${PKG_AVCODEC_LIBDIR}
        ${PKG_AVDEVICE_LIBDIR}
        ${PKG_AVFILTER_LIBDIR}
        ${PKG_AVFORMAT_LIBDIR}
        ${PKG_AVUTIL_LIBDIR}
        ${PKG_SWRESAMPLE_LIBDIR}
        ${PKG_SWSCALE_LIBDIR}
        ${PKG_OPUS_LIBDIR}
)

add_executable(ffmpeg-transcoding-example main.c)

target_link_libraries(
        ffmpeg-transcoding-example
        PRIVATE
        ${PKG_AVCODEC_LIBRARIES}
        ${PKG_AVDEVICE_LIBRARIES}
        ${PKG_AVFILTER_LIBRARIES}
        ${PKG_AVFORMAT_LIBRARIES}
        ${PKG_AVUTIL_LIBRARIES}
        ${PKG_SWRESAMPLE_LIBRARIES}
        ${PKG_SWSCALE_LIBRARIES}
        ${PKG_OPUS_LIBRARIES}
)