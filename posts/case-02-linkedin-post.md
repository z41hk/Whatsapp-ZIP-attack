🚨 A fake PDF on WhatsApp just installed a full remote-management agent on my sandbox PC. No classic "virus" needed.

I analysed a ZIP sent on WhatsApp with a Malay message: "Tolong buka komputer dan semak ya" (please open on a computer and check). Here is the full chain, A to Z. 👇

1️⃣ The lure
PDF_2K1RQW_20261023110727.zip (574 KB). The "open on a computer" line pushes you to a Windows PC, since phones can't run EXEs.

2️⃣ The VHD trick
Inside the ZIP is a .vhd virtual disk (8 MB). Double-click it and Windows mounts it as a new drive. Files that come out of a mounted disk often skip the usual "downloaded from the internet" warnings.

3️⃣ The payload
The mounted drive holds two files:
📄 PDF_2K1RQW_...exe, with a PDF-style name
📦 CRclIENt.DLL, which is DLL sideloading. The EXE loads the malicious DLL beside it. ANY.RUN flagged "loading modules from mounted disk drive" and an XOR-obfuscated URL (YARA).

4️⃣ Command and control
The decrypted config shows two URLs on a lookalike "download" domain: a beacon.php check-in and a .dat file for the next stage.

5️⃣ Silent installation
The malware uses Windows' own tar.exe to unpack w.zip into C:\ProgramData\Office89bd\, then runs msiexec in quiet mode to install UEMSAgent.msi.

That is the ManageEngine UEMS agent, a legitimate IT-admin tool. Because it is real, signed software, antivirus is far less likely to complain. 😬

6️⃣ What the agent then did on the PC
• Raised privileges from medium to High/System
• Installed system certificates (lsass and msiexec modified them)
• Registered as a Windows service
• Ran systeminfo and software inventory
• Enumerated USB devices and software usage
• Scanned for patches
• Dropped drivers and installed EDR-style components
• Forced a Group Policy refresh
• Flagged for screenshot capability (YARA)

Result: 367 processes, 26 malicious, 34 suspicious. Tags: rmm-tool, remote, fingerprinting.

7️⃣ Why this is scary
This is "living off the land" (abusing trusted software). The attacker gets remote control, inventory, and visibility over the PC, and could push more malware, steal data, or reuse a logged-in WhatsApp Web session to send the same lure to contacts.

🛡️ Defend
✅ Never open ZIP/VHD/EXE files from chat, even from known contacts. Call to confirm.
✅ A real PDF is never an .exe. Turn on file extensions.
✅ Block .vhd/.iso mounting and unknown MSI installs via policy.
✅ Alert on unexpected ManageEngine/RMM agents. If you don't use them, they are a red flag.
✅ If you ran it: disconnect the network, log out of WhatsApp Web, call IT/security.

IOCs are in the first comment. Please share this with staff who handle invoices and PDFs. 🔐

#CyberSecurity #Malware #DLLSideloading #RMM #WhatsApp #PhishingAwareness #ThreatIntelligence #ANYRUN #InfoSec #BlueTeam #Malaysia
