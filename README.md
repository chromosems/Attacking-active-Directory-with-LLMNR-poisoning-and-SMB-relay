
# LLMNR POISONING AND SMB REPLAY ATTACKS ON ACTIVE DIRECTORY

 
## Objective
 LLMNR poisoning involves intercepting and tampering with LLMNR requests and responses on a local network. The primary objective is to redirect network traffic intended for resolving hostnames to an attacker-controlled system and the implication here by poisoning LLMNR responses, attackers can impersonate legitimate network resources, such as servers or other network devices, leading to various security risks.
 SMB attacks targeting Active Directory aim to exploit vulnerabilities in the SMB protocol implementation or misconfigurations within the AD environment to compromise domain controllers which result to severe consequences for the network and security intergrity further more mitigating the attacks was key to the study
 - <img width="845" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/c45b1dd8-4e41-423b-95f7-fb844969b22f">
 *Ref 1:  Diagram of the labset up used for the attack*


### Skills Learned
- Active directory enumaration
- Capturing Hashes with responder.
- hash cracking.
- LLMNR poisoning mitigation
- SMBhost.
- SMB relay attack mitigation
- Gaining shell through metaspoilt
- nmapping used too identify smbhost

### Tools Used
- metaspoilt to execute exppoilts to shell access .
- Responder to exploit weakness.
- Hashcat used for cracking passwords.

## Steps
- I eth0 specifies the network interface to use for listening and capturing network traffic. In this case, "eth0" refers to the Ethernet interface. You may need to replace "eth0" with the appropriate network interface on your system.
- W informs responder to perform LLMNR and NBT-TS poisoning, d enables HTTP mode
- <img width="356" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/ede5e2bf-197c-4806-8b75-c197987e3ee2">
*Ref 1: Network Diagram*
