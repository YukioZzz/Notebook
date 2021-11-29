### debug with gdb in roslaunch

    <node pkg="$(arg project)" type="$(arg project)_mapOptmization"      name="$(arg project)_mapOptmization"       output="screen"     respawn="true"    launch-prefix="xterm -e gdb --args"/>



