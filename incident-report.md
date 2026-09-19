# Incident Report

## Executive Summary

The investigation identified multiple correlated indicators of suspicious activity across email, authentication, endpoint, file, and network telemetry.

Kevin opened a macro-enabled Word attachment shortly after his account logged in from a previously unseen IP address using his registered MFA device. EDR telemetry showed `WINWORD.EXE` creating `sync.ps1`, spawning `cmd.exe`, and leading to PowerShell launching 7-Zip and curl.

7-Zip created `finance_backup.zip` from files matching `C:\Finance\*.xlsx`. Curl was then launched with a command instructing it to upload that archive to `198.51.100.72`. Network telemetry recorded 913,204 outbound bytes, matching the recorded size of the archive.

Based on the combined evidence, the activity was assessed as a **probable compromise** with suspected data exfiltration.

---

## Assessment

- **Classification:** Probable compromise
- **Severity:** High
- **Confidence:** High
- **Priority:** High
- **Escalation Decision:** Escalate to Incident Response

Multiple correlated events across several telemetry sources form a coherent suspicious activity chain. Confirmed malware execution is not required to assess the activity as a probable compromise.

---

## Key Findings

### Authentication

Kevin's account recorded two failed login attempts followed by a successful login from a previously unseen IP address, `203.0.113.44`.

MFA was approved from Kevin's registered iPhone. This weakens a simple stolen-password explanation, but does not independently establish that the login was legitimate or explain the later endpoint activity.

### Email Activity

Kevin received and opened `Supplier_Details.docm`, a macro-enabled Word document.

The sender passed SPF and DKIM validation, while the sender domain had unknown reputation. Passing email authentication checks does not by itself establish that the message was benign.

### Endpoint and File Activity

EDR telemetry recorded the following process sequence:

`OUTLOOK.EXE → WINWORD.EXE → cmd.exe → powershell.exe`

`WINWORD.EXE` created `sync.ps1` in Kevin's temporary directory.

PowerShell later launched `7z.exe` with a command targeting:

`C:\Finance\*.xlsx`

File telemetry confirmed the creation of:

`finance_backup.zip`

with a recorded size of:

`913,204 bytes`

### Network Activity

PowerShell launched `curl.exe` with a command instructing it to upload `finance_backup.zip` to:

`198.51.100.72`

Network telemetry recorded:

- **Destination:** `198.51.100.72:443`
- **Outbound bytes:** 913,204
- **Inbound bytes:** 842

The outbound byte count exactly matched the recorded archive size, providing strong evidence that the archive data was sent outbound.

---

## Confirmed

- Kevin's account recorded two failed logins followed by a successful login from a previously unseen IP address.
- MFA was approved from Kevin's registered iPhone.
- Kevin opened `Supplier_Details.docm`.
- `WINWORD.EXE` created `sync.ps1`.
- `WINWORD.EXE` spawned `cmd.exe`.
- `cmd.exe` spawned PowerShell.
- PowerShell launched `7z.exe`.
- `7z.exe` was instructed to archive files matching `C:\Finance\*.xlsx`.
- `finance_backup.zip` was created with a recorded size of 913,204 bytes.
- PowerShell launched `curl.exe` with a command instructing it to upload the archive.
- Network telemetry recorded 913,204 outbound bytes and 842 inbound bytes.

---

## Inferred

- The activity sequence is consistent with a phishing-triggered compromise.
- The activity is consistent with collection and likely exfiltration of Finance spreadsheet data.

---

## Not Proven

- The identity of the person responsible.
- The intent of the person responsible.
- Whether every targeted `.xlsx` file was successfully included in the archive.
- The exact contents of `finance_backup.zip` without independently inspecting it.
- Whether the remote server successfully received and retained the complete archive.
- The contents of the 842 inbound bytes.
- Whether additional persistence or follow-on activity occurred.

---

## Recommended Actions

1. Isolate `KEVIN-LAPTOP` from the network while preserving the endpoint for investigation.
2. Escalate for deeper endpoint analysis, including inspection of `sync.ps1`, PowerShell activity, persistence mechanisms, and post-transfer activity.
3. Review Kevin's account sessions, authentication history, and MFA activity, and reset credentials if compromise is confirmed or strongly suspected.
4. Block or monitor the observed infrastructure and search the environment for related activity involving the external IP, sender domain, attachment, and similar messages.

---

## Next Investigation Steps

1. Inspect and analyze `sync.ps1`.
2. Investigate the sender and email domain in greater depth.
3. Review endpoint and network activity occurring after `curl.exe`.
4. Determine what data `curl.exe` transmitted and whether the remote host acknowledged or retained it.
5. Search for similar emails, attachments, scripts, and process chains across other endpoints.

---

## Final Analyst Conclusion

The investigation identified multiple correlated indicators of suspicious activity across email, endpoint, file, authentication, and network telemetry.

The strongest evidence included `WINWORD.EXE` creating `sync.ps1`, spawning `cmd.exe`, and leading to PowerShell launching 7-Zip to create `finance_backup.zip`, followed by `curl.exe` being instructed to upload the archive to an external IP address.

Network telemetry recorded 913,204 outbound bytes, matching the recorded archive size.

Based on the combined evidence, the activity was assessed as a **probable compromise** and should be escalated to Incident Response for containment and further investigation.
