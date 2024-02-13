Get Start with MSight Edge System
========================================
MSight essentially offers a number of utility nodes for the user to organize by themselves.
There are different ways to organize and control these nodes, we conclude *best practice* here for production deployment:

(**recomended for deployment**) Run each node in one docker container.

  The nodes will be organized with a docker-compose.yml file. There are several benefits:
    * Fully containerized and handled by docker compose script, making this option extremely easy to install, deploy and manage.
    * The robustness such as node restart and other features are handled by Docker daemon.
    * Logging system can directly stream logs to the cloud

  However, since each node is running in a separate containers, this method has a large storage overhead and require a large disk space 
  (which is normally not a big deal for modern edge device as harddrive is cheap nowadays).



