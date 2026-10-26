🚨 A "Statement of Account" on WhatsApp is now a malware delivery method.

I ran a dynamic analysis in a sandbox on a ZIP forwarded on WhatsApp to finance and admin staff.

The lure: Reconciliation_Working_Papers_20261026.zip, asking you to forward the statement to your Finance Manager and "open it on a computer". Phones can't run Windows EXEs, so the attacker steers you to a PC.

Inside: a fake "invoice" EXE (note the odd spaces in the name) and CrashHunter_PC3.dll.

What it does:
1️⃣ The EXE loads the malicious DLL beside it (DLL sideloading).
2️⃣ Tagged swmloader / loader / fingerprinting, scored 100/100 malicious.
3️⃣ It relaunches itself through a COM dllhost.exe process and runs a second copy from AppData\Local.
4️⃣ YARA flagged an XOR-obfuscated URL.

Why WhatsApp makes it worse: a logged-in WhatsApp Web session can be abused to send the same lure to contacts, from someone they trust.

🛡️ Protect yourself
✅ Never open ZIP/EXE files from chat, even from known contacts. Call to confirm.
✅ "Open it on a computer" plus a ZIP is a red flag.
✅ Turn on file extensions. An invoice ending in .exe is never an invoice.
✅ Block executables running from Downloads and AppData.
✅ If you ran it: disconnect the network, log out of WhatsApp Web, call IT/security.

#CyberSecurity #Malware #DLLSideloading #WhatsApp #PhishingAwareness #ThreatIntelligence #ANYRUN #InfoSec #BlueTeam
