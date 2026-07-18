cmake_minimum_required(VERSION 3.8)
project(vulcan_agv_description)

find_package(ament_cmake REQUIRED)

# Install every directory a robot_description package typically needs.
# ament indexes this share directory so `package://vulcan_agv_description/...`
# and get_package_share_directory('vulcan_agv_description') both resolve correctly.
install(
  DIRECTORY urdf meshes launch rviz config
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
