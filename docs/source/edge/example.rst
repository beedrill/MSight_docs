Example
========================================

This example demonstrates a basic but representative deployment scenario in roadside infrastructure monitoring. The setup involves a single RTSP camera streaming video data, which is processed by a series of MSight nodes to detect objects and forward relevant encoded messages to an RSU (Roadside Unit). This example provides a comprehensive view of how to structure nodes and topics to create a functional pipeline for traffic monitoring and communication.

Scenario Overview
----------------------------------------

In this scenario, the workflow involves:

1. **RTSP Camera Streaming**: One RTSP camera streams video data.
2. **RTSP Node**: Captures the stream and publishes the raw image data to a topic (`rtsp_topic`).
3. **Detection Node**: Subscribes to `rtsp_topic`, processes the image data for object detection, and publishes the results to another topic (`detection_topic`).
4. **SDSM Encoder Node**: Subscribes to `detection_topic`, encodes the detection results as SDSM (Standard Data Sharing Message) format, and publishes them to `sdsm_topic`.
5. **IFM Node**: Subscribes to `sdsm_topic` and forwards the encoded data to an RSU over UDP using the correct format.

This pipeline completes a loop that showcases data flow from image capture to actionable message transmission on roadside infrastructure.

Translating the Scenario to Pub/Sub Language
----------------------------------------

- **Nodes**: Independent executable units that publish or subscribe to topics to communicate data.
- **Topics**: Named channels used for communication between nodes, where each topic can hold specific types of data.

In this example:
- **`rtsp_topic`**: Used by the RTSP node to publish image data.
- **`detection_topic`**: Used by the detection node to publish detection results.
- **`sdsm_topic`**: Used by the SDSM encoder node to publish encoded SDSM data for the IFM node to read and forward.

This setup ensures a seamless flow of data through a pub/sub system, allowing each node to focus on its specific task while communicating effectively with other nodes.

Docker Deployment
----------------------------------------

For Docker deployment, we utilize a `docker-compose.yml` file that defines and runs all the services in isolated containers. This approach simplifies deployment and management, ensuring consistent environments across deployments.

**docker-compose.yml Example**:

.. code-block:: yaml

    version: "3.9"
    services:
    redis-server:
        image: redis
        network_mode: "host"
        restart: ${restart_strategy}
        env_file:
        - .env

    rtsp-node:
        image: michigantrafficlab/msight-edge:v0.0.9_0.3
        network_mode: "host"
        entrypoint: ["msight_launch_rtsp"]
        command: ["-n", "rtsp_node", "-t", "rtsp_topic", "--sensor-name", "camera1", "-u", "rtsp://dummy_ip:8616/stream"]
        restart: ${restart_strategy}
        volumes:
        - /etc/localtime:/etc/localtime
        - /etc/timezone:/etc/timezone:ro
        env_file:
        - .env

    detection-node:
        image: michigantrafficlab/msight-edge:v0.0.9_0.3
        network_mode: "host"
        entrypoint: ["msight_launch_yolov8_detection"]
        command: ["-n", "detection_node", "--subscribe-topic", "rtsp_topic", "--publish-topic", "detection_topic", "-c", "/det_config/config.yaml", "-m", "results_and_image"]
        restart: ${restart_strategy}
        volumes:
        - /etc/localtime:/etc/localtime
        - /etc/timezone:/etc/timezone:ro
        - ./det_config:/det_config
        env_file:
        - .env
        environment:
        - CUDA_VISIBLE_DEVICES=0

    sdsm-encoder-node:
        image: michigantrafficlab/msight-edge:v0.0.9_0.3
        network_mode: "host"
        entrypoint: ["msight_launch_sdsm_encoder"]
        command: ["-n", "sdsm_encoder_node", "--subscribe-topic", "detection_topic", "--publish-topic", "sdsm_topic"]
        restart: ${restart_strategy}
        volumes:
        - /etc/localtime:/etc/localtime
        - /etc/timezone:/etc/timezone:ro
        env_file:
        - .env

    ifm-node:
        image: michigantrafficlab/msight-edge:v0.0.9_0.3
        network_mode: "host"
        entrypoint: ["msight_launch_ifm"]
        command: ["-n", "ifm_node", "--subscribe-topic", "sdsm_topic", "--header-file", "/config/ifm_header.txt", "--rsu-addr", "127.0.0.1", "--rsu-port", "12345"]
        restart: ${restart_strategy}
        volumes:
        - /etc/localtime:/etc/localtime
        - /etc/timezone:/etc/timezone:ro
        - ./config:/config
        env_file:
        - .env


**Instructions**:

1. Place the `docker-compose.yml` and `.env` file in the same directory.
2. Run the deployment using:
   .. code-block:: bash

      docker-compose up -d

**Monitoring and Health Checks**:

- **Check Status**:

  .. code-block:: bash

      docker ps -a

  This command lists running containers to verify that all nodes are operational.

- **View Logs**:

  .. code-block:: bash

      docker logs <container_name>

  Replace `<container_name>` with the name of the container (e.g., `rtsp-node`) to view logs and check for issues.

Supervisor Deployment
----------------------------------------

For local deployments, `Supervisor <http://supervisord.org/>`_ is used to manage and monitor the nodes. Below is an example configuration:

**Supervisor Configuration (`msight_supervisor.conf`)**:

.. code-block:: ini

    [program:redis-server]
    command=redis-server
    autostart=true
    autorestart=true
    environment=MSIGHT_EDGE_DEVICE_NAME="mcity_edge"

    [program:rtsp-node]
    command=msight_launch_rtsp -n rtsp_node -t rtsp_topic --sensor-name camera1 -u rtsp://dummy_ip:8616/stream
    autostart=true
    autorestart=true
    environment=MSIGHT_EDGE_DEVICE_NAME="mcity_edge"

    [program:detection-node]
    command=msight_launch_yolov8_detection -n detection_node --subscribe-topic rtsp_topic --publish-topic detection_topic -c /det_config/config.yaml -m results_and_image
    autostart=true
    autorestart=true
    environment=MSIGHT_EDGE_DEVICE_NAME="mcity_edge",CUDA_VISIBLE_DEVICES="0"
    directory=/path/to/det_config

    [program:sdsm-encoder-node]
    command=msight_launch_sdsm_encoder -n sdsm_encoder_node --subscribe-topic detection_topic --publish-topic sdsm_topic
    autostart=true
    autorestart=true
    environment=MSIGHT_EDGE_DEVICE_NAME="mcity_edge"

    [program:ifm-node]
    command=msight_launch_ifm -n ifm_node --subscribe-topic sdsm_topic --header-file /path/to/config/ifm_header.txt --rsu-addr 127.0.0.1 --rsu-port 12345
    autostart=true
    autorestart=true
    environment=MSIGHT_EDGE_DEVICE_NAME="mcity_edge"
    directory=/path/to/config

**Instructions**:

1. Copy `msight_supervisor.conf` to `/etc/supervisor/conf.d/`.
2. Reload Supervisor configuration and start the services:

   .. code-block:: bash

      supervisorctl reread
      supervisorctl update
      supervisorctl start all

**Monitoring and Health Checks**:

- **Check Status**:

  .. code-block:: bash

      supervisorctl status

  This command lists all configured programs and their current status.

- **View Logs**:

  Log files are typically stored in `/var/log/supervisor/`. To view logs:

  .. code-block:: bash

      tail -f /var/log/supervisor/<program_name>.log

Useful Commands for Field Deployment
----------------------------------------

- **Restart a Program**:

  .. code-block:: bash

      supervisorctl restart <program_name>

- **Stop a Program**:

  .. code-block:: bash

      supervisorctl stop <program_name>

This comprehensive guide helps users understand and deploy MSight nodes using both Docker and Supervisor, ensuring a smooth and reliable deployment process for real-world roadside infrastructure.

Periodic Reboot
-------------------------------------
For both Docker and Supervisor deployments, it is recommended to schedule periodic reboots to maintain system health and performance. This practice helps clear memory, reset connections, and ensure that the system runs optimally over extended periods.

**Instructions**:

1. Create a cron job to reboot the system at a specific time. For example, to reboot at 3 AM daily:

   .. code-block:: bash

      0 3 * * * /sbin/shutdown -r now

2.  Running:

    .. code-block:: bash

       crontab -e
and adding the above line to the crontab file schedules the reboot.

