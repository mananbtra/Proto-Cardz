## 🗂️ Proto-Cardz #003: NBNS (NetBIOS Name Service)

**The "Brief":**

* **What it is:** The "Legacy Phonebook" for Windows.
* **The Job:** Helps old-school Windows machines find each other by **Computer Name** (NetBIOS name) instead of IP addresses.
* **The Port:** UDP 137.

### 1. The Logic Flow (How it Works)

1. **The Shout (Registration):** When a Windows PC starts, it shouts, "I am `DESKTOP-ABC`, and I'm at this IP!" to the whole network.
2. **The Question (Query):** If a PC wants to talk to `PRINTER-01`, it asks, "Who is `PRINTER-01`? Please tell me your IP."
3. **The Conflict:** If two PCs have the same name, NBNS sends a "Conflict" packet to stop them from using it.

### 2. The Artifact Hunt (Packet Data)

| Category | Value / Field | Wireshark Filter |
| --- | --- | --- |
| **Identity** | NetBIOS Name (Computer Name/Host Name) | `nbns.name` |
| **User** | Logged-in Username (Rare/Legacy) | `nbns.external_name` |
| **Type** | Node Type (B, P, M, or H) | `nbns.flags.node_type` |

### 3. Searchable Artifacts (The Cheat Sheet)

* **`<00>` (Workstation Service)**: If you see a name ending in `<00>`, that is the **Computer Name**.
* **`<03>` (Messenger Service)**: This is the field where you often find the **Logged-in User Account Name**—exactly what you found in your practical!
* **`<20>` (File Server Service)**: This indicates the machine is sharing files or folders.
* **`Registration` vs. `Query**`: "Registration" tells you who a machine *is*; "Query" tells you who a machine is *looking for*.

### 4. Practical Investigation

* **The "So What?":** In an incident, an IP address like `10.0.0.15` means nothing. NBNS gives that IP a human identity like `FINANCE-LAPTOP-01`. It turns "Traffic" into "Evidence."
* **The Red Flag:** **Spoofed Responses.** Attackers use tools like **Responder** to listen for NBNS queries and "pretend" to be the server the victim is looking for. This is how they steal NTLM hashes (passwords).
* **The Power Filter:** `nbns`
*(Since it's a simple protocol, just typing `nbns` in Wireshark usually gives you everything you need to identify every Windows machine on that segment).*