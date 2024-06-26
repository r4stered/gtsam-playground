# Setup 

These instructions are intended for use on Linux (including WSL) only.

You'll probably need to install these packages:

```
libboost-dev-all protobuf-compiler ninja-build
```

Use this one-line cmake incantation:

```
cmake -B build -GNinja -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_BUILD_TYPE=RelWithDebInfo
```

 Depending on your RAM, you may need to use `-j <number>` to control the number of jobs when building.

# Running a photon sim example

[This sim example](https://github.com/PhotonVision/champs_2024/tree/gtsam-testing/sim_projects/apriltag_yaw_only) in the gtsam-testing branch is set up to provide simulated data to the current build of gtsam-playground. Just run it as a robot simulation project! Until I get CLI working, the gtsam node NT server IP address will need to be manually changed per-machine this is tested on.

I visualize output data usually using advantagescope. The 3d visualizer is great.

In order for the data transfer to work properly, start advantagescope, then sim, then run gtsam-node. The /SmartDashboard/VisionSystemSim-main/Sim Field/Gtsam Robot topic will show the fused pose of the robot.

# NT API

As of right now, these are our publishers/subscribers. The set timestamp is used for latency compensation.

Subscribers
- /cam/tags: TagDetection struct
- /robot/odom: Robot twist from previous odom message

Publishers
- /cam/gtsam_seen_corners: Measured tag corners from cameras for valid tags on our map
- /cam/gtsam_predicted_corners: Expected tag corner locations. Not latency compensated, only valid when not moving.
- /cam/update_dt_ms: How long the update loop took to add all new factors and re-optimize.
- /cam/std_dev: Standard deviations for pose estimate, order is [rx ry rz tx ty tz]

# Notes

WPILib uses a version of Eigen from https://github.com/wpilibsuite/allwpilib/blob/main/upstream_utils/update_eigen.py#L100 SHA is 96880810295b65d77057f4a7fb83a99a590122ad
