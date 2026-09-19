# Incident Timeline

This timeline summarizes the key authentication, endpoint, file, and network events observed during the investigation.

| Time | Event |
|---|---|
| 09:14:03 | Kevin's account recorded a failed login from `203.0.113.44`. |
| 09:14:19 | Kevin's account recorded a second failed login from `203.0.113.44`. |
| 09:14:39 | MFA was approved from Kevin's registered iPhone. |
| 09:14:41 | Kevin's account successfully logged in from `203.0.113.44`. |
| 09:16:00 | Kevin opened the attachment `Supplier_Details.docm`. |
| 09:16:18 | `OUTLOOK.EXE` spawned `WINWORD.EXE` to open the attachment. |
| 09:16:29 | `WINWORD.EXE` created `sync.ps1` in Kevin's Temp directory. |
| 09:16:33 | `WINWORD.EXE` spawned `cmd.exe`. |
| 09:16:34 | `cmd.exe` spawned `powershell.exe` using `ExecutionPolicy Bypass`. |
| 09:17:02 | PowerShell spawned `7z.exe` with a command targeting `C:\Finance\*.xlsx`. |
| 09:17:10 | `7z.exe` created `finance_backup.zip`, recorded at 913,204 bytes. |
| 09:17:31 | PowerShell spawned `curl.exe` with a command instructing it to upload `finance_backup.zip`. |
| 09:17:32 | Network telemetry recorded `curl.exe` connecting to `198.51.100.72:443`, with 913,204 bytes outbound and 842 bytes inbound. |
