## README: Running the Kafka Demo Project

This project demonstrates simple communication between a Kafka producer (`kafka-demo-driver`) and a Kafka consumer (`kafka-demo-user`). The producer sends simulated location data, and the consumer receives and prints it.

### Prerequisites

1. **Java Development Kit (JDK):** Version 17 or higher is recommended.
2. **Apache Maven:** Used for building the project.
3. **Apache Kafka:**  A running Kafka broker is required (this README assumes it's running on `localhost:9092`).  See [Apache Kafka Quickstart](https://kafka.apache.org/quickstart) for Kafka and ZooKeeper setup instructions.  Note: The quickstart primarily uses Unix-style commands.  For Windows, adapt the commands as follows:

    **Unix/macOS:**
    ```bash
    bin/zookeeper-server-start.sh config/zookeeper.properties
    bin/kafka-server-start.sh config/server.properties
    ```

    **Windows:**
    ```powershell
    .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
    .\bin\windows\kafka-server-start.bat .\config\server.properties
    ```

### Project Structure

The project consists of two modules:

- `kafka-demo-driver`: The producer service, sending location updates.
- `kafka-demo-user`: The consumer service, receiving and printing location updates.

### Steps to Run

1. **Start ZooKeeper and Kafka Broker:** Follow the instructions in the Prerequisites section to start ZooKeeper and Kafka.

2. **Build the Projects:** Navigate to each project's root directory (`kafka-demo-driver` and `kafka-demo-user`) and build them using Maven:

    ```bash
    mvn clean install
    ```

3. **Run the Producer (`kafka-demo-driver`):**  Navigate to the `kafka-demo-driver` directory and run:

    ```bash
    mvn spring-boot:run
    ```

    This starts the producer service on port `1234`. The `KafkaConfig` class will automatically create the `cab-location` Kafka topic if it doesn't exist.  The producer will then send mock location data to this topic.

4. **Run the Consumer (`kafka-demo-user`):** In a separate terminal, navigate to the `kafka-demo-user` directory and run:

    ```bash
    mvn spring-boot:run
    ```

    This starts the consumer service on port `1233`, listening for messages on the `cab-location` topic and printing them to the console.

### Verification

After starting both services, you should see location data printed in the consumer's console. This confirms communication via Kafka is working.

### Troubleshooting

- **Connection Errors:** If there are errors connecting to Kafka, verify the broker is running at `localhost:9092` and check the logs.
- **No Messages Received:** Double-check the configurations in `application.properties` for both projects to ensure they're using the correct topic (`cab-location`), serializers, and deserializers.  Ensure both the producer and consumer have the correct `bootstrap.servers` property for Kafka and `group-id` for the consumer.
