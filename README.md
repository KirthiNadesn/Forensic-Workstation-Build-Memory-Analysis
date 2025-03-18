# Forensic-Workstation-Build-Memory-Analysis

## **Overview**
This project documents the setup and configuration of a forensic workstation using **Windows 10 VM, Ubuntu (WSL), and forensic tools** such as **Volatility3, Cyber Triage, and OleTools**. The goal is to perform memory analysis on a captured memory dump to detect security incidents and potential threats.

---

## **Table of Contents**
1. [Forensic Workstation Setup](#-forensic-workstation-setup)
2. [Configuring the Environment](#-configuring-the-environment)
3. [Installing Forensic Tools](#-installing-forensic-tools)
4. [Capturing Memory Dump](#-capturing-memory-dump)
5. [Memory Analysis using Volatility](#-memory-analysis-using-volatility)
6. [Memory Analysis using OleTools](#-memory-analysis-using-oletools)
7. [Memory Analysis using Cyber Triage](#-memory-analysis-using-cyber-triage)
8. [Conclusion](#-conclusion)

---

## **🖥️ Forensic Workstation Setup**
### **1️⃣ Setting Up Windows Subsystem for Linux (WSL)**
The hypervisor used to build the forensic workstation is **VMware**, and the setup is done inside a **Windows 10 VM**.

1. Open PowerShell as Administrator and enable WSL:
   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
   ```
2. Download Ubuntu 20.04 from the Microsoft Store or manually install it.
3. Change the downloaded file extension from .appxbundle to .zip by checking “Hide extensions for known file types” in File Explorer options.
4. Extract the zipped Ubuntu package to the UbuntuonWindows folder.
5. Install Ubuntu using the following command:
```powershell

Add-AppxPackage C:\Users\YourUser\Desktop\UbuntuonWindows\Ubuntu_2004.2021.825.0_x64.appx
```
6. Restart the system and complete Ubuntu setup.

## **Configuring the Environment**
### **2️⃣ Creating Forensic Folders**
1. Create dedicated directories for forensic work inside **C:**:
```powershell

mkdir C:\Forensic_Cases
mkdir C:\Forensic_Tools
```
2. Set the system time zone to UTC.
3. Enable Show Hidden Files in File Explorer.
4. Exclude these forensic folders from Windows Defender:
  - Open Windows Security → Manage settings → Add an exclusion.
  - Add both C:\Forensic_Cases and C:\Forensic_Tools to prevent automatic file removal during investigations.
    
## **🔬 Installing Forensic Tools**
### **3️⃣ Updating System and Installing Dependencies**
Run the following commands inside Ubuntu (WSL):

```bash

sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip -y
```
### **4️⃣ Installing Volatility 3**
```bash

pip3 install volatility3
```
- Verify installation:

```bash

vol
```
This should display Volatility 3 Framework 2.8.0.

## **5️⃣ Installing Cyber Triage**
1. Download Cyber Triage 3.11.10 from official website.
2. Install the application.
   
## **6️⃣ Installing OleTools**
```bash

pip3 install oletools
```
- Verify installation:

```bash

olevba --help
```
This confirms that olevba and related tools are available for analysis.

### **Capturing Memory Dump**
To capture the memory of the Windows 10 VM,

1. Open AccessData FTK Imager.
2. Select Capture Memory and save the file as memory_dump.mem.
3. Verify completion.
   
## **Memory Analysis using Volatility**
Volatility is used to extract forensic information from the memory dump.

### **7️⃣ Running Volatility Commands**
Replace /path/to/memory/dump/ with the actual location where the memory dump is stored,
- Display running processes in a hierarchical format:
```bash

vol -f /path/to/memory/dump/memory_dump.mem windows.pstree.PsTree
```
- Check active network connections at the time of capture:
```bash

vol -f /path/to/memory/dump/memory_dump.mem windows.netstat.Netstat
```
- Search for suspicious or hidden programs (possible malware):
```bash

vol -f /path/to/memory/dump/memory_dump.mem windows.malfind.MalFind
```
- Scan memory for all running processes, including hidden ones:
```bash

vol -f /path/to/memory/dump/memory_dump.mem windows.psscan.PsScan
```
Each of these commands extracts important forensic artifacts that help in identifying suspicious activity.

## ** Memory Analysis using OleTools**
Before running OleTools, we extract memory dump details:

1. Extract files from the memory dump:
```bash

vol -f /path/to/memory/dump/memory_dump.mem windows.dumpfiles.dumpFiles > /path/to/extracted/files/dumpfiles.txt
```
2. Analyze extracted memory dump files with olevba:
```bash

olevba /path/to/extracted/files/dumpfiles.txt
```
This checks for potentially harmful embedded macros or scripts.

3. Scan the extracted data with oleid to detect suspicious objects:
```bash

oleid /path/to/extracted/files/dumpfiles.txt

```
This helps identify hidden macros, embedded scripts, or other malicious artifacts.

## **🛡️ Memory Analysis using Cyber Triage**
Cyber Triage is used to scan the captured memory dump and detect threats.

### **8️⃣ Performing Memory Analysis**
1. Open Cyber Triage.
2. Select Memory Image and choose /path/to/memory/dump/memory_dump.mem.
3. Provide necessary details such as Host display name.
4. Choose Custom Scan and start analysis.
Once the scan completes, Cyber Triage provides a detailed report highlighting potential malware, suspicious activities, and process anomalies.

## **🎯 Conclusion**
This project successfully demonstrates the process of,

- Setting up a forensic workstation using Windows 10 VM and Ubuntu (WSL).
- Installing necessary forensic tools such as Volatility, Cyber Triage, and OleTools.
- Capturing and analyzing a memory dump using industry-standard forensic techniques.
- Extracting valuable forensic artifacts that can help in identifying malware and suspicious activities.
  
By following these steps, security professionals can investigate incidents, detect intrusions, and analyze system behavior using real forensic tools.
