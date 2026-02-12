## BLE-Based Proximity Monitoring System Using ESP32 and MQTT

## 1. Project Overview

This project implements a BLE-based proximity monitoring system using ESP32 DevKit v1 boards. 

Two ESP32 boards act as BLE beacons (advertisers), while one ESP32 board functions as a central scanner (base station). The base station scans for specific beacons, extracts RSSI values, and publishes the data to an MQTT broker. A Python client subscribes to the MQTT topics and displays real-time RSSI data in a tabular interface.

This system demonstrates embedded firmware development, BLE communication, MQTT networking, and real-time data processing.

---

## 2. Objective

The objective of this project is to:

- Implement BLE advertisement and scanning using ESP32
- Filter specific devices by name or MAC address
- Extract RSSI for proximity estimation
- Publish data using MQTT protocol
- Display and analyze signal strength using a Python application

---

## 3. System Architecture

### 3.1 Hardware Components

- ESP32 DevKit v1 (2 units) – BLE Beacons
- ESP32 DevKit v1 (1 unit) – BLE Scanner / Base Station
- Wi-Fi Network
- PC running Python
- MQTT Broker (e.g., Mosquitto)

---

### 3.2 Logical Architecture

```
BLE Beacon 1  ----\
                   --> ESP32 Base (Scanner) --> MQTT Broker --> Python Client
BLE Beacon 2  ----/
```

---

## 4. Technology Stack

| Layer              | Technology Used |
|--------------------|-----------------|
| Firmware           | ESP-IDF / Arduino Framework |
| Wireless Protocol  | Bluetooth Low Energy (BLE) |
| Network Layer      | Wi-Fi |
| Messaging Protocol | MQTT |
| Application Layer  | Python |
| MQTT Library       | paho-mqtt |

---

## 5. BLE Implementation

### 5.1 BLE Roles

| Device | BLE Role |
|--------|----------|
| Beacon ESP32 | Peripheral (Advertiser) |
| Base ESP32 | Central (Scanner) |

The system uses BLE advertising only. No connection is established between the base and beacons.

---

### 5.2 Beacon Configuration

Each beacon performs the following:

- Initializes BLE stack
- Sets device name (e.g., BEACON_1, BEACON_2)
- Starts non-connectable advertising
- Transmits advertising packets periodically

Advertising packet may include:
- Device Name
- MAC Address
- Service UUID (optional)

Advertising interval is configurable.

---

### 5.3 Base Station Scanner

The base station:

1. Initializes BLE scanning
2. Performs active or passive scan
3. Filters devices by:
   - Device Name OR
   - MAC Address
4. Extracts:
   - RSSI value
   - Beacon ID
   - Timestamp
5. Publishes data to MQTT broker

Scanning parameters:
- Scan interval
- Scan window
- Active/passive mode

---

## 6. RSSI and Distance Estimation

### 6.1 What is RSSI?

RSSI (Received Signal Strength Indicator) is a measure of signal strength in dBm.

Typical values:
- -30 dBm → Very close
- -60 dBm → Medium distance
- -90 dBm → Far

---

### 6.2 Distance Estimation Model

Distance can be approximated using the logarithmic path loss model:

```
Distance = 10 ^ ((TxPower - RSSI) / (10 * n))
```

Where:
- TxPower = RSSI at 1 meter
- n = Environment factor (2 to 4)

Note:
RSSI is unstable due to:
- Multipath reflections
- Human body absorption
- Interference
- Antenna orientation

---

## 7. MQTT Communication

### 7.1 Why MQTT?

MQTT is used because it is:

- Lightweight
- Low bandwidth
- Efficient for IoT
- Publish/Subscribe based

---

### 7.2 MQTT Roles

| Component | Role |
|------------|------|
| ESP32 Base | Publisher |
| MQTT Broker | Message Router |
| Python Client | Subscriber |

---

### 7.3 Topic Structure

Example topic format:

```
ble/beacons/beacon1/rssi
ble/beacons/beacon2/rssi
```

---

### 7.4 Message Format (JSON)

Example payload:

```json
{
  "beacon_id": "BEACON_1",
  "mac": "AA:BB:CC:DD:EE:FF",
  "rssi": -62,
  "timestamp": 1700000000
}
```

QoS level typically used: 0 or 1

---

## 8. Python Client Application

### 8.1 Responsibilities

The Python application:

- Connects to MQTT broker
- Subscribes to beacon topics
- Parses JSON payload
- Updates table view in real-time
- Optionally converts RSSI to estimated distance

---

### 8.2 Python Libraries Used

- paho-mqtt
- json
- time
- Optional: PyQt / Tkinter for GUI

---

## 9. Data Flow

1. Beacon advertises BLE packets
2. Base scans and filters specific beacons
3. Base extracts RSSI
4. Base publishes RSSI via MQTT
5. Python client receives and displays data

---

## 10. Challenges Faced

- RSSI fluctuations
- Duplicate advertisement packets
- Interference from Wi-Fi
- Signal instability
- Accurate distance estimation

---

## 11. Solutions Implemented

- Filtering by MAC address
- Averaging RSSI values
- Using structured JSON format
- Proper topic hierarchy

---

## 12. Limitations

- RSSI is not precise for accurate indoor positioning
- Environmental factors affect signal
- Single base station cannot triangulate position

---

## 13. Future Improvements

- Implement Kalman filter for RSSI smoothing
- Add multiple base stations for triangulation
- Store data in database
- Create web dashboard
- Add authentication to MQTT
- Add device provisioning system

---

## 14. Use Cases

- Indoor asset tracking
- Smart warehouse monitoring
- Proximity detection system
- IoT-based monitoring systems

---

## 15. Conclusion

This project successfully demonstrates:

- BLE advertisement and scanning
- Embedded firmware development on ESP32
- MQTT-based IoT communication
- Real-time data visualization using Python

It integrates embedded systems, wireless communication, networking protocols, and application-level software into a complete IoT solution.

---

## Author

Developed by:
Sreenivasulu Boya

