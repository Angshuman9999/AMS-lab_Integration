# Advanced_Mobility_Lab Systems

Drivers onboard AMS Urban cars. This branch is under development for ROS2 humble and jazzy.

## Cloning submodules
If you clone this repository, make sure to clone the submodules as well. You can do this by running:

```bash
git submodule update --init --recursive --remote
```

This will ensure you have all the submodules cloned and updated to the configured branches.

Note: This is not required now.

## Deadman's switch
On Logitech F-710 joysticks, the LB button is the deadman's switch for teleop, and the RB button is the deadman's switch for navigation. You can also remap buttons. See how on the readthedocs documentation.

## Topics

### Topics that the driver stack subscribe to
- `/drive`: Topic for autonomous navigation, uses `AckermannDriveStamped` messages.

### Runtime topics published by the driver stack
- `/odom`: Topic for `Odometry` messages.
- `/sensors/imu/raw`: Topic for `Imu` messages.
- `/sensors/core`: Topic for telemetry data from the VESC

## External Dependencies

1. ackermann_msgs [https://index.ros.org/r/ackermann_msgs/#humble](https://index.ros.org/r/ackermann_msgs/#humble).
2. joy [https://index.ros.org/p/joy/#humble](https://index.ros.org/p/joy/#humble). This is the driver for joysticks in ROS 2.
3. teleop_tools  [https://index.ros.org/p/teleop_tools/#humble](https://index.ros.org/p/teleop_tools/#humble). This is the package for teleop with joysticks in ROS 2.
4. vesc [GitHub - f1tenth/vesc at ros2](https://github.com/f1tenth/vesc/tree/ros2). This is the driver for VESCs in ROS 2.
5. ackermann_mux [GitHub - f1tenth/ackermann_mux: Twist multiplexer](https://github.com/f1tenth/ackermann_mux). This is a package for multiplexing ackermann messages in ROS 2.
<!-- 8. rosbridge_suite [https://index.ros.org/p/rosbridge_suite/#humble-overview](https://index.ros.org/p/rosbridge_suite/#humble-overview) This is a package that allows for websocket connection in ROS 2. -->

## Package in this repo

1. ams_stack: maintains the bringup launch and all parameter files

## Nodes launched in bringup

1. joy
2. joy_teleop
3. ackermann_to_vesc_node
4. vesc_to_odom_node
5. vesc_driver_node
6. ackermann_mux

## Parameters and topics for dependencies

### vesc_driver

1. Parameters:
   - duty_cycle_min, duty_cycle_max
   - current_min, current_max
   - brake_min, brake_max
   - speed_min, speed_max
   - position_min, position_max
   - servo_min, servo_max
2. Publishes to:
   - sensors/core
   - sensors/servo_position_command
   - sensors/imu
   - sensors/imu/raw
3. Subscribes to:
   - commands/motor/duty_cycle
   - commands/motor/current
   - commands/motor/brake
   - commands/motor/speed
   - commands/motor/position
   - commands/servo/position

### ackermann_to_vesc

1. Parameters:
   - speed_to_erpm_gain
   - speed_to_erpm_offset
   - steering_angle_to_servo_gain
   - steering_angle_to_servo_offset
2. Publishes to:
   - ackermann_cmd
3. Subscribes to:
   - commands/motor/speed
   - commands/servo/position

### vesc_to_odom

1. Parameters:
   - odom_frame
   - base_frame
   - use_servo_cmd_to_calc_angular_velocity
   - speed_to_erpm_gain
   - speed_to_erpm_offset
   - steering_angle_to_servo_gain
   - steering_angle_to_servo_offset
   - wheelbase
   - publish_tf
2. Publishes to:
   - odom
3. Subscribes to:
   - sensors/core
   - sensors/servo_position_command

### throttle_interpolator

1. Parameters:
   - rpm_input_topic
   - rpm_output_topic
   - servo_input_topic
   - servo_output_topic
   - max_acceleration
   - speed_max
   - speed_min
   - throttle_smoother_rate
   - speed_to_erpm_gain
   - max_servo_speed
   - steering_angle_to_servo_gain
   - servo_smoother_rate
   - servo_max
   - servo_min
   - steering_angle_to_servo_offset
2. Publishes to:
   - topic described in rpm_output_topic
   - topic described in servo_output_topic
3. Subscribes to:
   - topic described in rpm_input_topic
   - topic described in servo_input_topic
