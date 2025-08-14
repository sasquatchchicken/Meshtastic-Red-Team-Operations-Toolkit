# Meshtastic Red Team & Lawful Operations Toolkit  
*A Controlled Environment Mesh Network Assessment Suite*

[![Lawful Use Only — Authorized Red Team Toolkit](https://img.shields.io/badge/Lawful%20Use%20Only-Authorized%20Red%20Team%20Toolkit-red?style=for-the-badge&logo=security&logoColor=white)](#)
[![Air-Gapped / Faraday Lab Tested](https://img.shields.io/badge/Air--Gapped%20Lab-Tested-blue?style=for-the-badge&logo=hackaday&logoColor=white)](#)
[![Meshtastic-Compatible](https://img.shields.io/badge/Meshtastic-Compatible-green?style=for-the-badge&logo=meshery&logoColor=white)](#)

---

##  Overview
This repository contains Python scripts and CLI workflows for **Meshtastic mesh network node discovery, telemetry logging, and secure over-the-air (OTA) configuration changes**.

**All tools in this suite are designed for:**
- **Air-gapped Faraday lab environments**
- **Devices owned by the testing entity**
- **Authorized red team assessments, vulnerability research, and operational training**

The toolkit demonstrates how adversary-like capabilities can be applied for **lawful missions** such as:
- Counter–human trafficking
- Counter–narcotics
- Search & rescue in low-infrastructure areas
- Emergency mesh re-keying for field operations

---

## Ethical Use & Disclaimer 
> These tools must **never** be used on devices, networks, or channels without explicit permission.  
> Misuse of these tools is illegal and unethical.  
> This repository is intended only for **lawful, authorized testing**.

---

## Features

### **1. Node Discovery & Telemetry Logger**
- Blind scan for all reachable Meshtastic nodes.
- Collects and timestamps:
  - Node ID, MAC address, GPS coordinates
  - Device role, battery, voltage, SNR, hops
  - Public key, local PSK status
- Avoids duplicate logging via persistent JSON database.
- Evidence-ready JSON output for intelligence and analysis.

**Example Output:**
```json
{
    "node_id": "!xxxxxx", --> not shown for security reasons
    "first_seen": "2025-08-12T18:15:30.209630Z",
    "last_seen": "2025-08-12T18:15:40.256837Z",
    "role": "UNKNOWN",
    "long_name": "Meshtastic e5ec",
    "short_name": "mKr",
    "latitude": 42.2313984,
    "longitude": -73.8459648,
    "battery": 91,
    "voltage": 4.071,
    "snr": 10.5,
    "hops": 1,
    "mac_address": "xxxxx+Xx",  --> not shown for security reasons
    "public_key": "Dy0ge+GYvXrqh5QlSgI2RPMucmYwOlk7wP8ArhSogkI=",
    "local_psk": "PSK_ERROR"
}
```
### **2. PSK Payload Simulator**
-Uses an authorized transmitter node to send a 256-bit channel PSK to a remote target node.

-Demonstrates secure OTA key rotation via Meshtastic’s PKC (Public Key Cryptography) model.

-Supports targeting by Node ID from the discovery JSON file.

## **3. Remote Command & OTA Control**
-CLI workflows for:

-Reading admin/public keys

-Provisioning target nodes with admin keys

-Sending OTA configuration updates

-Works on both Windows (COMx) and Linux (/dev/ttyUSBx) Kali raspi4 (/dev/ttyACMx).

## Operational Workflow Diagram
```
flowchart TD
    A -->[Node Discovery Script] -->|Blind Scan|   [Telemetry JSON Log]
    B -->|Select Target Node ID|                   [PSK Payload Simulator]
    C -->|Generates OTA PSK Update|                [Meshtastic CLI Commands]
    D -->|Transmitter Node Sends OTA|              [Target Node]
    E -->|PSK Updated|                             [Verification via Discovery Script]
```
## **Red Team Mission Workflow — Lawful Engagement Scenario**
```
flowchart LR
    subgraph Field Deployment
        LEV[Law Enforcement Vehicle]
        PTX[Portable Transmitter Node]
        RX[Portable Laptop-windows or linux / field device kali box Raspi4]
```
```
    subgraph Mesh Network
        TN1[Target Node — Lab-owned Device]
        N2[Other Test Nodes]
        N3[Repeater Node]
```
```
    LEV -->|Operator Commands| RX
    RX -->|Run Discovery Script| PTX
    PTX -->|Blind Scan + Log| RX
    RX -->|Select Target Node ID| PTX
    PTX -->|OTA PSK Injection| TN1
    TN1 -->|Mesh Response| N2
    N2 -->|Forward| N3
    N3 -->|Forward| PTX
    PTX -->|Verification Telemetry| RX
```



## **Repository Structure**
**for security purposes we have chosen not to provide any of the working scripts or Meshtastic CLI syntaxes used in testing.  The tests were 100% successful and we have actual video results of the testing engagement**
```
├── discovery_logger.py         # Node scanning & telemetry logging
├── psk_payload_simulator.py    # OTA PSK injection simulation
├── cli_workflows.md            # Tested CLI command sequences
├── new node discovered.json    # Example output file
└── README.md                   # This file
```
## Lawful Use Cases
-Counter–Human Trafficking: Locate and track suspect mesh nodes, re-key seized devices to join a monitored law-enforcement channel.

-Counter–Narcotics: Detect logistics comms, disrupt cartel LoRa networks via OTA PSK changes.

-Search & Rescue: Identify and re-configure lost hiker devices to join rescue mesh.

## **Security Notes**
PKC enforcement blocks OTA changes unless an admin key is set.

Private keys are never sent over mesh; must be read locally via serial.

Unprotected nodes (no admin key) can be reconfigured by any node in range.
