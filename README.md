<<<<<<< HEAD
# vulcan_agv_description (ROS2)

Converted and corrected from a SolidWorks-to-URDF (SW2URDF) export originally
named `Assem2`, targeting ROS1/catkin. This package is now ROS2/ament.

## What was fixed vs. the original export
1. **Package renamed** `Assem2` -> `vulcan_agv_description` (all 11
   `package://` mesh URIs updated to match).
2. **`camera` joint changed from `continuous` to `fixed`** — it does not
   rotate relative to `base_link`; leaving it `continuous` would have made
   `joint_state_publisher` treat it as an unactuated moving joint and turned
   a should-be-static TF transform into a dynamic, unstable one.
3. **Wheel spin axes cleaned to exact unit vectors** (`leftwheel`,
   `rightwheel`, `flwheel`, `frwheel`, `rlwheel`, `rrwheel`) — the raw export
   had small (~0.5-3.6 degree) tilts from SolidWorks mate tolerance.
   **`rrwheel`'s original tilt was ~8.4 degrees**, meaningfully larger than
   the rest — verify this corner's castor/wheel mate in your SolidWorks
   assembly; it may be a real misalignment, not just export noise.
4. Converted the whole package from **catkin (ROS1) to ament_cmake (ROS2)**:
   `package.xml` is now format 3, `CMakeLists.txt` uses `ament_cmake`.
5. Rewrote `display.launch` (ROS1 XML) as **`launch/display.launch.py`**
   (ROS2 Python launch), reading the URDF file contents directly instead of
   using the ROS1 `textfile` param trick.
6. Added a minimal working `rviz/vulcan_agv.rviz` with RobotModel + TF
   displays and `Fixed Frame: base_link`.
7. Kept your `joint_names_Assem2.yaml` as `config/joint_names_vulcan_agv.yaml`
   for future `ros2_control` use (Milestone 9).

## What is still missing (you need to supply these)
Only `base_link.STL` was provided, so **10 of 11 meshes are missing**:
```
left_wheel.STL      right_wheel.STL
fl_castor.STL       fl_wheel.STL
fr_castor.STL       fr_wheel.STL
rl_castor.STL       rl_wheel.STL
rr_castor.STL       rr_wheel.STL
camera_link.STL
```
Drop these into `meshes/` (same filenames, case-sensitive) once you export
them from SolidWorks — the URDF already references them at the correct path.
Until then, `colcon build` will succeed but RViz will only render the chassis
and show broken/missing-mesh warnings for every other link.

## Build & Test
```bash
# from your ROS2 workspace root, e.g. ~/agv_ws
cp -r vulcan_agv_description src/
colcon build --packages-select vulcan_agv_description
source install/setup.bash

ros2 launch vulcan_agv_description display.launch.py
```
This brings up `robot_state_publisher`, `joint_state_publisher_gui`
(sliders), and RViz2 together — the ROS2 equivalent of your old
`display.launch`. Drag the sliders and confirm:
- Both drive wheels spin about the lateral (Y) axis only
- All four casters pivot about the vertical (Z) axis only
- The camera_link frame does NOT show a slider (it's fixed now — correct)

## Not yet converted
`gazebo.launch` (spawning into Gazebo) was **not** converted yet — ROS2
Gazebo spawning uses a different mechanism (`ros_gz_sim` or
`gazebo_ros`'s `spawn_entity.py` depending on which Gazebo version we
target) and belongs to Milestone 8, not this URDF-cleanup step. We'll build
that when we get there.

## Next step in the roadmap
Milestone 7 — convert this static URDF into parametric Xacro macros
(`agv_base.xacro`, `agv_wheels.xacro`, `agv_sensors.xacro`).
=======
# AGV
>>>>>>> 4d8995c9b2082b7c0577bf429fb930c40ec7dcfb
