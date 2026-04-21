# DLL-Hijacking-Investigation
DLL Hijacking Investigation - Investigated suspicious activity involving `HandbookReader_251727.exe` and an unsigned `System.dll` associated with possible DLL hijacking and malware behavior. Documented findings, mapped activity to MITRE ATT&amp;CK, and summarized indicators of compromise.

## Note on Data Privacy
Due to the use of real client data during this investigation, screenshots and raw logs have been omitted to protect sensitive information.
All findings presented here have been generalized while preserving the technical accuracy of the investigation process.

---

## Overview
This project documents a real investigation of suspicious activity involving `HandbookReader_251727.exe` and the creation of an unsigned DLL named `System.dll`. The alert behavior suggested possible DLL hijacking or malware-related execution.

The goal of this investigation was to review the file activity, process behavior, and related detections to determine whether the activity was malicious and identify what techniques may have been used.

---

## Alert Summary
During the investigation, a suspicious executable named `HandbookReader_251727.exe` was observed creating a file named `System.dll`. The DLL was unsigned and appeared shortly after process execution. Additional related alerts later identified similar activity associated with the same executable and classified it as malware.

This sequence of behavior raised concern for possible DLL search order hijacking or the use of a malicious DLL loaded by a seemingly legitimate process.

---

## Key Observations
- `HandbookReader_251727.exe` created `System.dll`
- The DLL was **unsigned**
- The DLL was written shortly after execution of the process
- Additional alerts tied similar `HandbookReader.exe` activity to malware detections
- Suspicious memory behavior was observed, including **remote memory free** activity
- The behavior pattern was consistent with malware staging or DLL hijacking techniques

---

## Files Observed
### Suspicious DLL
- **File Name:** `System.dll`
- **SHA1:** `99b2fa21aead4b6ccd8ff2f6d3d3453a51d9c70c`
- **Verified Status:** NotSigned
- **File Size:** 11776 bytes

### Related Executable
- **Process Name:** `HandbookReader_251727.exe`

---

## Investigation Approach
The investigation focused on:
1. Reviewing the process creation event
2. Identifying file modification activity tied to the process
3. Validating whether the dropped DLL was signed or trusted
4. Correlating related alerts involving the same executable
5. Assessing whether the behavior aligned with known malware or DLL hijacking techniques

---

## Why This Was Suspicious
Several factors made this activity stand out:

- A process dropped a DLL named `System.dll`, which could be used to appear legitimate
- The DLL was not digitally signed
- The file creation occurred immediately after execution
- Follow-on detections labeled similar activity as malware
- Remote memory-related behavior can indicate code injection, process tampering, or malicious execution flow

Taken together, these behaviors increased confidence that this was not normal application activity.

---

## MITRE ATT&CK Mapping
### T1574.001 – DLL Search Order Hijacking
This technique may apply because the activity involved a suspicious DLL being placed in a way that could allow it to be loaded by an application instead of a legitimate library.

### T1055 – Process Injection
This may be relevant due to the observed remote memory free behavior, which can be associated with process injection or manipulation of another process’s memory space.

### T1105 – Ingress Tool Transfer
If the executable or DLL was introduced onto the system by an attacker or staged for follow-on execution, this technique could also be relevant depending on the broader timeline.

---

## Outcome
Based on the file creation behavior, unsigned DLL presence, suspicious memory activity, and related malware detections, this activity was assessed as **likely malicious**.

The investigation supports the conclusion that `HandbookReader_251727.exe` was involved in suspicious behavior consistent with DLL hijacking or malware delivery.

---

## Lessons Learned
- Unsigned DLL creation by an unfamiliar process should be treated as high risk
- A common system-like DLL name such as `System.dll` can be used to blend in
- Related alerts often provide valuable confirmation when malware confidence is initially unclear
- Memory-related behaviors can strengthen the case for malicious activity even when the initial alert is file-based

---

## Skills Demonstrated
- Endpoint alert investigation
- Malware triage
- IOC documentation
- Behavioral analysis
- MITRE ATT&CK mapping
- Technical report writing

---

## Future Improvements
To expand this project further, I plan to add:
- screenshots from the investigation workflow
