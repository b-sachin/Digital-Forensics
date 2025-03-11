# AmCache.hve File

## Understanding AmCache.hve  

### What is AmCache?  
`AmCache.hve` logs:  
- **Program execution history**  
- **File metadata (hashes, timestamps)**  
- **Installed software details**  
- **USB and removable device usage**  

### 📍 Location of AmCache.hve  
```
C:\\Windows\\AppCompat\\Programs\\AmCache.hve
```

### Differences Between AmCache, Prefetch, and ShimCache  

| Feature | AmCache | Prefetch | ShimCache |
|---------|---------|----------|-----------|
| **Purpose** | Logs executed applications | Caches frequently used programs | Tracks application execution |
| **Retention** | Persistent (until deleted) | Limited (stores last 128 entries) | May be overwritten |
| **Timestamp Stored** | First execution time | Last execution time | Last modified time |

---

## Forensic Artifacts Stored in AmCache  

### Key Forensic Data in AmCache.hve  
1. **Executed Programs** – Helps in **malware analysis and APT detection**  
2. **File Metadata** – File path, name, **SHA-1 hash**, digital signatures  
3. **Program Execution Timestamps** – Helps correlate **incident timelines**  
4. **Device Information** – Tracks **USB and external storage activity**  
5. **Persistence Mechanisms** – Used by **malware for execution persistence**  

---

## Analyzing AmCache.hve  

### Tools for Extracting and Parsing AmCache  
| Tool | Purpose |
|------|---------|
| **AmcacheParser (Eric Zimmerman)** | Extracts and parses AmCache data |
| **RegRipper** | Automated registry analysis |
| **FTK Imager** | Extracts AmCache.hve for forensic review |
| **Plaso (Log2Timeline)** | Correlates AmCache data with other forensic artifacts |

### Step-by-Step AmCache Analysis  
1. **Extract AmCache.hve**  
2. **Use AmcacheParser to parse the data**  
   ```
   AmcacheParser.exe -f AmCache.hve -o output.csv
   ```  
3. **Analyze Execution Timestamps & Hashes**  
4. **Correlate with other forensic evidence (event logs, Prefetch, ShimCache)**  

---

## Case Study: Investigating Malware Execution Using AmCache  

### Scenario:  
An organization detects **suspicious outbound traffic** from an employee's computer. Security analysts suspect that **malware** is running in the background, exfiltrating sensitive data. However, traditional antivirus scans **fail to detect any threats**.  

### Investigation Process:  

1️⃣ **Extract AmCache.hve from the suspect's machine**  
   - The forensic investigator copies the file from:  
     ```
     C:\\Windows\\AppCompat\\Programs\\AmCache.hve
     ```

2️⃣ **Analyze AmCache using AmcacheParser**  
   - The investigator runs:  
     ```
     AmcacheParser.exe -f AmCache.hve -o output.csv
     ```  
   - The parsed results reveal an **unrecognized executable**:  
     ```
     C:\\Users\\JohnDoe\\AppData\\Roaming\\updater.exe
     ```

3️⃣ **Verify the SHA-1 Hash of the Executable**  
   - The investigator checks the **SHA-1 hash** of `updater.exe` against VirusTotal:  
     - **Result:** The file is flagged as **"AgentTesla RAT"**, a known **remote access trojan (RAT)** used for **data theft and keylogging**.

4️⃣ **Correlate AmCache Data with Other Artifacts**  
   - The investigator checks **ShimCache, Prefetch, and event logs** to determine:  
     - **When** the malware was first executed.  
     - **How** it gained persistence on the system.  
     - **If** any other suspicious files were dropped.  

5️⃣ **Conclusion & Remediation**  
   - The forensic team confirms that **"updater.exe" was executed two days before the alert**.  
   - **USB device logs** from AmCache suggest that the malware was **introduced via an infected USB drive**.  
   - The security team **removes the malware, isolates affected systems, and strengthens USB security policies**.  

---

## Lessons Learned from This Case Study  
✔ **AmCache can track malware execution even if the file is deleted**.  
✔ **SHA-1 hashes help quickly verify threats using online databases**.  
✔ **USB activity logs in AmCache can help trace infection sources**.  
✔ **Correlating AmCache with other artifacts strengthens forensic analysis**.  

---

## Challenges and Limitations  

### Limitations of AmCache Analysis  
- **Data Overwrites** – Older entries may be replaced over time.  
- **Limited Timestamps** – Stores only **first execution timestamp**, not subsequent runs.  
- **Tampering by Attackers** – Malware can modify or delete AmCache entries.  

### How to Validate AmCache Data  
✔ Cross-reference with **Prefetch, Event Logs, and ShimCache**.  
✔ Check integrity using **digital signatures and hash analysis**.  
✔ Look for **deleted registry entries** using forensic tools like **Volatility**.  

---

# Conclusion: Why AmCache Is Critical in Forensics  
✔ **Tracks historical program execution**  
✔ **Records metadata and digital signatures**  
✔ **Links malware execution to timestamps and file hashes**  
✔ **Survives longer than event logs**  

**Forensic investigators use AmCache to track unauthorized access, detect malware infections, and analyze system activity.**  
