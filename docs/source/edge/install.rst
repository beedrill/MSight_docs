Installation
==============

It is encouraged to use Docker to install MSight-edge system. The MSight-edge package is tightly coupled with Docker and one can use a single Docker-compose file
to deploy the entire MSight-edge system. Of course, one can also install MSight-edge locally from source.


Use Docker image
----------------
The local installation is bound tightly with the operation system and environment. For deployment, it is recommended to use docker installation method. 

.. code-block:: bash
    
    docker pull michigantrafficlab/msight-edge


Build docker image from source
------------------------------
In some usecases, you can also build the docker image in the root of the repository:

.. code-block:: bash
    
    docker build -f docker/DOCKERFILE -t msight-img --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) .

Then you can use MSight within the Docker image. Or you can run your MSight code with docker run:

.. code-block:: bash
    
    docker run msight-img <your MSight code>


Local Installation from source
------------------------------

To install MSight-edge from source, you can use the following commands:

.. code-block:: bash

    pip install . --user

