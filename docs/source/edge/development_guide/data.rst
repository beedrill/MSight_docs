
Development Guide on Data in MSight Edge
========================================

This guide provides an overview of how data handling works within the MSight Edge system and explains how to create custom data types to extend its functionality. The MSight Edge system uses structured data types that facilitate communication between different components and the cloud.

Understanding Data in MSight Edge
----------------------------------------

MSight Edge defines data types using the `Data` and `SensorData` classes. These classes form the backbone for data serialization, deserialization, and communication within the MSight system. The system primarily uses `msgpack` for efficient serialization and deserialization of data.

Data Class
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The `Data` class is the base class for all data types in MSight. It provides methods for:
- **Serialization**: Converting data into a byte format using `msgpack` to send between nodes.
- **Deserialization**: Converting byte data back into structured data.
- **JSON Conversion**: Converting data to a JSON format for output purposes (note: not reversible).

**Key Methods in the `Data` Class**:

- `serialize()`: Serializes the data using `msgpack`.
- `deserialize(data: bytes)`: Deserializes `msgpack`-formatted byte data into an instance of the data class.
- `to_dict()`: Converts the data to a dictionary (to be implemented by subclasses).
- `to_json()`: Converts the data to a JSON string.

SensorData Class
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`SensorData` extends the `Data` class and includes additional attributes for sensor-related information:

- `sensor_name`: The name of the sensor that produced the data.
- `event_timestamp`: The timestamp when the event occurred.
- `device_name`: Automatically includes the device name from the environment variable `MSIGHT_EDGE_DEVICE_NAME`.

This class acts as a base for all data types representing sensor information in the MSight system.

**Key Methods in the `SensorData` Class**:

- `_to_dict()`: Must be implemented by subclasses to include additional payload information.
- `to_dict()`: Combines the sensor attributes with the payload.
- `from_dict(payload: dict)`: Reconstructs an instance from a dictionary representation.

How Data Works
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Data in MSight flows between nodes through topics. Each node serializes data using `msgpack` before publishing it to Redis, ensuring fast and compact communication. When a node receives data, it deserializes it to obtain an instance of the corresponding MSight standard data type.

Creating Custom Data Types
----------------------------------------

To create a custom data type, extend the `SensorData` class and implement the required methods. Below is an example of how to create a custom data type for representing temperature sensor readings.

Example: Custom Temperature Data Type
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code-block:: python

    from msight_edge.data import SensorData

    class TemperatureData(SensorData):
        def __init__(self, sensor_name: str, temperature: float, event_timestamp: float = None):
            super().__init__(sensor_name, event_timestamp)
            self.temperature = temperature

        def _to_dict(self):
            # Add the temperature value to the payload
            return {"temperature": self.temperature}

        @staticmethod
        def _from_dict(payload: dict):
            # Create a TemperatureData instance from the payload
            return TemperatureData(
                sensor_name=payload["sensor_name"],
                temperature=payload["temperature"],
                event_timestamp=payload["event_timestamp"]
            )

    # Usage example
    if __name__ == "__main__":
        # Create an instance of the custom data type
        temp_data = TemperatureData("sensor_1", 22.5)
        
        # Serialize the data
        serialized_data = temp_data.serialize()

        # Deserialize the data back to an object
        deserialized_data = TemperatureData.deserialize(serialized_data)

        # Print the deserialized object details
        print(f"Sensor: {deserialized_data.sensor_name}, Temperature: {deserialized_data.temperature}, Timestamp: {deserialized_data.event_timestamp}")


Explanation:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **Initialization**: The `TemperatureData` class inherits from `SensorData` and adds an additional attribute `temperature`.
- **Serialization and Deserialization**: The `serialize()` and `deserialize()` methods handle the conversion of the data to and from bytes using `msgpack`.
- **Custom Logic**: The `_to_dict()` and `_from_dict()` methods are overridden to include and extract the `temperature` field.

Summary
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- The `Data` and `SensorData` classes provide a structured way to handle data serialization and communication in the MSight system.
- Creating custom data types involves extending `SensorData` and implementing the necessary methods to add custom payloads.
- Efficient data serialization using `msgpack` ensures minimal latency and overhead when transmitting data between nodes.

This guide helps developers understand and implement custom data types to extend the functionality of MSight Edge while maintaining compatibility with the system's infrastructure.
