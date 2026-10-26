# WhatsApp ZIP malware analysis

Dynamic analysis (ANY.RUN sandbox) of two malicious ZIP files delivered over WhatsApp to finance and admin staff. Both push the victim to "open it on a computer", because phones cannot run Windows executables.

> Defensive research and awareness only. No malware samples are stored in this repo. Indicators are defanged.

| Case | Lure | Technique | Verdict |
|------|------|-----------|---------|
| [01](case-01-swmloader/) | `Reconciliation_Working_Papers_20261026.zip` ("Statement of Account") | EXE + DLL sideloading, swmloader, COM `dllhost` relaunch | 100/100 malicious |
| [02](case-02-fake-pdf-manageengine/) | `PDF_2K1RQW_20261023110727.zip` (Malay: "Tolong buka komputer dan semak ya") | VHD mount, DLL sideloading, silent install of ManageEngine UEMS agent (RMM abuse) | Malicious activity, 26 malicious / 34 suspicious processes |

## Contents
- `case-01-swmloader/` and `case-02-fake-pdf-manageengine/`: write-up and screenshots
- `iocs/`: indicators (CSV) and report links
- `posts/`: the LinkedIn posts
- `docs/index.html`: ready-to-post page with images (enable GitHub Pages on `/docs` to host it)

## Common pattern
1. A forwarded WhatsApp message with a ZIP and an urgent "open on a computer" request.
2. A fake document name (`Inv ...exe`, `PDF_...exe`) next to a malicious DLL.
3. DLL sideloading loads the DLL when the EXE is run.
4. Obfuscated (XOR) C2 URL, then a second stage or a legitimate remote-management tool.
5. Possible spread: a logged-in WhatsApp Web session could be abused to send the same lure to contacts. This is a risk, not something the sandbox proved.

## Detection and hunting ideas
- EXE launched from a mounted `.vhd`/`.iso` drive or an extracted ZIP folder that loads an unsigned DLL from its own folder.
- `dllhost.exe /Processid:{...}` spawning a user-writable-path EXE with `--evhs=`.
- `tar.exe -xf ...\ProgramData\...zip` followed by a quiet `msiexec /i ...msi`.
- Unexpected ManageEngine UEMS agent (`dcagentservice.exe`, `uesAgentService.exe`, `meaap.exe`) on hosts that do not use ManageEngine.
- Policy: block mounting of `.vhd`/`.iso` for standard users, enforce AppLocker/WDAC on user-writable paths, show file extensions.

## If you ran it
Disconnect from the network, log out of WhatsApp Web, change passwords from a clean device, and contact IT/security.

## Reports
- Case 02: https://any.run/report/8dc7d72c69920332e171fe87057ccc7c8cc23cecec850c36ea56f94f1e189214/cbf53acf-8692-4605-a6aa-ad0603e7c60b
- Case 01: add your ANY.RUN links in `iocs/reports.txt`

## Disclaimer
ManageEngine is legitimate software. Case 02 describes an attacker abusing it, not a flaw in the vendor's product.
