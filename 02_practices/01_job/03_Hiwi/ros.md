### debug with gdb

1. in `roslaunch` 

    <node pkg="$(arg project)" type="$(arg project)_mapOptmization"      name="$(arg project)_mapOptmization"       output="screen"     respawn="true"    launch-prefix="xterm -e gdb --args"/>


2. in `rosrun`

    rosrun --prefix 'gdb -ex run --args' ....

### Nvidia support

refering to the ros tutorial [Hardware Acceleration](http://wiki.ros.org/docker/Tutorials/Hardware%20Acceleration)

check integrated gpu:

    lspci | grep -i vga

check nvidia gpu:

    lspci | grep -i nvidia


check nvidia gpu usage:

    nvidia-smi

### rosbag filter

refer to the [link](https://answers.ros.org/question/56935/how-to-remove-a-tf-from-a-ros-bag/)

    rosbag filter input.bag output.bag 'topic == "ALL THE TOPICS YOU WANT TO KEEP" or topic == "/tf" and m.transforms[0].header.frame_id != "/base_link" and m.transforms[0].child_frame_id != "/virtual_cam"'
    

