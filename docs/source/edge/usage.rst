Get Start with MSight Edge System
========================================
MSight essentially offers a number of utility ROS2 nodes for the user to organize by themselves.
There are different ways to organize and control these nodes, we conclude *three most popular options* here with pro and cons:

1. (**recomended for deployment**) Run each node in one docker container.

  In this case, the nodes will be organized with a docker-compose.yml file. There are several benefits:
    * Fully containerized and handled by docker compose script, making this option extremely easy to install, deploy and manage, in general, this is simpler than a ROS launch script.
    * People with less ROS knowledge and more Docker container knowledge can handle this with very low learning curve
    * The robustness such as node restart and other features are handled by Docker daemon.
    * Logging system can directly stream logs to the cloud

  However, since each node is running in a separate containers, this method has a large storage overhead and require a large disk space 
  (which is normally not a big deal for modern edge device as harddrive is cheap nowadays).
  The following two options can give more compact deal.

2. Oganize the nodes with a ROS2 launch script, and run all nodes in one container.

    In this case, all nodes will be running in a single docker container. The nodes will be handled by a ROS launch script. This is a typical way to run a ROS system, but it requires ROS expertise to organize these nodes using a ROS launch script.
    Since all nodes are running in one Docker container, this method has less storage and, possibly, communication overhead.

3. Oganize the nodes with a ROS2 launch script, and run directly on host machine

    In this way, all the nodes will run natively on the host machine. This is a good option for developers who need to update and try the code very often. Since ROS system is sensitive with Linux version, this is **not recommended** for deployment.


