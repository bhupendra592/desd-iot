# MQTT Pub/Sub Assignment with Mosquitto

This README demonstrates how to use the `mosquitto_pub` and `mosquitto_sub` commands to publish and subscribe to MQTT topics, including examples with single-level wildcards, interactive inputs, and file-based inputs.

---

## Prerequisites

1. **Install Mosquitto Clients**:
   ```bash
   sudo apt update
   sudo apt install mosquitto-clients
   ```

2. **Start an MQTT Broker**:
   Ensure you have a running MQTT broker on `localhost` (or use an external broker).
   - Default port: `1883`.

3. **Operating System**:
   Ensure you are using Ubuntu 20.04 or above for the steps in this guide.

---

## Basic Publish and Subscribe

### **1. Subscribe to a Topic**

Run the following command to subscribe to the topic `cdac/acts/desd`:

```bash
mosquitto_sub -t cdac/acts/desd -h localhost -p 1883
```

### **2. Publish a Single Message**

Publish a message to the same topic:

```bash
mosquitto_pub -t cdac/acts/desd -h localhost -p 1883 -m "Hello, DESD!"
```

- **Expected Output**: The subscriber terminal displays:
  ```plaintext
  Hello, DESD!
  ```

---

## Single-Level Wildcard Example

### **1. Subscribe with Single-Level Wildcard**

Subscribe to all subtopics under `cdac/acts/` using the `+` wildcard:

```bash
mosquitto_sub -t cdac/acts/+ -h localhost -p 1883
```

### **2. Publish Messages to Different Subtopics**

#### Publish to `cdac/acts/desd`:
```bash
mosquitto_pub -t cdac/acts/desd -h localhost -p 1883 -m "Message to DESD"
```

#### Publish to `cdac/acts/iot`:
```bash
mosquitto_pub -t cdac/acts/iot -h localhost -p 1883 -m "Message to IoT"
```

#### Publish to `cdac/acts/embedded`:
```bash
mosquitto_pub -t cdac/acts/embedded -h localhost -p 1883 -m "Message to Embedded"
```

- **Expected Output in Subscriber**:
  ```plaintext
  cdac/acts/desd Message to DESD
  cdac/acts/iot Message to IoT
  cdac/acts/embedded Message to Embedded
  ```

### **Use Cases of Single-Level Wildcard (`+`)**

- **Device Type Differentiation**:
  Monitor all devices in a category without subscribing to each one individually, e.g., `devices/sensors/+` to subscribe to `devices/sensors/temperature`, `devices/sensors/humidity`, etc.

- **Dynamic Topic Structure**:
  Use for applications where part of the topic is variable, such as subscribing to `users/+/messages` to receive messages for all users.

- **Simplified Subscriptions**:
  Reduce the complexity of managing multiple specific subscriptions by using a single wildcard subscription.

---

## Publish Multiple Messages

### **1. Using Interactive Input**

Run the following command to publish messages interactively:

```bash
mosquitto_pub -t cdac/acts/desd -h localhost -p 1883 -l
```

Type each message and press Enter. Example:

```plaintext
Temperature: 25°C
Humidity: 60%
Pressure: 1013 hPa
```

- **Expected Output in Subscriber**:
  ```plaintext
  Temperature: 25°C
  Humidity: 60%
  Pressure: 1013 hPa
  ```

### **2. Publish Messages from a File**

Create a file `sensor_data.txt` with the following content:

```plaintext
Light: ON
Fan: OFF
Voltage: 230V
```

Publish the file’s content line by line:

```bash
mosquitto_pub -t cdac/acts/desd -h localhost -p 1883 -l < sensor_data.txt
```

- **Expected Output in Subscriber**:
  ```plaintext
  Light: ON
  Fan: OFF
  Voltage: 230V
  ```

---

## Explanation of `<` in Linux

The `<` symbol in the command line is used for **input redirection**. It redirects the content of a file as input to a command.

- Example:
  ```bash
  mosquitto_pub -t cdac/acts/desd -h localhost -p 1883 -l < sensor_data.txt
  ```
  This redirects the content of `sensor_data.txt` to `mosquitto_pub` for publishing.

---

## Assignment: Practical Questions

Attempt the following questions on an Ubuntu 20.04 or later machine. Use the Mosquitto tools as explained above. Provide screenshots or logs of your outputs.

### **1. Basic Publish and Subscribe**
- **Task**: Subscribe to the topic `pgdesd/class/assignments` and publish the message "Hello, PGDIoT!".
- **Commands**:
  ```bash
  mosquitto_sub -t pgdesd/class/assignments -h localhost -p 1883
  ```
  ```bash
  mosquitto_pub -t pgdesd/class/assignments -h localhost -p 1883 -m "Hello, PGDIoT!"
  ```
- **Expected Output**:
  ```plaintext
  Hello, PGDIoT!
  ```

### **2. Using Single-Level Wildcard**
- **Task**: Subscribe to `pgdesd/class/+` and publish messages to the following topics:
  - `pgdesd/class/sensors`
  - `pgdesd/class/actuators`
- **Commands**:
  ```bash
  mosquitto_sub -t pgdesd/class/+ -h localhost -p 1883
  ```
  ```bash
  mosquitto_pub -t pgdesd/class/sensors -h localhost -p 1883 -m "Sensor Data: 25°C"
  ```
  ```bash
  mosquitto_pub -t pgdesd/class/actuators -h localhost -p 1883 -m "Actuator Status: ON"
  ```
- **Expected Output**:
  ```plaintext
  pgdesd/class/sensors Sensor Data: 25°C
  pgdesd/class/actuators Actuator Status: ON
  ```

### **3. Publish Multiple Messages Using `-l`**
- **Task**: Publish three lines of messages interactively to `pgdesd/class/sensors`.
- **Commands**:
  ```bash
  mosquitto_pub -t pgdesd/class/sensors -h localhost -p 1883 -l
  ```
  - Input:
    ```plaintext
    Temperature: 30°C
    Humidity: 70%
    Pressure: 1000 hPa
    ```
- **Expected Output**:
  ```plaintext
  Temperature: 30°C
  Humidity: 70%
  Pressure: 1000 hPa
  ```

### **4. Publish Messages from a File**
- **Task**: Create a file `device_status.txt` with the following content and publish it to `pgdesd/class/devices`:
  ```plaintext
  Device1: Active
  Device2: Inactive
  Device3: Error
  ```
- **Commands**:
  ```bash
  mosquitto_pub -t pgdesd/class/devices -h localhost -p 1883 -l < device_status.txt
  ```
- **Expected Output**:
  ```plaintext
  Device1: Active
  Device2: Inactive
  Device3: Error
  ```

### **5. Multi-Client Interaction**
- **Task**: Open two subscribers, one subscribing to `pgdesd/class/+` and the other to `pgdesd/class/devices`. Publish a message to `pgdesd/class/devices` and observe outputs on both subscribers.
- **Commands**:
  - Subscriber 1:
    ```bash
    mosquitto_sub -t pgdesd/class/+ -h localhost -p 1883
    ```
  - Subscriber 2:
    ```bash
    mosquitto_sub -t pgdesd/class/devices -h localhost -p 1883
    ```
  - Publisher:
    ```bash
    mosquitto_pub -t pgdesd/class/devices -h localhost -p 1883 -m "Testing Multi-Client"
    ```
- **Expected Output**:
  - Subscriber 1:
    ```plaintext
    pgdesd/class/devices Testing Multi-Client
    ```
  - Subscriber 2:
    ```plaintext
    Testing Multi-Client
    ```

---

## Summary
- **`-t`**: Specifies the MQTT topic.
- **`-h`**: Specifies the MQTT broker hostname or IP.
- **`-p`**: Specifies the port (default is `1883`).
- **`-m`**: Sends a single message.
- **`-l`**: Reads messages line by line from standard input or a file.
- **`+`**: A single-level wildcard to match subtopics.

---

Happy messaging with MQTT!
