# THREE_MUSKETEERS
RAKSHA-NET: A Low-Cost Community Road Safety and Emergency Alert Network   


# 🛡️ RAKSHA-NET
### A Low-Cost Community Road Safety and Emergency Alert Network

**Team:** THREE MUSKETEERS  
**Institution:** Sri Sairam Engineering College, Chennai, Tamil Nadu  
**Hackathon:** MY Bharat Hack for Social Cause  
**Theme:** Governance & Civic Technology

> **When connectivity fails, community safety must not.**

RAKSHA-NET is a proposed community-powered road-safety alert network designed to help communicate road hazards to nearby participating devices and designated community responders. It combines ESP32-based embedded nodes, hybrid wireless communication using ESP-NOW and LoRa, optional GPS-based location tagging, and a responder dashboard for incident monitoring and acknowledgement.

The project aims to support timely hazard awareness and coordinated local response, particularly in rural and semi-urban areas where mobile connectivity may be unreliable.

---

## 📌 Table of Contents

- [Problem Statement](#-problem-statement)
- [Proposed Solution](#-proposed-solution)
- [System Architecture](#-system-architecture)
- [How It Works](#-how-it-works)
- [Technology Stack](#-technology-stack)
- [Key Features](#-key-features)
- [Target Beneficiaries](#-target-beneficiaries)
- [Current Progress and MVP Plan](#-current-progress-and-mvp-plan)
- [Performance Evaluation](#-performance-evaluation)
- [Future Enhancements](#-future-enhancements)
- [Privacy and Safety](#-privacy-and-safety)
- [Repository Structure](#-repository-structure)
- [Team](#-team)
- [References](#-references)

---

## 🚨 Problem Statement

Road hazards, accidents, fallen trees, waterlogging, and road blockages may not be communicated promptly to nearby road users or local responders.

Common challenges include:

- Dependence on mobile connectivity for conventional reporting.
- Fragmented communication between road users and local responders.
- Limited visibility of locally reported hazards.
- Difficulty confirming whether an alert has been received.
- Cost and deployment constraints associated with large-scale infrastructure.

RAKSHA-NET explores a low-cost, modular communication system that can relay hazard reports between participating wireless nodes and provide a centralized interface for authorized responders.

The need and effectiveness of the proposed solution must be validated through stakeholder interviews and controlled pilot testing.

## 💡 Proposed Solution

RAKSHA-NET uses a hybrid wireless architecture to transmit hazard information through suitable communication links.

An authorized user or roadside node reports an incident. An ESP32-based node creates a structured alert, evaluates the available communication path, and sends the message using ESP-NOW or LoRa according to the supported network configuration.

A receiving node or gateway can relay the alert where supported. When internet connectivity is available, the gateway can forward incident information to a web dashboard for authorized responders to review and acknowledge.

### Example hazard categories

- 🚧 Road obstruction or construction diversion
- 🚗 Reported road accident
- 🌧️ Heavy rain or waterlogging
- ⚠️ Other locally reported hazards

**Important:** The initial concept uses manually reported hazards. Automatic accident detection and AI-assisted risk prediction are future enhancements, not assumed existing capabilities.

---

## 🏗️ System Architecture

The following diagram illustrates the proposed end-to-end architecture, including reporting nodes, hybrid wireless communication, optional relay nodes, the gateway, dashboard, and future extensions.

![RAKSHA-NET System Architecture](docs/images/raksha-net-architecture.png)

*Figure 1: RAKSHA-NET system architecture. Replace the image path above with the actual location of your architecture diagram in the repository.*

### Architecture components

| Component | Responsibility |
|---|---|
| Hazard reporting interface | Allows an authorized user to report an incident |
| ESP32 reporting node | Creates and transmits the hazard message |
| GPS module (optional) | Provides location coordinates when available |
| ESP-NOW | Supports suitable short-range communication between ESP32 devices |
| LoRa RA-02 / SX1278 | Supports low-data-rate, longer-range wireless communication |
| Link-selection logic | Selects a supported communication path based on link suitability |
| Intermediate relay nodes (optional) | Forward messages when the network topology supports relaying |
| Receiving node / gateway | Receives alerts and forwards data to the dashboard when connectivity is available |
| Web dashboard | Displays incident information and acknowledgement status |
| Database (planned) | Stores incident records for authorized access and later analysis |

### Communication design

The communication layer considers:

- Link availability and quality.
- Received Signal Strength Indicator (RSSI) and Signal-to-Noise Ratio (SNR), where available.
- Packet delivery success and retransmissions.
- Message acknowledgement and duplicate suppression.
- Latency and energy consumption.

ESP-NOW and LoRa have different communication characteristics. Their practical range and reliability depend on hardware configuration, antenna, interference, obstructions, and deployment conditions.

The architecture does not assume that every ESP32 node can automatically bridge ESP-NOW and LoRa. Such forwarding requires explicit firmware and gateway or relay logic.

---

## 🔄 How It Works

The proposed workflow is as follows:

**Step 1 — Hazard Reporting**

An authorized user submits a hazard category, timestamp, and available location through a reporting interface or manual roadside trigger.

**Step 2 — Message Creation**

The ESP32 creates a structured message with an identifier and incident details.

Example payload:

```json
{
  "id": "RX001",
  "type": "ROAD_HAZARD",
  "category": "ROAD_OBSTRUCTION",
  "timestamp": "2026-10-25T10:30:00",
  "latitude": 12.9786,
  "longitude": 80.1234
}
```

*The values above are illustrative, not a real incident record. Use an appropriate time format and valid coordinates in the implementation.*

**Step 3 — Communication Selection**

The node evaluates supported communication options and chooses an available path according to the implemented switching logic.

**Step 4 — Wireless Transmission**

The alert is transmitted through ESP-NOW or LoRa, depending on the configured communication path.

**Step 5 — Reception and Optional Relay**

A receiving node processes the packet. Intermediate nodes may forward it if relay functionality has been implemented and configured.

**Step 6 — Dashboard Update**

A gateway forwards the incident to the dashboard when a suitable internet or local network connection is available.

**Step 7 — Acknowledgement**

An authorized responder reviews the incident and acknowledges receipt. The system can use message identifiers, acknowledgements, bounded retries, and duplicate suppression to manage delivery.

**Workflow summary:**

`Report → Create Message → Select Link → Transmit → Receive / Relay → Dashboard → Responder Acknowledgement`

---

## 🧰 Technology Stack

### Hardware

| Component | Purpose |
|---|---|
| ESP32-WROOM-32 | Embedded controller and wireless communication |
| LoRa RA-02 / SX1278 | Long-range, low-data-rate radio communication |
| GPS NEO-6M (optional) | Location tagging |
| Push button (optional) | Manual hazard trigger |
| Laptop / local server | Dashboard hosting and development |

### Software

| Technology | Purpose |
|---|---|
| ESP32 firmware | Message creation and wireless communication |
| ESP-NOW | Short-range ESP32 communication |
| LoRa communication library | Radio packet transmission and reception |
| C / C++ | Embedded firmware and communication logic |
| Python / web technologies | Dashboard and backend implementation, as finalized |
| FastAPI (planned option) | REST API for incident management |
| React or Streamlit (planned option) | Dashboard interface |
| SQLite or Firebase (planned option) | Incident storage |

The software stack may be adjusted to match the implementation. Technologies marked as planned options should only be listed as implemented after they are integrated and tested.

---

## ✨ Key Features

### 1. Hybrid Wireless Communication
Combines ESP-NOW and LoRa according to supported hardware and network configuration.

### 2. Adaptive Link Selection
Evaluates communication conditions and selects a suitable path using implemented switching logic.

### 3. Structured Hazard Messages
Includes a message identifier, incident category, timestamp, and available location.

### 4. Message Reliability Mechanisms
Supports planned or implemented acknowledgement, bounded retries, and duplicate suppression.

### 5. Optional Location Tagging
Uses GPS coordinates when available to help responders identify the reported location.

### 6. Community Responder Dashboard
Provides an interface for incident monitoring, status review, and acknowledgement.

### 7. Modular Design
Allows additional nodes, reporting interfaces, and dashboard capabilities to be introduced as the project develops.

---

## 👥 Target Beneficiaries

RAKSHA-NET is intended to support:

- Two-wheeler riders and daily commuters.
- Pedestrians and school communities.
- Rural and semi-urban road users.
- Community volunteers.
- Educational institutions and road-safety groups.
- Designated local responders.

The system is intended to supplement existing reporting and response mechanisms, not replace emergency services or official road-safety systems.

---

## 🧪 Current Progress and MVP Plan

The project builds on prior hybrid vehicle-to-vehicle communication work.

### Existing technical foundation

The team's project documentation describes:

- ESP32 packet-exchange experiments.
- ESP32 and LoRa-related dashboard work.
- Switching-logic simulation using QNX SDP 8.0 / QEMU.

These activities provide a foundation for adapting the communication architecture to a community road-safety use case.

### Minimum Viable Product (MVP)

The next demonstrable version is planned to include:

1. Two ESP32-based wireless nodes.
2. A manual hazard-report trigger.
3. Structured incident message creation.
4. ESP-NOW / LoRa transmission according to the implemented configuration.
5. Message reception and acknowledgement.
6. A basic dashboard displaying incident information and status.
7. Controlled communication-performance testing.

### Implementation roadmap

| Phase | Planned activity | Validation |
|---|---|---|
| 1 | Validate the local problem | Stakeholder interviews |
| 2 | Build two wireless nodes | Successful packet exchange |
| 3 | Implement reporting and acknowledgement | End-to-end message tests |
| 4 | Develop the dashboard | Incident display and status updates |
| 5 | Measure communication performance | Repeatable laboratory tests |
| 6 | Conduct a supervised pilot, subject to permission | Documented field results |

**Current status:** The architecture includes both an existing technical foundation and proposed road-safety functionality. Do not describe the complete MVP or a community deployment as operational until those stages have been implemented and demonstrated.

---

## 📊 Performance Evaluation

The system will be evaluated using measurable technical indicators.

| Metric | What it measures |
|---|---|
| Alert delivery rate | Percentage of transmitted alerts successfully received |
| End-to-end latency | Time between report creation and alert reception |
| Packet loss | Proportion of transmitted packets not received |
| Communication range | Distance over which the link meets defined test criteria |
| Acknowledgement time | Time between alert transmission and acknowledgement |
| Energy consumption | Power or energy used during defined operating conditions |
| Duplicate alert rate | Frequency of repeated messages reaching the application |
| Prototype cost | Hardware and assembly cost per node |

No performance values are claimed here. Record actual results under specified test conditions before reporting numerical achievements.

A controlled pilot should document environmental conditions, node placement, antenna configuration, packet count, and test duration so that results can be interpreted and reproduced.

---

## 🚀 Future Enhancements

### AI-Assisted Risk Analysis
Analyse verified incident history to identify recurring hazard categories and locations. Any predictive capability would require sufficient reliable data and evaluation.

### GIS-Based Hazard Mapping
Visualize verified reports on a map and identify locations requiring road-safety inspection.

### Automated Hazard Detection
Explore sensor-based or computer-vision detection as a future extension, with false-positive testing and human verification.

### Mobile Reporting Application
Provide a user-friendly interface for authorized hazard reporting and alert reception.

### Additional Communication Technologies
Investigate integration with other communication methods where technically feasible and permitted.

### Community and Institutional Integration
Explore collaboration with educational institutions, community organizations, and authorized local responders, subject to permissions and agreements.

---

## 🔐 Privacy and Safety

RAKSHA-NET is a proposed supplementary alert system. Responsible development should include:

- Consent and appropriate access control.
- Collection of only necessary location and incident information.
- Authorized reporting and human verification.
- Protection against duplicate, malicious, or misleading reports.
- Controlled testing before any roadside deployment.
- Permission from relevant authorities before field installation.
- Clear communication that alerts may be delayed or lost.

The system does not guarantee accident prevention, message delivery, or emergency-service response.

---

## 📁 Repository Structure

A suggested repository organization is:

```text
RAKSHA-NET/
├── README.md
├── docs/
│   ├── images/
│   │   └── raksha-net-architecture.png
│   ├── architecture.md
│   └── test-results/
├── firmware/
│   ├── reporting_node/
│   ├── receiving_node/
│   └── relay_node/
├── dashboard/
│   ├── frontend/
│   └── backend/
├── simulation/
├── tests/
├── hardware/
│   └── bill-of-materials.md
├── .gitignore
└── LICENSE
```

This is a recommended structure; retain only directories and files that actually exist in the repository.

### Adding the architecture image

1. Create a folder named `docs/images/`.
2. Save your final architecture diagram as `raksha-net-architecture.png` inside that folder.
3. Commit the image to GitHub.
4. Keep the Markdown image reference in this README synchronized with the actual filename.

If you store the image in the repository root instead, update the Markdown reference accordingly.

---

## 👨‍💻 Team

**Team Name:** THREE MUSKETEERS  
**Institution:** Sri Sairam Engineering College, Chennai, Tamil Nadu

- Raghav G S
- Aakash P
- Yugenthar A P

Team responsibilities should be added after the members confirm their actual roles.

---

## 📚 References

1. Jiang, D. and Delgrossi, L. (2008). *IEEE 802.11p: Towards an International Standard for Wireless Access in Vehicular Environments.* IEEE Vehicular Technology Conference.
2. Karagiannis, G. et al. (2011). *Vehicular Networking: A Survey and Tutorial on Requirements, Architectures, Challenges, Standards and Solutions.* IEEE Communications Surveys & Tutorials, 13(4), 584–616.
3. Molina-Masegosa, R. and Gozalvez, J. (2017). *LTE-V for Sidelink 5G V2X Vehicular Communications.* IEEE Vehicular Technology Magazine, 12(4), 30–39.
4. Khondoker, R. et al. (2018). *Vehicular Ad Hoc Networks (VANETs): Challenges and Simulation Tools.* Computer Networks, 145, 112–135.

These references provide background on vehicular networking and wireless communication. Additional references specific to ESP-NOW, LoRa, ESP32 hardware, and dashboard technologies should be added as those components are implemented.

---

## 📌 Project Information

**Project:** RAKSHA-NET  
**Team:** THREE MUSKETEERS  
**Theme:** Governance & Civic Technology  
**Location:** Tamil Nadu, India  
**Purpose:** Community road-safety awareness and emergency alert coordination

> RAKSHA-NET aims to turn local hazard information into shared community awareness through affordable wireless communication and accountable incident reporting.

