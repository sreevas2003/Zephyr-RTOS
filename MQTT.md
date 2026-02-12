# MQTT (Message Queuing Telemetry Transport)  

## 1. Introduction

MQTT (Message Queuing Telemetry Transport) is a lightweight messaging protocol designed for communication in low-bandwidth, high-latency, and unreliable networks.

It is widely used in:

- Internet of Things (IoT)
- Machine-to-Machine (M2M) communication
- Embedded systems
- Remote monitoring systems

MQTT operates on top of TCP/IP and follows a Publish–Subscribe communication model.

---

## 2. Why MQTT?

Traditional protocols like HTTP follow a request-response model, which may not be efficient for resource-constrained devices.

MQTT is designed to:

- Minimize network bandwidth usage
- Reduce device power consumption
- Provide reliable message delivery
- Support asynchronous communication

---

## 3. MQTT Architecture

MQTT uses a Publish–Subscribe model with three main components:

### 3.1 Broker
- Central server
- Receives all messages from publishers
- Filters messages by topic
- Distributes messages to subscribers
- Manages client connections

### 3.2 Publisher
- Sends messages to a topic
- Does not know who receives the message

### 3.3 Subscriber
- Subscribes to topics
- Receives messages from broker

### 3.4 Topic
- Logical channel used to organize messages
- Example: home/temperature (or) factory/machine1/status

Clients do not communicate directly with each other. All communication passes through the broker.

---

## 4. Working of MQTT

1. Client connects to the broker.
2. Publisher sends message to a topic.
3. Subscriber subscribes to a topic.
4. Broker forwards message to all subscribers of that topic.

This allows:

- One-to-many communication
- Many-to-one communication
- Many-to-many communication

---

## 5. Key Features of MQTT

### 5.1 Lightweight Protocol
- Small header size
- Low network overhead
- Suitable for embedded devices

### 5.2 Publish–Subscribe Model
- Decouples sender and receiver
- Improves scalability
- Enables asynchronous communication

### 5.3 Quality of Service (QoS)

MQTT provides three levels of message delivery reliability:

#### QoS 0 – At Most Once
- Message sent once
- No acknowledgment
- Possible message loss

#### QoS 1 – At Least Once
- Message acknowledged
- May receive duplicates

#### QoS 2 – Exactly Once
- Guaranteed single delivery
- Highest reliability
- More overhead

```
mosquitto_pub -t test/topic -m "QoS1 message" -q 1
```
---

### 5.4 Retained Messages
- Broker stores last retained message for a topic
- New subscribers immediately receive that message
```
mosquitto_pub -t test/topic -m "Stored value" -r
```
---

### 5.5 Last Will and Testament (LWT)
- Client sets a will message during connection
- If client disconnects unexpectedly, broker publishes the will message
- Used to detect device failures

---

### 5.6 Security
MQTT supports:

- Username and password authentication
- TLS/SSL encryption

Default Ports:
- 1883 → Non-secure
- 8883 → Secure (TLS)

---

## 6. Advantages of MQTT

- Low bandwidth usage
- Low power consumption
- Small code footprint
- Scalable architecture
- Bi-directional communication
- Reliable message delivery using QoS

---

## 7. Limitations of MQTT

- No built-in encryption (requires TLS configuration)
- Not ideal for extremely high-speed data transfer
- Topic-based filtering may lack strict structure

---

## 8. Applications of MQTT

- Smart home automation
- Industrial IoT
- Remote monitoring systems
- Telemetry data collection
- Cloud-connected embedded devices
- Sensor networks

---

```
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
sudo systemctl status mosquitto
```
terminal 1
```
mosquitto_sub -h localhost -t topic/test
```
terminal 2
```
mosquitto_pub -h localhost -t topic/test -m "Hello MQTT"
```

| Flag       | Purpose        |
| ---------- | -------------- |
| `-h`       | Broker address |
| `-p`       | Port           |
| `-t`       | Topic          |
| `-m`       | Message        |
| `-q`       | QoS            |
| `-r`       | Retain         |
| `-u`, `-P` | Authentication |
| `-d`       | Debug          |

