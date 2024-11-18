# ardupilot-ur5
## Instructions to launch Ardupilot with UR arm with teleoperation

### Launch the docker container

```bash
docker run --rm -it --name ardupilot_ur --runtime=nvidia -e DISPLAY=$DISPLAY -e QT_X11_NO_MITSHM='1' -e NVIDIA_VISIBLE_DEVICES=all -e NVIDIA_DRIVER_CAPABILITIES=compute,video,utility,graphics -v /tmp/.X11-unix:/tmp/.X11-unix -v /run/user/1000/gdm/Xauthority:/home/ardupilot/.Xauthority -v /dev/dri:/dev/dri -p 5760:5760 -p 9002:9002 -v "$(pwd):/ardupilot" -u "$(id -u):$(id -g)" pranav2481/ardupilot:20.04 bash
```

### Launch the gazebo simulation inside the container

```bash
roslaunch ur_gripper_gazebo ur_gripper_hande_cubes.launch ur_robot:=ur3e grasp_plugin:=1
```

### Launch Moveit for the arm (Separate terminal session)

```bash
docker exec -it ardupilot_ur bash
roslaunch ur_gripper_85_moveit_config start_moveit.launch
```

### Launch the teleop node (Separate terminal session)

```bash
docker exec -it ardupilot_ur bash
rosrun ur_custom_teleop custom_teleop_gripper.py
```

## Instructions for teleop control

#### To control each joint in the arm:

- Use keys `1-6` to select the joint. 1 is the joint at the base of the arm and 6 is the wrist.
- Use the `+/-` keys to increase/decrease the joint values to move the arm.

#### To control the gripper:

- `0` key will close the gripper.
- `8` key will attach the cube to the gripper. (**Use this to avoid the cube from falling off)
- `9` key will open the gripper.
- `7` key will detach the object from the fripper. (**Use this after opening the gripper to drop the cube.)
