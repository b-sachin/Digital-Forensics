# USRClass.dat Hive File

## Overview

`USRClass.dat` is a **Windows registry hive file** that plays a crucial role in digital forensics. 
It contains user-specific data, including **COM class registrations, file associations, shell settings, 
and evidence of user activity**. Forensic analysts examine it to reconstruct **user actions, 
application usage, and system modifications**.

## Why Is USRClass.dat Important in Forensics?

- **Contains Evidence of User Activity:** Tracks **file interactions, program usage, and recent actions**.
- **Stores ShellBag Data:** Helps investigators **determine what folders were accessed and when**.
- **File Association Manipulation:** Can reveal **malware persistence techniques**.
- **User-Specific COM Object Registrations:** Can indicate **unauthorized software installations**.
- **Survives User Deletions:** Unlike event logs, registry hives like USRClass.dat **persist longer**.

## Location of USRClass.dat

```
C:\Users\{Username}\AppData\Local\Microsoft\Windows\USRClass.dat
```

- This is a **per-user registry hive**, meaning it stores settings only for that specific user.
- Unlike `NTUSER.dat`, it is **not part of a roaming profile**, making it useful for **local forensic investigations**.

## Key Forensic Artifacts in USRClass.dat

### 1. ShellBag Data - Tracing Folder Access

ShellBag records track folders that a user has opened using Windows Explorer.

- **Forensic Path:**  
  ```
  HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
  ```
- **What it reveals:**  
  - **Which folders were accessed**
  - **Timestamps of access**
  - **USB or network drive usage**

**Use Case:** Helps reconstruct **timeline of user activity**, even for **deleted folders**.

### 2. File Associations - Identifying Program Execution

- **Forensic Path:**  
  ```
  HKEY_CLASSES_ROOT\Applications\{ProgramName}
  ```
- **What it reveals:**  
  - **Which files were opened using which programs**
  - **What software a user prefers for certain file types**
  - **Potential signs of malware modifying file handlers**

**Use Case:** Determines **suspicious file executions** (e.g., `.exe` files run using `notepad.exe` could indicate tampering).

### 3. COM Class Registrations - Detecting Malware Persistence

Malware often registers itself as a COM object to execute when Windows starts.

- **Forensic Path:**  
  ```
  HKEY_CLASSES_ROOT\CLSID
  ```
- **What it reveals:**  
  - **Which COM objects are installed for a specific user**
  - **Suspicious DLLs registered for execution**
  - **Persistence mechanisms of malware**

**Use Case:** Identifies **hidden backdoors, trojans, or rootkits**.

### 4. User-Specific App Data - Identifying Application Usage

- **Forensic Path:**  
  ```
  HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\CurrentVersion
  ```
- **What it reveals:**  
  - **Installed UWP applications**
  - **Recent application launches**
  - **Program usage habits**

**Use Case:** Helps track **unauthorized or suspicious software usage**.

## Tools for Analyzing USRClass.dat in Forensics

| Tool                                   | Purpose                                                   |
|----------------------------------------|-----------------------------------------------------------|
| **Registry Explorer (Eric Zimmerman)** | Advanced forensic registry analysis                       |
| **FTK Imager**                         | Extracts and mounts registry hives                        |
| **Hivex (Linux)**                      | Parses registry hives in forensic investigations          |
| **RegRipper**                          | Automates registry analysis for malware and user activity |

## How to Analyze USRClass.dat in Forensic Investigations

### Step 1: Extract USRClass.dat

1. Boot into a **forensic environment** (e.g., using FTK Imager).
2. Navigate to:
   ```
   C:\Users\{Username}\AppData\Local\Microsoft\Windows\
   ```
3. Copy **USRClass.dat** without modifying timestamps.

### Step 2: Mount the Hive for Analysis

1. Open **Registry Editor (`regedit`)**.
2. Click **HKEY_LOCAL_MACHINE**.
3. **File → Load Hive → Select USRClass.dat**.
4. Assign a name (e.g., `USRClass_Analysis`).

### Step 3: Examine Key Artifacts

- **ShellBag Data:**  
  ```
  HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
  ```  
  → Shows accessed folders, timestamps, and deleted paths.

- **File Execution History:**  
  ```
  HKEY_CLASSES_ROOT\Applications
  ```  
  → Shows which files were opened and which programs were used.

- **Suspicious COM Objects:**  
  ```
  HKEY_CLASSES_ROOT\CLSID
  ```  
  → Look for **unknown CLSIDs linked to malware files**.

## Case Study: Investigating Suspicious Activity

### Scenario:

A forensic investigator is analyzing a **compromised Windows machine** suspected of **data exfiltration**.

### Steps Taken:

1. **Extracted USRClass.dat** from the suspect’s profile.
2. **Loaded the hive** in Registry Explorer.
3. **Examined ShellBag data** to find evidence of **USB device usage**.
4. **Analyzed file associations** to see if any **sensitive files** were opened recently.
5. **Checked COM objects** for signs of **malware persistence**.

### Findings:

- Evidence showed that the user accessed **confidential documents** stored on a **USB drive**.
- **COM object analysis** revealed a **malware DLL** registered to persist across reboots.
- File associations showed that an **unauthorized program** (`stealer.exe`) was used to open `.docx` files.

## Conclusion: Why USRClass.dat Is Crucial in Forensics

- **Tracks user activity** (file access, program execution).
- **Contains artifacts of malware persistence** (COM hijacking, file associations).
- **Helps recover evidence of deleted files and folders** (ShellBag analysis).
- **Survives user deletion attempts** (registry hives persist across reboots).

**Forensic analysts rely on USRClass.dat** to reconstruct user actions, detect malware activity, and build digital evidence in cybercrime investigations.
