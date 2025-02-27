# Wireshark Analysis 

## Objective
This project focuses on leveraging Wireshark for network packet analysis. By capturing and analyzing network traffic, I identified anomalies and potential security threats using targeted Wireshark filters. 

### Skills Learned

- Use Wireshark to capture and analyze network packets for anomalies
- Use Wireshark filters to isolate malicious activities
- Apply MITRE ATT&CK tactics to map adversary techniques in network traffic

### Tools Used

- Wireshark - CiscoTalos Intelligence - VirusTotal - Google

## Investigate the Statistics of the PCAP

Capture File Details

![Screenshot 2025-02-20 151147](https://github.com/user-attachments/assets/c86f219a-2b0f-48cc-9587-bba2e956af1d)
![Screenshot 2025-02-20 151156](https://github.com/user-attachments/assets/7a040c91-2234-4cbc-9293-6e6b7694c292)

- Around 3 hours of PCAP with 55207 packets

Conversations

![Screenshot 2025-02-20 151337](https://github.com/user-attachments/assets/c78e6dba-be57-435d-8e74-c09c924911a3)

- At this point, I would think that 10.0.0.149 is our infected endpoint and 10.0.0.6 endpoint is our domain controller

- I'll make a note of all the external IP addresses commonly used

Protocol Hierarchy

![Screenshot 2025-02-20 151535](https://github.com/user-attachments/assets/0e0fc11f-f0f6-4399-8956-cfdc44fe4937)

- Make a note of protocol used (ARP & SMB)

- I want to take a look at the 4 HTTP traffic packets that we have (cleartext)

![Screenshot 2025-02-20 162951](https://github.com/user-attachments/assets/f66bb618-6c2f-46ac-a5de-02b6552347b8)

- Follow HTTP stream of the first packet because the DAT file looks interesting
  
![Screenshot 2025-02-20 151850](https://github.com/user-attachments/assets/3175ae09-5a8b-4fd1-92e3-f06fb0a8b562)

1. User-Agent string being "curl" is suspicious as it means they've executed some sort of script
2. "MZ" is the file signature for MZ DOS FILE 
3. Since the infected endpoint now have the 86607 DAT FILE, I will export it and get the file hash to do a reputation check

![Screenshot 2025-02-20 163253](https://github.com/user-attachments/assets/b43f608e-ef7f-4669-858d-2dc7312150b0)
![Screenshot 2025-02-20 152640](https://github.com/user-attachments/assets/71e427dd-e4f5-40c6-8411-d79b3285201e)
![Screenshot 2025-02-20 152801](https://github.com/user-attachments/assets/fd3fee4c-8aa0-432e-80a1-8bfdda1023c1)
![Screenshot 2025-02-20 152714](https://github.com/user-attachments/assets/4dffc207-79ad-4461-9e94-9cf5e4d2f3cb)

Infected with Qakbot Malware
- Qakbot tend to perform ARP scan in order to discover other endpoints on the network
- I want to check if 10.0.0.149 did an ARP scan on to the broadcast address

![Screenshot 2025-02-20 153942](https://github.com/user-attachments/assets/de76c7f6-dc83-4069-8400-b71987edc808)
![Screenshot 2025-02-20 163512](https://github.com/user-attachments/assets/dc7f6e21-0928-4fc2-a133-454dd8567883)

Adversary indeed did an ARP scan and managed to find 10.0.0.6 and 10.0.0.1 (ICMP REPLY)
- See a lot of imcomplete TCP handshake attempts and reset flags on 10.0.0.1 endpoint

![Screenshot 2025-02-20 161013](https://github.com/user-attachments/assets/42af3c3c-8273-4254-b06b-2c78b37a1bf5)
![Screenshot 2025-02-20 161153](https://github.com/user-attachments/assets/985bdb33-6849-4483-8392-5f6de1b7d4f0)

In the beginning, there was a lot of SMB sessions in the protocol hierarchy
- As we see the objects transferred in SMB --> see a lot of DLL files transfered into our DC endpoint

![Screenshot 2025-02-20 161927](https://github.com/user-attachments/assets/244c2008-8345-4767-aaab-f427012a9838)























