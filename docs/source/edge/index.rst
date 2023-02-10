MSight's Edge Portion
========================

MSight's Edge Device portion is designed to be running on a edge device, which is in the same network of all the roadside sensors and other infrastructure device. It is responsible for collecting data from the roadside sensors and other infrastructure devices, and then sending the data to the cloud server. 

Msight Edge Device is designed to be running on a Ubuntu 22.04 LTS system. It is recommended to use a machine with excess CPU, GPU and memory resources.

MSight is based on `Robot Operation System 2 (ROS2) <https://docs.ros.org/en/humble/index.html>`_. It is recommended to use ROS2 Humble to run MSight, which is ideal to run on Ubuntu 22.04 LTS.


.. toctree::
   :glob:
   :maxdepth: 2

   install
   main_concepts
   usage
