### 建模

1. `SDF` 与 `URDF` 格式 区别：见[博客](https://www.guyuehome.com/7052)

SDF（Simulation Description Format）是一种XML格式，能够描述机器人、静态和动态物体、照明、地形甚至物理学的各方面的信息。SDF可以精确描述机器人的各类性质，除了传统的运动学特性之外，还可以为机器人定义传感器、表面属性、纹理、关节摩擦等；SDF还提供了定义各种环境的方法。包括环境光照、地形等。

URDF（Unified Robot Description Format）在ROS中是一种功能强大且标准化的机器人描述格式，但依然缺少许多功能。e.g. 不能定义非机器人物体，例如灯光，高度图等。

2. URDF and XACRO in `roslaunch`, see [gazebo tutorial](http://gazebosim.org/tutorials?tut=ros_roslaunch)


2. `SDF`的使用

见[链接](https://www.guyuehome.com/7113)

3. 简化mesh网格模型

例如[1](https://poppy.discourse.group/t/cfc-reduce-3d-meshes-complexity-of-the-urdf-model/1031/2),和[2](https://myshumi.net/2014/03/02/mesh-simplification-and-collada-exporter/)

4. `dae` 和 `stl` 的区别
参考[链接](https://answers.ros.org/question/39959/stl-vs-dae/)

stl is the simpler variant of representing a mesh which comes from the CAM and rapid prototyping world, and only allows for surface geometry information. In contrast, dae files use XML and allow to embed much richer information, such as color, texture and other data.

The Collada format, on the other hand was intended for exchanging digital assets, and can encode not only 3D geometry, but also physics, kinematics, animations/morphing, shaders, manages level-of-detail, and more.

obj files:
参考[1](https://matterandform.net/blog/long-live-the-obj)


### Docker Enable GPU
参考[docker资源限制文档](https://docs.docker.com/config/containers/resource_constraints/#gpu)

对于已经编译好的image,可直接使用`docker run`命令，只需要`--gpus all`参数即可暴露gpu环境并运行`nvidia-smi`命令，即

    docker run -it --rm --gpus all ubuntu nvidia-smi

对于`docker-compose`的设置，则可参考[文档](https://docs.docker.com/compose/gpu-support/), 使用`deploy:resources:reservations:devices`设置，即可运行`nvidia-smi`命令

对于`rviz`, 则需要在Dockerfile中同时声明以下环境变量

    ENV NVIDIA_VISIBLE_DEVICES \
        ${NVIDIA_VISIBLE_DEVICES:-all}
    ENV NVIDIA_DRIVER_CAPABILITIES \
        ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics


### About /clock
见[官方文档](http://wiki.ros.org/Clock)


### Robot Description
1. About setting up robot using tf, 见[官方文档](http://wiki.ros.org/navigation/Tutorials/RobotSetup/TF)
2. Using urdf or xacro instead, see [page](https://github.com/ros/robot_state_publisher/issues/119)
```xml
<launch>
  <param name="robot_description" command="$(find xacro)/xacro '$(find foo_description)/urdf/foo.urdf.xacro'" />
  <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" />
</launch>
```

### About simulation of the laser

1. gazebo configuration files of a sensor [velodyne](http://gazebosim.org/tutorials?tut=guided_i1)
