Installation
==============

There are two ways of installing MSight, the local installation and the Docker installation.

Local Installation from source
------------------------------

MSight is based on `ROS2 Humble <https://docs.ros.org/en/humble/index.html>`_, therefore, for local installation, it is recommended to use `Ubuntu Jammy <https://releases.ubuntu.com/22.04/>`_ as the base system.

To install ROS2, follow the `official documentation <https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html>`_

After installation of ROS2,  you can compile and setup MSight edge with the following commands:

.. code-block:: bash

    colcon build
    source install/local_setup.sh

Use Docker image
----------------
The local installation is bound tightly with the operation system and environment. For deployment, it is recommended to use docker installation method. 

.. code-block:: bash
    
    docker pull msight/msight-edge


Build docker image from source
------------------------------
In some usecases, you can also build the docker image in the root of the repository:

.. code-block:: bash
    
    docker build -f docker/DOCKERFILE -t msight-img --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) .

Then you can use MSight within the Docker image. Or you can run your MSight code with docker run:

.. code-block:: bash
    
    docker run msight-img <your MSight code>

