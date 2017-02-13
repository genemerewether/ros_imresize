cmake_minimum_required(VERSION 2.8.3)
project(ros_imresize)

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  roscpp
  std_msgs
)

find_package( OpenCV REQUIRED )

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS cv_bridge image_transport roscpp std_msgs
#  DEPENDS system_lib
)

include_directories(
  ${catkin_INCLUDE_DIRS} include
)

add_library(resizeimage src/image_handler.cpp
)
target_link_libraries(resizeimage ${catkin_LIBRARIES})

add_executable(ros_imresize src/resize_image.cpp)
target_link_libraries(ros_imresize
   ${catkin_LIBRARIES} resizeimage
)

install(TARGETS resizeimage ros_imresize
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  PATTERN ".svn" EXCLUDE
)
