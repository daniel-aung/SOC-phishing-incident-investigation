# Investigation Indicators

The following indicators were observed during the investigation. These are relevant to the case, but not all should be treated as confirmed malicious Indicators of Compromise (IoCs) without further validation.

- **IP Address:** `198.51.100.72`
- **Email Domain:** `vendor-payments.example`
- **Attachment:** `Supplier_Details.docm`
- **Script:** `sync.ps1`
- **Archive:** `finance_backup.zip`
- **Suspicious Process Chain:** `OUTLOOK.EXE → WINWORD.EXE → cmd.exe → powershell.exe → 7z.exe / curl.exe`

## Notes

- The external IP address and sender domain had **unknown reputation** in the available threat-intelligence data.
- Legitimate tools such as PowerShell, 7-Zip, and curl are not inherently malicious, but their use in this process chain was suspicious in context.
- The process lineage and timing made these indicators relevant to the investigation.
