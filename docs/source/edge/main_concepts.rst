Main Concepts
================

The core idea behind the MSight edge library is to provide a suite of executable **nodes** that can be utilized to construct comprehensive applications. These nodes are designed to operate independently, enabling flexible use across various contexts and applications. This modular design allows for scalable and adaptable system development. The following figure illustrates the main concepts of the library:

.. image:: figs/main_concepts.png

### Node Types
MSight offers three main types of nodes, each serving a distinct purpose in the data pipeline:

1. **Source Nodes**: Responsible for acquiring data from sources such as sensors, files, or web services, and making it available to other nodes. These nodes ensure that the data is formatted in a way that is compatible with subsequent processing steps.
2. **Data Processing Nodes**: Handle the processing of data received from source nodes, performing tasks such as filtering, segmentation, and tracking. These nodes also format the processed data so it can be utilized by subsequent nodes.
3. **Sink Nodes**: Receive data from processing nodes and perform actions such as displaying, saving to a file, or transmitting to a web service.

Data Types
------------

The nodes in MSight exchange data using specific data types that ensure consistency and interoperability. These data types are represented as classes designed for serializability, facilitating communication between nodes through the pub/sub system. Each data type includes methods for serialization and deserialization to convert data to and from byte arrays. Below are some of the key data types provided by MSight:

- **MSight SensorImage**: Represents an image captured by a sensor, stored as a NumPy array.
- **MSight Road Object List**: Represents a list of detected road objects, typically used for perception and tracking tasks.
- **MSight Bytes**: A flexible data type for storing byte arrays, allowing for any type of data.
- **MSight PointCloud** (under development): Represents a point cloud captured by a sensor, stored as a NumPy array.

Supported Nodes
----------------

The following table lists the supported nodes in MSight, their descriptions, types, input/output data types, launch commands, and status. For more details about specific command-line arguments, use `--help`.

.. list-table:: Supported Nodes
   :widths: 10 20 10 10 10 10 10
   :header-rows: 2

   * - Name
     - Description
     - Node Type
     - Input Data Type
     - Output Data Type
     - Launch Command
     - Status
   * - RTSP
     - Reads data from an RTSP stream and provides it to other nodes.
     - Source
     - RTSP stream
     - MSight SensorImage
     - `msight_launch_rtsp`
     - Stable
   * - Local Image
     - Reads data from files and publishes it, simulating a deployment sensor. Useful for debugging and testing.
     - Source
     - Local
     - MSight SensorImage
     - `msight_launch_local_image`
     - Stable
   * - Ouster
     - Reads data from a Bluecity edge device and provides it to other nodes.
     - Source
     - Frame from Bluecity API
     - MSight Road Object List
     - `msight_launch_ouster`
     - Stable 
   * - Derq Sensor Handler
     - Reads data from a Derq edge device and publishes it.
     - Source
     - Derq detection result (JSON) from UDP
     - MSight Road Object List
     - N/A
     - Under development
   * - SDSM Receiver
     - Receives encoded SDSM messages from UDP and decodes them to MSight Road Object Lists.
     - Source
     - SDSM message from UDP
     - MSight Road Object List
     - `msight_launch_sdsm_receiver`
     - Under development
   * - MSight Detection Node
     - Performs object detection and tracking on images.
     - Data Processing
     - MSight SensorImage
     - MSight Road Object List
     - `msight_launch_yolov8_detection`
     - Stable
   * - MSight Geo Fusion Node
     - Fuses outputs from different data processors.
     - Data Processing
     - MSight Road Object List
     - MSight Road Object List
     - `msight_launch_geo_fusion`
     - Stable
   * - MSight Tracking Node
     - Tracks detected objects over time.
     - Data Processing
     - MSight Road Object List
     - MSight Road Object List
     - `msight_launch_tracking`
     - Stable
   * - SDSM Message Encoder
     - Encodes results into SDSM message format.
     - Data Processing
     - MSight Road Object List
     - MSight Bytes
     - `msight_launch_sdsm_encoder`
     - Stable
   * - BSM Message Encoder
     - Encodes results into BSM message format.
     - Data Processing
     - MSight Road Object List
     - N/A
     - N/A
     - Under development
   * - IFM
     - Sends IFM messages to RSU using the UDP protocol.
     - Sink
     - MSight Bytes
     - N/A
     - `msight_launch_ifm`
     - Stable
   * - AWS Kinesis Pusher
     - Uploads results to AWS Kinesis.
     - Sink
     - MSight Road Object List
     - N/A
     - `msight_launch_kinesis_pusher`
     - Stable
   * - CSV File Writer
     - Writes results to a CSV file.
     - Sink
     - MSight Road Object List
     - N/A
     - N/A
     - Under development
   * - Image Viewer
     - Displays images from sensors.
     - Sink
     - MSight SensorImage
     - N/A
     - `msight_launch_image_viewer`
     - Stable
