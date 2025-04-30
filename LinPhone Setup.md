
![Logo](https://media.licdn.com/dms/image/C511BAQHuhA2qldrtsQ/company-background_10000/0/1583851212514/rivan_it_training_systems_corp_cover?e=2147483647&v=beta&t=umRaaUQ29GMaFpCWJFcvAwWCPqg8znpWb1n_LjoxUGA)


# LinPhone Configuration

Linphone is a free, open-source Voice over IP (VoIP) application that enables voice and video calls using the SIP (Session Initiation Protocol). When calling an IP phone with Linphone, it connects to the target device using its SIP address or IP address. Linphone sends signaling messages to establish the call, and once accepted, the audio stream is transmitted over the internet using codecs like Opus or G.711. 


## Requirements

- **LinPhone** <img src=https://play-lh.googleusercontent.com/ugOoXPn9nK7yXKv96b2iAqIlJ0IupVmzr2_KG6CMRM_M_5FfalKa_7NG7Qit4Mkg7RDN width="25">
- **Visual Studio** <img src=https://iconape.com/wp-content/png_logo_vector/visual-studio-code.png width="25">


## ⚙️Lab Environment Setup

1. **Connect the hardware**: Refer to the printed setup guide included in your kit or view the digital [Setup Guide PDF](Enter.pdf).
2. **Access router CLI** via console cable using:
   - PuTTY(Free) or SecureCRT(License)
3. **Power on all devices**, verify LED indicators, and begin configuration.



## ✅Step by Step 
## Task 1: Access console using SecureCRT
Open SecureCRT and click the quick connect button 
<br>
<img src=https://github.com/user-attachments/assets/b9db7d7d-1623-4759-b294-cb7664901495 width="300">
<br>
Select `Serial` as Protocol
<br>
<img src=https://github.com/user-attachments/assets/64ec6f89-c5af-462a-a041-e8106efa2cfe width="300">
<br>
Select the `USB-SERIAL` and `9600` Baud rate
<br>
![image](https://github.com/user-attachments/assets/8813ba8d-b855-46e1-b7c8-7121ee029112)

## Task 2 - Access WIFI using Python
Automate your wireless access point setup by using a Python script to configure SSID, password, and VLAN tagging through `netmiko`.


Install netmiko module
```bash
py -m pip install netmiko
```
clone this repository to access the prebuilt python codes 
```git
git clone https://github.com/yyynnot/Take-Home-Lab.git
```
Open the VS Code and open the folder 
edit the `autoAP-jsn.json` Replace hostname, ssid, and wifi-pass. <br>
![{FA25CC70-1A82-4F84-97B4-4D75D0EC9C15}](https://github.com/user-attachments/assets/3cae10a3-fd5a-4785-a20f-e5733bae147b)<br>
After editing run `autowifi-jsn.py` <br>
<img src=https://github.com/user-attachments/assets/fd285666-da87-446f-b9b9-faf7b35cac4e width=750><br>
Terminal should be like this <br>
![{19290986-AC25-47B9-BB95-AAE0CF3A7C47}](https://github.com/user-attachments/assets/cc419e22-1f70-497f-9321-c23c7099379a) <br>
Connect on the WIFI. <br>
<img src="https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-noZ2ZVkibg/a8aff252a9ea4cecf1ce133b1c307d5f6d2436f4cb411660a23ad89fa20c8f41f01dac678802914ffb50c4962f9ae4fa9c4216028a0d8844cad449fa7516dcee4d6099c3abbfab5fec9c48fe59da222859eaec14bc2472c203a21bee6ebdb3fa1d4ec4d5?fit=max&fm=webp&lossless=true" width="500"><br>

## Task 3 - Mac Address and User Credentials Setup
Open settings in your phone and find your Mac Address<br>
<img src="https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-tly3tE3Xdq/d7e10b3943c22fcef7a0c0593bb0047d69c58bc0f64a985ba0104b11dcce618aba303acbc0282a831f91371035ff831636496c937cc703b3b30ea6ce1e3c093c804c728f6c5085d4e5563d1c784aa5e1260ca1ca240c7c057e4df499b3c0e5baa498ab1d?fit=max&fm=webp&lossless=true" width="500"><br>

SIP CONFIGURATION FOR CUCM
```bash
conf t
 voice service voip
  allow-connections h323 to sip
          
  allow-connections sip to h323
  allow-connections sip to sip
  supplementary-service h450.12
 sip
   bind control source-interface fa0/0
   bind media source-interface fa0/0
   registrar server expires max 600 min 60
!
 voice register global
  mode cme
  source-address 10.28.1.1 port 5060
  max-dn 12
  max-pool 12
  authenticate register
  create profile sync
 voice register dn 1
   number 2823
   allow watch
   name 2823
 voice register dn 2
   number 2869
   allow watch
   name 2869
!
  voice register pool 1
    id mac __.__.__.__
    number 1 dn 1
    dtmf-relay sip-notify
    username 2823 password 2823
    codec g711ulaw
!
  voice register pool 2
    id mac  __.__.__.__
    number 1 dn 2
    dtmf-relay sip-notify
    username 2869 password 2869
    codec g711ulaw
!
```



## Task 4 - LinPhone Connection 
Download Linphone on Google Playstore or App Store<br>
<img src="https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-s_U3b8D2bI/c2797d3e6655e8ffd9b475ee66dbfd2375f266bc04745191d1c361f5b2e5047799e79877dc6997e47ada4fb41a4385febf0f41006de2a207014136cb4b9c35338d4856701863df37b69eec79af101799868e4fb95bf755b5428cd15d3461885ade48a36e?fit=max&fm=webp&lossless=true" width="500"><br>


Click the `use SIP account`<br>
<img src="https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-hpwN1tZOmh/c5e6b7fe8af0c87ecc3de438db44cfd6599045370a581d957306f26183b450683fc6ffab7b02ab315351ad058142001b7feff6f39769efeadda6d1d5a9426fd0664b691b79e8ec829b882ec985ea08e7843af9f47ad7fb76e9a88c2ad545f3b15cf728c2?fit=max&fm=webp&lossless=true" width="500"><br>

Enter your Voice Register pool credentials<br>
<br>Username `2823`
<br>Password `2823`<br>
<img src="https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-9Er9u3AMey/dc7d70ce9e00f7302093dbf513680200a48ce2f540f2e892e79e59623312a27a13303ac25811187af888b186c10a45a5571d77813b3aae773e671494fe5ae50f3b8289bc73fa8cbf94647bde5097532dcab4d6f470d0e728bd1bdaa9174f5e7ccd2fb7c4?fit=max&fm=webp&lossless=true" width="500"><br>


