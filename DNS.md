## 🗂️ Proto-Cardz #001: DNS (Domain Name System)

**The "Brief":**

* **What it is:** The internet's phonebook/directory.
* **The Job:** Translates human-readable names (`google.com`) into machine-readable IP addresses (`142.250.190.46`).
* **The Port:** UDP 53 (most common) or TCP 53 (for large transfers/zone transfers).

### 1. The Logic Flow (How it Works)

1. **The Question (Query):** The client (your PC) sends a packet to the DNS server asking, *"Who is `evil-malware-site.com`?"*
2. **The Search (Recursion):** If the local server doesn't know, it asks higher-level servers until it finds the answer.
3. **The Answer (Response):** The server sends a packet back with the IP address (A record) or an error if the site doesn't exist (NXDOMAIN).

### 2. The Artifact Hunt (Packet Data)

| Category | Value / Field | Wireshark Filter |
| --- | --- | --- |
| **Identity** | Requested Domain Name | `dns.qry.name` |
| **Action** | Answer IP Address | `dns.a` |
| **Status** | Return Code (Error/Success) | `dns.flags.rcode` |

### 3. Searchable Artifacts (The Cheat Sheet)

* **`Standard query 0x... A`**: Look for this in the Info column to see what websites the victim is trying to reach.
* **`TXT Records`**: Malware often hides commands or encoded data inside DNS TXT records to bypass firewalls.
* **`NXDOMAIN`**: This means "Non-Existent Domain." A flood of these suggests a "Domain Generation Algorithm" (DGA) where malware is guessing names to find its server.
* **`Time to Live (TTL)`**: Very low TTLs (seconds) are often used by fast-flux botnets to keep changing their IP addresses quickly.

### 4. Practical Investigation

* **The "So What?":** DNS is almost never blocked by firewalls, making it the perfect "tunnel" for hackers to steal data or send commands.
* **The Red Flag:** **Large DNS packets.** Most DNS queries are small. If you see a DNS response that is 500+ bytes or uses TCP, someone might be tunneling data out of the network.
* **The Power Filter:** `dns.flags.response == 0`
*(This hides all the "answers" and shows you only the "questions" the computer asked—perfect for seeing a timeline of where a user went).* 