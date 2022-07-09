cmake_minimum_required(VERSION 3.0.2)
project(roschat_msgs)

find_package(catkin REQUIRED COMPONENTS
  genmsg
  std_msgs
)

add_message_files(FILES
  RosChat.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
)

catkin_package(CATKIN_DEPENDS std_msgs genmsg)
