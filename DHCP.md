## 🗂️ Proto-Cardz #002: DHCP (Dynamic Host Configuration Protocol)

**The "Brief":**

* **What it is:** The "Information Desk" of the network.
* **The Job:** Automatically assigns IP addresses, subnet masks, and gateways to devices so they can communicate.
* **The Port:** UDP 67 (Server) and UDP 68 (Client).

### 1. The Logic Flow (The DORA Process)

1. **Discovery:** Client yells, "Is there a DHCP server out there?" (Broadcast).
2. **Offer:** Server says, "I'm here! You can use IP address `192.168.1.50`."
3. **Request:** Client says, "Great, I'll take that one!"
4. **Acknowledgement:** Server says, "Done. It's yours for the next 24 hours."

### 2. The Artifact Hunt (Packet Data)

| Category | Value / Field | Wireshark Filter |
| --- | --- | --- |
| **Identity** | Host Name of Device | `dhcp.option.hostname` |
| **Physical** | Client MAC Address | `dhcp.hw.addr` |
| **Network** | Requested IP Address | `dhcp.option.requested_ip_address` |

### 3. Searchable Artifacts (The Cheat Sheet)

* **`Option: (12) Host Name`**: This is the "Holy Grail" string. It tells you the actual name of the computer (e.g., `DESKTOP-H4K3R1`).
* **`Option: (61) Client identifier`**: Often contains the MAC address; useful if the hardware layer of the packet was stripped or changed.
* **`Option: (55) Parameter Request List`**: This shows what the device is asking for. Different OSs (Windows vs. Linux vs. IoT) ask for different parameters, allowing you to **fingerprint** the device type.
* **`Option: (6) Domain Name Server`**: Shows which DNS server the client was told to use. If this points to a weird, external IP, you might have found a "Rogue DHCP Server" attack.

### 4. Practical Investigation

* **The "So What?":** When you see a malicious IP in a pcap, you need to know which physical machine it belonged to at that exact moment. DHCP provides the link between the **IP** and the **Machine Name**.
* **The Red Flag:** **DHCP NAK (Negative Acknowledgement).** If you see many of these, it could indicate a "DHCP Starvation" attack where an attacker is trying to exhaust all available IPs to crash the network.
* **The Power Filter:** `dhcp.option.dhcp == 1`
*(This filters for "Discover" packets, which are the ones that almost always contain the Host Name of the machine starting up).*