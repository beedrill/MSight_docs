
Launch Script Tutorial
========================================

This tutorial explains how to create and register a launch script in MSight Edge, enabling seamless node deployment using command-line interfaces or orchestration tools like Docker and Supervisor.

Overview of Launch Scripts in MSight
----------------------------------------

Launch scripts in MSight Edge are Python scripts designed to initialize and run specific nodes. These scripts facilitate the deployment and management of nodes by allowing developers to specify node parameters and easily integrate them into deployment workflows. By registering these scripts as entry points in the `setup.py` file, they can be executed using the MSight command-line interface (CLI).

How Launch Scripts Work
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **Script Structure**: Each script imports the necessary MSight modules, sets up argument parsing for command-line input, initializes a node, and starts it using the `spin()` method.
- **Deployment**: Once a launch script is registered with the MSight CLI, it can be executed directly from the command line or used in Docker/Supervisor setups for automated deployment.

Example Launch Script: RTSP Source Node
----------------------------------------

Below is an example of a launch script for an RTSP source node:

.. code-block:: python

   from msight_edge.nodes.source_rtsp import RTSPSourceNode
   from msight_edge import Topic, ImageData
   import argparse

   def main():
       parser = argparse.ArgumentParser(
           description='Launch a source node to grab RTSP stream.')
       parser.add_argument('-n', '--name', required=True,
                           type=str, help='The name of the source node.')
       parser.add_argument('-t', '--publish-topic', required=True,
                           type=str, help='The topic to publish the image data to.')
       parser.add_argument('--sensor-name', type=str,
                           required=True, help='The name of the sensor.')
       parser.add_argument('-u', '--url', required=True, help="The URL of the RTSP stream")
       parser.add_argument('-g', '--gap', type=int, default=0, help="Gap between the two frames published")
       args = parser.parse_args()

       topic = Topic(args.publish_topic, ImageData)
       source_node = RTSPSourceNode(
           args.name, topic, args.sensor_name, args.url, gap=args.gap)
       print(f"Starting the RTSP Source node {args.name}.")
       source_node.spin()

   if __name__ == "__main__":
       main()

Explanation:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **Argument Parsing**: The script uses `argparse` to allow users to specify parameters such as the node name, publish topic, sensor name, and RTSP URL.
- **Topic Creation**: The `Topic` object defines the channel to which the node will publish data.
- **Node Initialization**: The `RTSPSourceNode` is instantiated with the specified parameters and started using the `spin()` method.

Registering Launch Scripts in MSight
----------------------------------------

To make the launch script executable as a CLI command, it must be registered in `setup.py` using the `entry_points` argument.

Example `setup.py` Configuration:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

   from setuptools import setup, find_packages
   import os

   def read_version():
       with open("msight_edge/VERSION", 'r') as f:
           return f.read().strip()

   def load_requirements(filename='requirements.txt'):
       with open(filename, 'r') as f:
           return f.read().splitlines()

   def find_so_files():
       so_files = {}
       for root, dirs, files in os.walk('msight_edge'):
           for file in files:
               if file.endswith('.so'):
                   package_path = root.replace(os.sep, '.')
                   so_files.setdefault(package_path, [])
                   so_files[package_path].append(os.path.join(root, file)[len(package_path.replace('.', os.sep)) + 1:])
       return {k: v for k, v in so_files.items()}

   def get_pkg_data():
       data = {'msight_edge': ['VERSION']}
       so_data = find_so_files()
       for k, v in so_data.items():
           data[k] = v
       return data

   setup(
       name="msight_edge",
       version=read_version(),
       packages=find_packages(),
       package_data=find_so_files(),
       description="A package for edge computing with msight edge nodes.",
       long_description=open("README.md").read(),
       long_description_content_type="text/markdown",
       author="Rusheng Zhang",
       author_email="rushegz@umich.edu",
       url="https://github.com/michigan-traffic-lab/MSight_Edge2/tree/main",
       install_requires=load_requirements(),
       entry_points={
           "console_scripts": [
               "msight_launch_rtsp=cli.launch_rtsp:main",
               # Additional launch scripts can be registered here
           ]
       },
   )

Explanation:
^^^^^^^^^^^^^^^^^

- **Entry Points**: The `entry_points` section specifies CLI commands that will be created when MSight is installed. For example, `msight_launch_rtsp` will map to the `main` function in the `cli.launch_rtsp` module.
- **Deployment**: Once registered, these commands can be run directly from the command line (e.g., `msight_launch_rtsp`).

Using Launch Scripts with Docker and Supervisor
----------------------------------------

Launch scripts can be integrated into Docker containers or managed by Supervisor for automated deployment and monitoring.

**Docker Integration**:
1. Create a `Dockerfile` or use `docker-compose` to define services that run the launch script.
2. Use `CMD` or `ENTRYPOINT` in the `Dockerfile` to specify the launch script.

**Supervisor Configuration**:

1. Add an entry in the Supervisor configuration file:
   .. code-block:: ini

      [program:rtsp-source-node]
      command=msight_launch_rtsp -n my_rtsp_node -t rtsp_topic --sensor-name camera1 -u rtsp://dummy_ip:8616/stream
      autostart=true
      autorestart=true

This setup ensures that nodes can be launched and managed with ease, facilitating real-time data processing and sensor communication within the MSight Edge system.
