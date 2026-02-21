## 🗂️ Proto-Cardz #004: LDAP (Lightweight Directory Access Protocol)

**The "Brief":**

* **What it is:** A protocol used to access and maintain distributed directory information services.
* **The Job:** It’s the "Corporate Directory." It allows users to find people, computers, and groups within a network (like Active Directory).
* **The Port:** TCP/UDP 389 (Standard) or TCP 636 (LDAPS/Over SSL).

### 1. The Logic Flow (The Client-Server Exchange)

1. **Bind Request:** The client asks to "log in" to the directory (often providing a username and password/hash).
2. **Search Request:** The client asks a question, like "Find the email address for user `jdoe`" or "List all members of the `Domain Admins` group."
3. **Search Result:** The server returns the requested attributes (the "entries").
4. **Unbind:** The client closes the connection.

### 2. The Artifact Hunt (Packet Data)

| Category | Value / Field | Wireshark Filter |
| --- | --- | --- |
| **Identity** | Distinguished Name (DN) | `ldap.distinguishedName` |
| **Action** | Search Filter (The query) | `ldap.filter` |
| **User Info** | SAMAcountName / UserID | `ldap.AttributeValue` |

### 3. Searchable Artifacts (The Cheat Sheet)

* **`bindRequest`**: This is the login attempt. Look here to find the **DN (Distinguished Name)** of the user trying to authenticate.
* **`sAMAccountName`**: This is the specific login ID of a user (e.g., `jsmith`).
* **`memberOf`**: A very important field that shows which security groups a user belongs to (e.g., "Administrators").
* **`Simple Authentication`**: If you see this in the "Bind" packet, it means the password might be sent in **cleartext**.
* **`objectClass`**: Tells you if the result is a "user," "computer," or "group."

### 4. Practical Investigation

* **The "So What?":** Attackers use LDAP to perform "Enumeration." Once they get a foothold, they query LDAP to find out who the high-value targets (Admins) are and which servers they can access.
* **The Red Flag:** **Brute Force or Massive Enumeration.** If you see a single IP address making hundreds of `searchRequest` packets for different users in a few seconds, someone is likely "scraping" your directory.
* **The Power Filter:** `ldap.protocolOp == 3`
*(This shows only the **Search Requests**. It’s the fastest way to see exactly what an attacker was looking for inside the network).*

### 5. Miscellaneous 
-  sasl buffer : simple authentication security layer - type of authentication - buffers contains info like encryption name, key , seq_num ldap uses sasl authentication. 