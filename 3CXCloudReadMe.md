# Temporary macOS HTTP Server - Unsupported Phone Provisioning

This guide explains how to quickly host `.cfg` files from your Mac using Python’s built-in web server — ideal for short-term phone provisioning (e.g. Yealink, Polycom, Grandstream).

---

## Step 1 - Create a folder and provision your cfg files

In Documents/Desktop, wherever you feel like having the folder, create one called "provisioning". I named mine different so in screenshots it will look different.
Create a phones.csv file with the format below

### Example CSV (phones.csv)
	  •	MAC,EXT,AUTH_ID,AUTH_PASSWORD,LABEL,SERVER,PORT,TRANSPORT
	  •	0004F24EC2C4,220,220,SeCr3tP@ss,"James Smith",deez.3cx.com.au,5060,UDP
	  •	0004F24F6F0F,221,221,AnotherPass,"John Sykes",deez.3cx.com.au,5060,UDP

Open Powershell and run

    •	.\Make-PolycomCfg.ps1 -WriteMaster
    • -CsvPath "C:\path\to\phones.csv" `
  	•	-OutDir "/Users/james.hallifax/Documents/provisioning/" 

This will populate your provisioning folder with the cfg files as below. 
-WriteMaster will write the 000000000000.cfg file that you require as a base if not using self hosted 3CX. If you are using self hosted method, don't use -WriteMaster.

<img width="420" height="290" alt="image" src="https://github.com/user-attachments/assets/7d35f3f4-7d21-402b-bb20-ab05c1ef9ac9" />

## Step 3 - Start HTTP Server

Open Terminal

	  •	cd Documents/provisioning
	  •	python3 -m http.server 8000 --bind 0.0.0.0

That folder will now be reachable with your device IP

<img width="373" height="362" alt="image" src="https://github.com/user-attachments/assets/7fcb1b83-e155-439d-9a6e-abd19232dfc0" />

## Step 4 - Add provisioning url to phone

As normal, open the phone web gui. If you haven't reset the phone, ensure you reset it.
Go to Settings > Provisioning Server and set the method to HTTP.
Set the url to

	  •	http://<your-ip>:8000

<img width="252" height="225" alt="image" src="https://github.com/user-attachments/assets/de484d39-b1fe-4b07-80ba-b7382ed3bec6" />


Save and reboot the phone. As long as the voice vlan can connect to your device vlan, it will grab the cfg file and provision.
