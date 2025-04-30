![Logo](https://media.licdn.com/dms/image/C511BAQHuhA2qldrtsQ/company-background_10000/0/1583851212514/rivan_it_training_systems_corp_cover?e=2147483647&v=beta&t=umRaaUQ29GMaFpCWJFcvAwWCPqg8znpWb1n_LjoxUGA)
# CUCM HOME LABORATORY CONFIGURATION

The CUCM Home Laboratory Configuration is designed for learning and experimenting with Cisco Unified Communications Manager (CUCM) in a home lab setting. This configuration includes CUCM servers, IP phones, Access Points, and IP Camera to simulate a real-world VoIP (Voice over IP) environment. The lab setup allows users to practice tasks such as call routing, voicemail setup, user configuration, and troubleshooting in a controlled, hands-on environment. 

##  Equipment Inclusions

**1. Cisco Unified Communications Manager:** (Cisco 1800 Series Router)

**2. Cisco IP Phones:**  (8945 models)

**3. IP Camera:** (Arecont Vision Microdome Series)

**4. Wireless Access Point:** (Cisco Airnet 2700)

## Equipments Reference

**1. Cisco IP Phones (8945 models)**



![Logo](https://www.cisco.com/c/dam/en/us/support/web/images/series/routers-1800-series-integrated-services-routers-isr-alternate3.jpg)

**2. Cisco IP Phones: (8945 models)**

![Logo](https://commswarehouse.co.uk/wp-content/uploads/2017/10/Cisco_8945_IP_GradeA-7-of-7.jpg)

**3. Cisco IP Phones (8945 models)**
![Logo](https://www.identisys.com/images/shared/product-images/microdome2.png?sfvrsn=196daf4b_2)

**4. Wireless Access Point: (Cisco Airnet 2700)**
![Logo](https://c1.neweggimages.com/ProductImage/A389_1_201708031059793619.jpg)

## Commands for Checking

| **COMMANDS**            |                                                                 |
| ----------------- | ------------------------------------------------------------------ |
| **Check the Physical Ports** | ![](https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-FcdHNq0DvB/b86de24426e9e93bc28aac0987a1660346a4dd2ec0936dd11b44f7babd3970c361b42b22bc3e0fb3794a7e4aede83c1447d7a4f1e98c55d3708cf1c9961ac95d6f78cc2653bbd9c91a556efdc1190dd8449011fc7284d87312570158f1650eb1e4e53a5f?fit=max&fm=webp&lossless=true)  |
| **sh mac-address-table** | ![](https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-3bU1F8R4au/dd69fe10ba8278ff2cab6546e0a867d5f67e0d0463c09dc5f9352842ba6d3923408d6b0a4403531db3209b618c817655dc50cafcb742829178728aa04f6139432aea6bc1f3dd4d9df03e49f8b37be7c3b76423b8c0e2ac7ad658f03da769b1212351757c?fit=max&fm=webp&lossless=true) |
| **show ip interface brief** | ![](https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-tjcHS9t1In/552e1a13a61566dace8e70c2bade3d2c0a93751c5eff29571281d66a1c76dc663ecb0806b950444eb8b5c84d2920d87a4cf4d83652c85d02a9ea27366a64250b7cef5601f7853f4820c4e36ef5ff99022a19a3170c5e77b7b453f8d0a28ad49da5f94bc4?fit=max&fm=webp&lossless=true)  |
| **show cdp neighbor-details** | ![](https://codaio.imgix.net/docs/F82M8FTNDh/blobs/bl-Ei2YHdP3MB/52080d045e54f13a460e0489f04ae2f4a6b09d51b1f284f8c47d4c4322542bbd1ccef9ae16bc90996f4b1147b7efa6274a587ebcc1ba6ee085b4976daecf9f8bba0214cd72786eed90d04e595d241111938d7649832fe58f5a54e5a9bd387463831599d5?fit=max&fm=webp&lossless=true)  |


## STEP BY STEP CONFIGURATIONS

**✅Task1: Vlan Configuration**
```bash
config t
vlan 1
name default
vlan 10
name RIVANWIFI
vlan 50
name RIVANVIDEO
vlan 100
name RIVANVOIP
exit
exit
show vlan-switch

```

**✅Task2: LAN/ethernet ports to VLAN Resigment/Transfer:**
```bash
configure terminal
interface FastEthernet0/1/1 
 switchport mode access
 switchport voice vlan 100
interface FastEthernet0/1/3 
 switchport mode access
 switchport access vlan 50
interface FastEthernet0/1/5 
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/1/7 
 switchport mode access
 switchport access vlan 1
end
show vlan-switch
```


**✅Task3: SWITCH VLAN INTERFACE:**
```bash
config t
Int Vlan 1
 no shutdown
 ip add 10.28.1.1 255.255.255.0
 description DEFAULT
Int Vlan 10
 no shutdown
 ip add 10.28.10.1 255.255.255.0
 description RIVANWIRELESS
Int Vlan 50
 no shutdown
 ip add 10.28.50.1 255.255.255.0
 description RIVANCAMS
Int Vlan 100
 no shutdown
 ip add 10.28.100.1 255.255.255.0
 description RIVANVOIP
end
show ip interface brief
```

**✅Task4: PREPARE THE DHCP SERVER:**
```bash
config t
ip dhcp Excluded-add 10.28.1.1 10.28.1.100
ip dhcp Excluded-add 10.28.10.1 10.28.10.100
ip dhcp Excluded-add 10.28.50.1 10.28.50.100
ip dhcp Excluded-add 10.28.100.1 10.28.100.100
ip dhcp pool DEFAULT
   network 10.28.1.0 255.255.255.0
   default-router 10.28.1.1
   domain-name DEFAULT.COM
   dns-server 10.28.1.10
ip dhcp pool RIVANWIFI
   network 10.28.10.0 255.255.255.0
   default-router 10.28.10.1
   domain-name RIVANWIFI.COM
   dns-server 10.28.1.10
ip dhcp pool RIVANCAMS
   network 10.28.50.0 255.255.255.0
   default-router 10.28.50.1
   domain-name RIVANCAMS.COM
   dns-server 10.28.1.10
ip dhcp pool RIVANVOIP
   network 10.28.100.0 255.255.255.0
   default-router 10.28.100.1
   domain-name RIVANVOIP.COM
   dns-server 10.28.1.10
   option 150 ip 10.28.100.1   
   END
```

**✅Task5: IP CAMERA CONFIGURATION:**
```bash
config t
ip dhcp pool SECURITYCAMERA
 host 10.28.50.8 255.255.255.0
 client-identifier 001a.070b.4735
 default-router 10.28.50.1
```

**✅Task6: CALL CENTER SETUP:**
```bash
config t   
no telephony-service
telephony-service
   no auto assign
   no auto-reg-ephone
   max-ephones 5
   max-dn 20
   ip source-address 10.28.100.1 port 2000
   create cnf-files
ephone-dn 1
  number 2811
ephone-dn 2
  number 2822
ephone-dn 3
  number 2833
ephone-dn 4
  number 2844
ephone-dn 5
  number 2855
ephone-dn 6
  number 2866
ephone-dn 7
  number 2877
ephone-dn 8
  number 2888
 ephone-dn 9
   number 2899
ephone-dn 10
 number 2898
Ephone 1
  Mac-address 6899.cda1.c909
  type 8945
  button 1:8 2:7 3:6 4:5
  restart
end
```

**✅Task7: AUTOMATIC VOICE ANSWER:**
```bash
config t
dial-peer voice 69 voip
 service rivanaa out-bound
 destination-pattern 2869
 session target ipv4:10.28.100.1
 incoming called-number 2869
 dtmf-relay h245-alphanumeric
 codec g711ulaw
 no vad
!
telephony-service
 moh "flash:/en_bacd_music_on_hold.au"
!
application
 service rivanaa flash:app-b-acd-aa-3.0.0.2.tcl
  paramspace english index 1        
  param number-of-hunt-grps 2
  param dial-by-extension-option 8
  param handoff-string rivanaa
  param welcome-prompt flash:en_bacd_welcome.au
  paramspace english language en
  param call-retry-timer 15
  param service-name rivanqueue
  paramspace english location flash:
  param second-greeting-time 60
  param max-time-vm-retry 2
  param voice-mail 1234
  param max-time-call-retry 700
  param aa-pilot 2869
 service rivanqueue flash:app-b-acd-3.0.0.2.tcl
  param queue-len 15
  param aa-hunt1 2800
  param aa-hunt2 2877
  param aa-hunt3 2801
  param aa-hunt4 2833
  param queue-manager-debugs 1
  param number-of-hunt-grps 4
```


## ❗HOW TO FIX THE VOICE ANSWERING 

```bash
config t
 application
  no service callqueue flash:app-b-acd-2.1.2.2.tcl
  no service rivanaa flash:app-b-acd-aa-2.1.2.2.tcl
```
 

