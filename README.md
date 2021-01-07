# Packet capture on ASA & ASR routers
These playbooks are build to help GNOC and SAP GMP L4 team to do the packet capture on ASA firewalls and ASR routers. This set of playbooks also have the capability to download the pcap file from devices to the controller system. 
The structure of this sub repository is very simple. There are two playbooks for each device type, the first one starts the capture and the second one removes the capture and downloads the pcap file on the controller system. 

>## **Repository rules and guidelines**
- **Prerequisite**
  - Gather the Interface name, source IP, destination IP, protocol, source port and destination port detail before you run the playbook.
  - In case you want to enable asp_drop pcap on asa, keep the asp_drop type name handy with you along with all other details mentioned above. It is not possible to provide all the asp-drop type in user prompt due to its huge number.  
- **Plyabooks**
  - ***ios_pcap_start.yml*** - Start the capture on the ios-xe device. User needs to provide details mentioned in the prerequisite section through the user prompt. Kindly note down the pcap name which you give on prompt, the same pcap need to be provided in the second playbook to remove the capture and download the pcap file.
  - ***ios_pcap_stop.yml*** - Remove the capture which was started by the playbook ios_pcap_start.yml and download the pcap file on controller system


  - ***asa_pcap_start.yml*** - Start the capture on the asa device. User needs to provide details mentioned in the prerequisite section through the user prompt.Kindly note down the pcap name which you give on prompt, the same pcap need to be provided in the second playbook to remove the capture and download the pcap file 
  - ***asa_pcap_stop.yml*** - Remove the capture which was started by the playbook asa_pcap_start.yml and download the pcap file on controller system

  - ***asa_context_pcap_start.yml*** - Start the capture on the asa context device. User needs to provide details mentioned in the prerequisite section through the user prompt.Kindly note down the pcap name which you give on prompt, the same pcap need to be provided in the second playbook to remove the capture and download the pcap file 
  - ***asa_context_pcap_stop.yml*** - Remove the capture which was started by the playbook asa_context_pcap_start.yml and download the pcap file on controller system

- **Inventory File**
  - This is the only file which the user needs to maintain. Add the ip address of the device on which you want to do pcap, in the respective section of the file.
  - For context mode ASA, provide the context name (on which pcap needs to be enabled, example: GMP-INTERNET-A) and ip address of the system context (on which context is active, example: 10.192.6.10 ) in the  inventory file. 

```
     [ios]
     10.173.254.141

     [asa]
     10.8.245.70

     [asa_context]
     GMP-INTERNET-A ansible_ssh_host=10.192.6.105
```

- **Limitation**
  - Not all packet capture options are avaliable. Playbook will enable commonly used pcaps. 

- **User Prompt Example**

```
  ## PCAP Start ##
  02ZC0D3LVDM:GMP i348672$ ansible-playbook -i inventory asa_pcap_start.yml
  Username: i348672
  Password: 
  Capture_Name: test123
  Buffer_Size (1534 to 33554432 bytes OR empty): 2000
  Type 'yes' if you want to capture only header or else hit enter: yes
  Packet_Lenght (14 to 9216 Bytes OR empty): 
  '***Warring: use with caution*** Provide asp-drop type ( 'all' is frequently used type)' OR 'hit 'enter' key for the normal packet capture': 
  Interface Name (Not required if you selected the asp drop capture): INSIDE_1092
  Protocol (ip/tcp/udp ..etc OR protocol number 0 to 255): tcp
  Source_IP (Example: 192.168.1.0 255.255.255.0 OR host 192.168.1.1 OR any): any
  Destination_IP (Example: 192.168.1.0 255.255.255.0 OR host 192.168.1.1 OR any): any
  Port Number Example: eq <0-65535> OR range <0-65535> <0-65535> (Note: Press enter if you want to capture all ports OR if you selected protocol type 'IP'): 
  
  
  
  
  ## PCAP STOP ##
  C02ZC0D3LVDM:GMP i348672$ ansible-playbook -i inventory asa_pcap_stop.yml
  Username: i348672
  Password: 
  Capture_Name: test123
``` 
