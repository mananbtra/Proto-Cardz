# Proto-Cardz
Structured network protocol deep-dives for defenders who live in Wireshark and think in packets — mapping logic flows, ports, and forensic artifacts.

**Proto-Cardz** is a structured flashcard collection designed to help cybersecurity students, SOC analysts, blue teamers, and forensic investigators quickly understand and analyze core network protocols.

Each card distills a protocol into its **purpose, logic flow, packet artifacts, and investigative value** — all in a consistent, practical format optimized for real-world packet analysis (Wireshark-ready).

---

## Included Protocols

This collection currently includes Proto-Cardz for:

* **DHCP** – Dynamic Host Configuration Protocol
* **DNS** – Domain Name System
* **LDAP** – Lightweight Directory Access Protocol
* **NetBIOS** – Network Basic Input/Output System
* **SAMR** – Security Account Manager Remote Protocol

More protocols will be added as the collection expands.

---

## Why Proto-Cardz?

Protocols are everywhere in network investigations — but documentation is often:

* Too theoretical
* Too fragmented
* Or too long to use during active analysis

Proto-Cardz bridges that gap by providing:

*  One-page protocol intelligence
*  Wireshark-friendly 🦈 artifact references
*  Investigation-oriented breakdowns
*  Clear logic flow stages
*  Real-world red flags 🚨

Whether you're reviewing a pcap, preparing for an exam, or sharpening your DFIR skills — these cards act as quick-reference operational guides.

---

## 🗂 Card Structure

Each Proto-Card follows a standardized layout for consistency and rapid scanning:

### **Proto-Cardz #XXX: [Protocol Name]**

#### 🔹 The Brief

* **What it is:** A one-sentence definition
* **The Job:** Why it exists (e.g., resolution, authentication, file transfer)
* **The Port:** Default transport protocol and port number

### 1️⃣ The Logic Flow (How It Works)

A stage-based breakdown of the protocol lifecycle:

* **Stage 1:** Initialization / Handshake
* **Stage 2:** Authentication / Negotiation
* **Stage 3:** Data / Command Exchange

This helps analysts understand *where* in the flow something suspicious may occur.

### 2️⃣ The Artifact Hunt (Packet Data)

A structured artifact extraction table:

| Category | Value / Field        | Wireshark Filter |
| -------- | -------------------- | ---------------- |
| Identity | Host, User, IP       | filter.field     |

Designed for rapid pivoting inside Wireshark.

### 3️⃣ 🔍 Searchable Artifacts (The Cheat Sheet)

A curated keyword list for:

* Packet Details search
* IOC extraction
* Threat hunting pivots

These are the strings and fields that matter during investigations.

### 4️⃣ Practical Investigation

Each card ends with operational insight:

**The "So What?"** – **The Red Flag** – **The Power Filter**

---

## How to Use

1. Open a protocol card.
2. Review the logic flow to understand session behavior.
3. Use the Artifact Hunt table to pivot inside Wireshark.
4. Apply the Power Filter to isolate meaningful traffic.
5. Watch for the Red Flag behavior.

---

## Who This Is For

* SOC Analysts
* Blue Teamers
* DFIR Practitioners
* Cybersecurity Students
* CTF Players
* Anyone analyzing pcaps

---

*Knowing *how* a protocol works is good.*
*Knowing *how to hunt through it* is better.*
