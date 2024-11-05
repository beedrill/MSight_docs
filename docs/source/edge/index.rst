MSight's Edge Portion
========================

MSight's Edge Device portion is designed to run on an edge device that shares the same network with all roadside sensors and other infrastructure devices. It is responsible for collecting data from these sensors and devices, performing in-time processing, and then sending the processed data to the cloud server.

The system is built on a distributed node architecture where each node can operate independently. This design ensures flexibility and reliability, allowing the edge portion of MSight to scale efficiently and handle various processing tasks in parallel. Nodes communicate with each other using a robust pub/sub system implemented with Redis Pub/Sub, ensuring minimal latency and efficient data transfer between them.

One of the significant advantages of MSight's edge architecture is its ease of use for field deployment. End users do not need to understand the underlying implementation of the pub/sub communication system. The MSight edge system is designed to package atomic functionalities into self-contained nodes, enabling field personnel to deploy and manage different functionalities without writing a single line of Python code. This user-centric design allows users to focus on deploying and utilizing the system without needing deep technical expertise.

For developers and engineers integrating new sensors or implementing custom algorithms, MSight provides the flexibility to focus solely on their specific component, seamlessly plugging it into the existing node-based structure. This approach reduces complexity, promotes modular development, and enhances the adaptability of the system.

It is recommended to use a machine with ample CPU, GPU, and memory resources to ensure optimal performance. The is usually the case of a a roadside edge device.

.. toctree::
   :glob:
   :maxdepth: 2

   install
   main_concepts
   usage
   example
   development_guide/index
