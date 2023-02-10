Examples for MSight
===================

Hello world example
-------------------

The hello world example is typically used to test the installation of MSight edge package. To run the example after installation, do:

.. code-block:: bash

    ros2 run msight_hello_world msight_hello_world


RTSP sensor example
-------------------

We offer two example for RTSP sensors:

Run an RTSP sensor handler and an example subscriber
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To run RTSP sensor, at least setup the two parameters, the **topic_name** and the **url_address** parameter:

.. code-block:: bash

    ros2 run msight_sensor_handlers rtsp_sensor_handler --ros-args -p topic_name:=/rtsp_sensor -p url_address:=<your address>

Then if everything works, you should be able to verify and see the IP camera by:

.. code-block:: bash

    ros2 run msight_sensor_handlers example_image_subscriber --ros-args -p show:=true -p topic_name:=/rtsp_sensor


Or run an example launch script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A simple RTSP example comes with the MSight package, run the following to start the example

.. code-block:: bash

    ros2 launch msight_rtsp_example rtsp_example_launch.py url_address:=<your RTSP URL>

Then you can use RVIZ and load RVIZ configuration located in ``src/msight_rtsp_example/msight_rtsp_example/msight_rtsp_example.rviz`` to visualize the image. This example is useful to test RTSP sensor, as well as testing if MSight installed is working normally.


Run bluecity example
--------------------

.. code-block:: bash

    ros2 run msight_sensor_handlers bluecity_sensor_handler --ros-args -p udid:=<your udid> -p username:=<your username> -p password:=<your password> -p topic_name:=/bc_results -p sensor_position_lat:=<your latitude> -p sensor_position_lon:=<your longitude> -p sensor_rotation:=<your rotation>



You can check the results with the example subscriber:

.. code-block:: bash
    
    ros2 run msight_sensor_handlers example_vehicle_list_subscriber --ros-args -p topic_name:=bc_results

Visualization
-------------

The following script contain an topic translator that translate MSight image topic to a standard image topic so that RViz can visualize:

.. code-block:: bash

    ros2 launch msight_example_visualization visualization_launch.py sensor_topic:=rtsp_sensor unwrapped_sensor_topic:=rtsp_sensor_unwrapped

Now you can visualize the RTSP camera in RVIZ with topic **rtsp_sensor_unwrapped**