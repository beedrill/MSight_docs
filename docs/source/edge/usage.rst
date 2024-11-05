Deployment Strategies
========================================

MSight offers utility nodes that users can organize and manage independently. There are different strategies for organizing and controlling these nodes. Below, we outline the best practices for production deployment.

Containerized Deployment (Recommended)
----------------------------------------

**Run each node in a separate Docker container**. This strategy involves organizing the nodes using a `docker-compose.yml` file. Here are the key benefits of this approach:

- **Ease of Installation and Management**: The entire system is containerized and handled by a Docker Compose script, making deployment, installation, and management straightforward.
- **Robustness**: Docker's daemon manages node restarts and other failover features, adding resilience to the deployment.
- **Logging System**: Logs can be streamed directly to the cloud, facilitating remote monitoring and troubleshooting.

**Considerations**:
While this method is robust and easy to manage, running each node in a separate container increases storage overhead and requires significant disk space. This is generally not an issue for modern edge devices, as storage is relatively inexpensive.

Local Deployment
----------------------------------------

**Deploy nodes locally using custom scripts or process managers like `supervisor`**. MSight does not provide built-in scripting for deployment but includes various launch scripts for starting individual nodes. For local deployments, users are encouraged to use existing tools to script, launch, and manage these nodes.

Benefits of Local Deployment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **Lower Disk Space Requirement**: Running nodes locally avoids the overhead of separate Docker containers, resulting in reduced disk space usage.
- **Potential Performance Gains**: Running directly on the local system can provide slightly better performance compared to containerized deployment.

Drawbacks
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **Configuration Complexity**: Setting up and managing nodes locally requires more manual configuration and scripting, which can be more challenging and error-prone compared to using Docker.

Comparison of Deployment Strategies
----------------------------------------

The following table compares the two deployment strategies based on key aspects:

.. list-table:: Comparison of Deployment Strategies
   :widths: 20 30 30
   :header-rows: 1

   * - Aspect
     - Containerized Deployment (Docker)
     - Local Deployment (Supervisor)
   * - **Ease of Deployment**
     - High (managed by `docker-compose.yml`, minimal manual setup)
     - Moderate (requires custom scripts and setup)
   * - **Resource Overhead**
     - Higher (due to containerization)
     - Lower (direct local execution)
   * - **Resilience and Robustness**
     - High (handled by Docker daemon with restart capabilities)
     - Moderate (depends on supervisor scripts and manual configurations)
   * - **Disk Space Requirement**
     - High (larger storage footprint due to container images)
     - Lower (less disk space needed)
   * - **Performance**
     - Slightly reduced (due to container overhead)
     - Slightly better (runs directly on the host system)
   * - **Flexibility**
     - High (isolated environments for each node, easier conflict management)
     - Moderate (potential conflicts in the local environment)

Recommendations for Deployment Management
----------------------------------------

MSight provides launch scripts for starting individual nodes but does not include built-in scripting for full deployment. Users are advised to use external tools and strategies for managing deployments:

- **For Containerized Deployments**: Create and manage a `docker-compose.yml` file to streamline deployment with `docker compose up`. This strategy is robust, easy to manage, and suitable for environments where disk space is not a major constraint.
- **For Local Deployments**: Use a process manager like `supervisor` to script and manage node launches. This approach is beneficial when disk space is limited or when maximum performance is needed.

By following these strategies, users can choose the deployment method that best suits their needs, balancing ease of use, resource management, and performance.
