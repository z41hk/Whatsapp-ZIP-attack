# Case 02: Fake PDF ZIP that installs a ManageEngine agent

![WhatsApp lure](images/01-whatsapp-lure.png)

**Lure:** `PDF_2K1RQW_20261023110727.zip` (574 KB) with the Malay message "Tolong buka komputer dan semak ya" (please open on a computer and check).

## Chain
1. **VHD:** the ZIP holds a `.vhd` (~8 MB). Double-clicking mounts it as a drive.
   ![VHD](images/02-vhd-in-zip.png)
2. **Payload:** the drive holds `PDF_2K1RQW_...exe` and `CRclIENt.DLL` (32 KB), loaded by sideloading. ANY.RUN: "loading modules from mounted disk drive", XOR-obfuscated URL (YARA).
   ![VHD contents](images/03-vhd-contents.png)
3. **C2 config (decrypted):** `hxxps://www[.]fafadownload[.]com/beacon[.]php` and `hxxps://ccc[.]fafadownload[.]com/plapdla[.]dat`.
4. **Silent install:** `tar.exe -xf C:\ProgramData\Office89bd\w.zip`, then `msiexec /i ...UEMSAgent.msi` (quiet). This is the ManageEngine UEMS agent, a legitimate RMM tool.
   ![tar and msiexec](images/04-tar-msiexec-process-tree.png)
5. **Agent activity:** privilege elevation (medium to High/System), certificate changes (lsass, msiexec), Windows services, `systeminfo`, software inventory, USB enumeration, software metering, patch scan, driver and EDR-style components, `gpupdate`, YARA screenshot capability.
   ![Inventory](images/05-systeminfo-inventory.png)
   ![USB and config](images/06-usb-config-modules.png)
   ![Agent modules](images/07-agent-modules.png)

**Stats:** 367 processes, 204 monitored, 26 malicious, 34 suspicious. Tags: `fingerprinting`, `manageengine`, `rmm-tool`, `remote`, `arch-exec`.

The report does not show which server the agent registered to, so remote control is the risk, not a confirmed observation.
