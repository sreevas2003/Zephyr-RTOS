# BLE Applications Overview
A typical BLE application consists of two devices: a peripheral and a central. Before establishing a connection, a peripheral broadcasts its presence via a process called BLE advertising. The central scans for available peripherals. Once the central finds the desired peripheral, a connection is established between the two.

<img width="505" height="277" alt="image" src="https://github.com/user-attachments/assets/8050d292-40b8-4e26-a521-213e2088c2d6" />

# BLE Stack Architecture
The BLE stack architecture is typically divided into three main layers: the application layer, the host layer, and the controller layer.

<img width="628" height="431" alt="image" src="https://github.com/user-attachments/assets/c3b12f20-f08d-4108-9c93-e69cd2f92d5c" />

### Application
The application layer is where the specific functionality of BLE-enabled devices is implemented. It interacts with the lower layers of the stack through the generic attribute profile (GATT), which allows the definition of services, characteristics, and the corresponding data.

### Controller Layer (Lower Layer)

**Components:**

- Physical Layer (PHY)
- Link Layer (LL)

**🔹 Physical Layer (PHY)**

<img width="884" height="312" alt="image" src="https://github.com/user-attachments/assets/642214a4-0afb-4494-9ef7-c9c056e561d8" />

Operates in 2.4 GHz ISM band
Uses:
- 40 channels
- 3 Advertising channels (37, 38, 39)
- 37 Data channels
Modulation: GFSK
Data rates:
- 1 Mbps (BLE 4.x)
- 2 Mbps (BLE 5)

**🔹 Link Layer (LL)**

Responsible for:
- Advertising
- Scanning
- Connection establishment
- Packet formatting
- Retransmissions
- Channel hopping

It ensures reliable communication between devices.

### Host Layer (Upper Layer)

**🔹 HCI (Host Controller Interface)**

Interface between Host and Controller

Can be:
- UART
- SPI
- USB

**🔹 L2CAP (Logical Link Control and Adaptation Protocol)**

- Allows multiple protocols to share one BLE connection.
- Segmentation and reassembly of packets : BLE packets have size limits. If upper layer sends large data: 1. L2CAP splits it and 2. Receiver reassembles it

**SMP (Security Manager Protocol)**

SMP (Security Manager Protocol) is the part of the BLE stack responsible for:
- Pairing
- Bonding
- Key distribution
- Encryption setup

It ensures secure communication between BLE devices.

**🔹 ATT (Attribute Protocol)**

- Defines how data is organized
- Works in Client–Server model
- Uses attributes (handle, type, value)

**🔹 GATT (Generic Attribute Profile)**

Builds on ATT

Defines:
- Services
- Characteristics
- Descriptors

This is what most application developers work with.

**🔹 GAP (Generic Access Profile)**

Defines device roles and connection procedures.

Roles:
- Broadcaster
- Observer
- Peripheral
- Central
