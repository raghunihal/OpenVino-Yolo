cmake_minimum_required(VERSION 3.12)
set(PROJECT_NAME Yolo9-OpenVino)
set(SRC main.cpp)

project(${PROJECT_NAME})
find_package(OpenCV REQUIRED)

include_directories(
    ${OpenCV_INCLUDE_DIRS}
    /opt/intel/openvino_2024/runtime/include
    /opt/intel/openvino_2024/runtime/include/ie
    /opt/intel/openvino_2024/runtime/include/ngraph
)

message(STATUS "OpenCV library status:")
message(STATUS "version:${OpenCV_VERSION}")
message(STATUS "libraries:${OpenCV_LIBS}")
message(STATUS "include path:${OpenCV_INCLUDE_DIRS}")
### generate detector.so
add_library (detector SHARED ${CMAKE_CURRENT_SOURCE_DIR}/yolov9_openvino.cpp)
target_include_directories(detector
      PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
      PUBLIC  ${OpenCV_INCLUDE_DIRS}
      /opt/intel/openvino_2024/runtime/include
      /opt/intel/openvino_2024/runtime/include/ie
      /opt/intel/openvino_2024/runtime/include/ngraph
)
target_link_libraries(detector
      ${OpenCV_LIBS}
      /opt/intel/openvino_2024/runtime/lib/intel64/libopenvino.so
    )

add_executable(${PROJECT_NAME} ${SRC})
target_link_libraries(${PROJECT_NAME} 
            ${OpenCV_LIBS}
            detector
    )