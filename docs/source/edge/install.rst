Installation
==============

It is encouraged to use Docker to install the MSight-edge system. The MSight-edge package is tightly coupled with Docker, and a single Docker-compose file can be used to deploy the entire MSight-edge system effortlessly. However, for those who prefer more customization or specific needs, MSight-edge can also be installed locally from source.

Use Docker Image
----------------
The local installation is highly dependent on the operating system and environment, which can introduce compatibility issues. For streamlined and consistent deployment, it is recommended to use the Docker installation method.

MSight-edge offers Docker images with different tags for flexibility:
- **`latest`**: The standard image with full functionality, including perception algorithms and GPU support.
- **`latest-compact`**: A minimal version of the image, which excludes perception algorithms and GPU support. This version is designed for sensor data streaming and cloud interaction only. The `compact` tag is ideal for scenarios where the edge system is primarily used for data collection and does not require intensive computation, thus saving disk space.

To pull the Docker image:

.. code-block:: bash
    
    docker pull michigantrafficlab/msight-edge:latest

For the compact version:

.. code-block:: bash
    
    docker pull michigantrafficlab/msight-edge:latest-compact

Build Docker Image from Source
------------------------------
In some use cases, you might need to build the Docker image directly from the source repository. To do this, navigate to the root directory of the MSight repository and use the following command:

.. code-block:: bash
    
    docker build -f docker/DOCKERFILE -t msight-img --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) .

Once built, you can run your MSight code within the Docker image:

.. code-block:: bash
    
    docker run msight-img <your MSight code>

Local Installation from Source
------------------------------
To install MSight-edge from source, use the following commands:

.. code-block:: bash

    pip install . --user

Installing from source provides flexibility and control for development purposes but may require additional setup to match your environment's specific configurations.
