**SAMR (Security Account Manager Remote Protocol)** is the "Account Management Tool."

In those malicious traffic pcaps, SAMR is often a "smoking gun" for **enumeration**. When an attacker gets onto a Windows machine, they use SAMR to "ask" the Domain Controller: *"Tell me every user that exists on this network."*

---

## 🗂️ Proto-Cardz #005: SAMR (Security Account Manager Remote)

**The "Brief":**

* **What it is:** A DCE/RPC-based protocol used to remotely query, create, modify, and delete accounts.
* **The Job:** It allows for remote management of users, groups, and passwords in the Windows SAM database or Active Directory.
* **The Port:** It runs over **TCP 445** (SMB) or **TCP 139**.

### 1. The Logic Flow (The RPC "Handshake")

1. **Connect:** The client connects to the `samr` interface via an SMB pipe (usually `\pipe\samr`).
2. **Open Policy:** The client asks for a "handle" (permission) to look at the domain.
3. **EnumDomains / EnumUsers:** The client asks to list all domains or all users.
4. **Close:** The client closes the handle and the connection.

### 2. The Artifact Hunt (Packet Data)

| Category | Value / Field | Wireshark Filter |
| --- | --- | --- |
| **Identity** | Domain Name | `samr.domain_name` |
| **Action** | Operation Name (e.g., EnumUsers) | `dcerpc.obj_id == samr` |
| **User Info** | Target Username | `samr.user_name` |

### 3. Searchable Artifacts (The Cheat Sheet)

* **`SamrEnumerateUsersInDomain`**: This is the big one. If you see this, the machine is literally asking for a list of every user account.
* **`SamrQueryInformationUser`**: The client is asking for specific details about a user (like when they last logged in or if their password expires).
* **`SamrSetInformationUser`**: **High Alert.** This indicates someone is trying to *change* an account (like resetting a password).
* **`RID (Relative Identifier)`**: Look for numbers like `500` (Administrator) or `501` (Guest). Attackers search for these specific IDs to find high-value targets.

### 4. Practical Investigation

* **The "So What?":** Normal users almost **never** use SAMR. It is mostly used by Admins or, more commonly, by attackers using tools like `BloodHound`, `net user /domain`, or `enum4linux` to map out your network.
* **The Red Flag:** A non-admin workstation suddenly calling `SamrEnumerateUsersInDomain`. This is a classic sign of the **Reconnaissance** phase of an attack.
* **The Power Filter:** `samr.opnum == 13`
*(This specifically filters for the "Enumerate Users" command. If you see this coming from a weird IP, start your incident response!)*