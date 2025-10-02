# MakeCfgFile – Polycom Config Generator

This PowerShell script (Make-PolycomCfg.ps1) automates creating Polycom SoundPoint IP phone configuration files from a CSV input.

⸻

## What it does
	•	Reads a CSV containing phone extension and credential details.
	•	For each row, generates a <MAC>_phone.cfg XML file in the correct Polycom SoundPoint format.
	•	Each file includes:
	•	SIP registration info (extension, auth ID, auth password, server/port/transport)
	•	Voicemail callback settings
	•	SNTP time settings
	•	Optionally writes a 000000000000.cfg master config that tells each phone to fetch its own <MAC>_phone.cfg.

These files can then be placed in the 3CX provisioning folder (or any HTTP/HTTPS server) so phones auto-provision on boot.

⸻

## CSV format

The CSV must include these column headers:

MAC,EXT,AUTH_ID,AUTH_PASSWORD,LABEL,SERVER,PORT,TRANSPORT

	•	MAC: Phone’s MAC address (12 hex digits, no separators)
	•	EXT: Extension number (e.g., 220)
	•	AUTH_ID: SIP Authentication ID (from 3CX extension settings)
	•	AUTH_PASSWORD: SIP Authentication Password (from 3CX extension settings)
	•	LABEL: Display label shown on the phone screen
	•	SERVER: Registrar FQDN or IP (e.g., deez.3cx.com.au)
	•	PORT: SIP port (usually 5060)
	•	TRANSPORT: SIP transport (usually UDP; can be TCP or TLS if needed)

Note: If a label contains spaces, wrap it in quotes. Example: "Staff Room".

⸻

### Example CSV (csvExample.csv)
	•	MAC,EXT,AUTH_ID,AUTH_PASSWORD,LABEL,SERVER,PORT,TRANSPORT
	•	0004F24EC2C4,220,220,SeCr3tP@ss,"James Smith",deez.3cx.com.au,5060,UDP
	•	0004F24F6F0F,221,221,AnotherPass,"John Sykes",deez.3cx.com.au,5060,UDP


⸻

## Usage

	•	.\Make-PolycomCfg.ps1 -CsvPath "C:\path\to\csvExample.csv" `
  	•	-OutDir "C:\ProgramData\3CX\Instance1\Data\Http\Interface\provisioning\<token>" 
  	•	-WriteMaster

Parameters

	•	-CsvPath – Path to your input CSV
	•	-OutDir – Destination folder for generated config files
	•	-WriteMaster – Also creates the master 000000000000.cfg file

After running, each phone will have a matching <MAC>_phone.cfg file ready to serve.

⸻

## Using with non-Polycom phones

This script outputs Polycom SoundPoint XML.
If you need configs for other brands (e.g., Yealink, Fanvil, Snom):
	1.	Export/download a working .cfg from an existing phone of that brand.
	2.	Use its syntax/tags as the reference.
	3.	Modify the New-PolycomPhoneCfg function in the script to emit that format instead.

This lets you keep the same CSV structure while generating vendor-specific configuration files.
