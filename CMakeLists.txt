cmake_minimum_required(VERSION 3.0)
project(cvfd LANGUAGES CXX)

add_executable(${PROJECT_NAME} src/main.cpp src/FaceDetector.cpp)
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_14)

find_package(PkgConfig REQUIRED)

# GStreamer
pkg_check_modules(GST REQUIRED gstreamer-1.0 gstreamer-audio-1.0 gstreamer-video-1.0  gstreamer-app-1.0)
message("GST_INCLUDE_DIRS = ${GST_INCLUDE_DIRS}")
message("GST_LIBRARY_DIRS = ${GST_LIBRARY_DIRS}")
message("GST_LIBRARIES = ${GST_LIBRARIES}")

link_directories( ${GST_LIBRARY_DIRS}  )
include_directories( ${GST_INCLUDE_DIRS}  )

# OpenCV setup
find_package(OpenCV REQUIRED)
target_link_libraries(${PROJECT_NAME} ${GST_LIBRARIES} ${OpenCV_LIBS})
target_include_directories(${PROJECT_NAME} PRIVATE include)


# Introduce preprocessor variables to keep paths of asset files
set(FACE_DETECTION_CONFIGURATION "${PROJECT_SOURCE_DIR}/assets/deploy.prototxt")
set(FACE_DETECTION_WEIGHTS "${PROJECT_SOURCE_DIR}/assets/res10_300x300_ssd_iter_140000_fp16.caffemodel")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

target_compile_definitions(${PROJECT_NAME} PRIVATE FACE_DETECTION_CONFIGURATION="${FACE_DETECTION_CONFIGURATION}")
target_compile_definitions(${PROJECT_NAME} PRIVATE FACE_DETECTION_WEIGHTS="${FACE_DETECTION_WEIGHTS}")


add_executable(video3 src/video3.cpp src/FaceDetector.cpp)
target_compile_features(video3 PUBLIC cxx_std_14)
