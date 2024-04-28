
# LLMNR POISONING AND SMB REPLAY ATTACKS ON ACTIVE DIRECTORY

 
## Objective
 LLMNR poisoning involves intercepting and tampering with LLMNR requests and responses on a local network. The primary objective is to redirect network traffic intended for resolving hostnames to an attacker-controlled system and the implication here by poisoning LLMNR responses, attackers can impersonate legitimate network resources, such as servers or other network devices, leading to various security risks.
 SMB attacks targeting Active Directory aim to exploit vulnerabilities in the SMB protocol implementation or misconfigurations within the AD environment to compromise domain controllers which result to severe consequences for network and security integrity and mitigation
 - <img width="582" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/473afa52-139b-4001-aa26-5139be3c4da3">
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
- Metasploit to execute vulnerabilities to shell access .
- Responder to exploit weakness.
- Hashcat used for cracking passwords.

## Step1 LLMNR Poisoning
- I eth0 specifies the network interface to use for listening and capturing network traffic. In this case, "eth0" refers to the Ethernet interface. You may need to replace "eth0" with the appropriate network interface on your system.
- W informs responder to perform LLMNR and NBT-TS poisoning, d enables HTTP mode
- <img width="412" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/014c85a6-e677-4aeb-97fb-67ddf85729d1">
*Ref 2: the screen shot shows the out of the command and notice both the LLMNR is ON as well as HTTP server which are significant for the attack, and now responder is listening for a connection*
- Now the attacker machine is listening for the event will then start up the Punisher machine under fcastle user in the MARVEL domain and call out the attacker IP from fcastle machine
- <img width="489" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/cb6284f0-d43d-4efc-8f72-83c52382655f">
 *Ref 3: ones you quick search the attacker IP address  it will prompt network credential login simply ignore those and immidiately check the attack machine for response*
 - Ones that connection has been established a hash will be generated
 - <img width="337" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/4b9f5f36-bf28-4a6f-b32f-755edaa01fd9">
 - With the given hash, next step is cracking the hash using hashcat but first identifying the type of hashcat hence will use grep NTLM thus network protocol v2
 - <img width="291" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/6bad1e34-2b63-4245-82a3-bcf1893227f7">
  *Ref 4:  v2 in thi case is 5600*
-  i have named hashes.txt for basing on different directories therefore will still use hashes.txt along with rockyou , additionally will add the force to ensure that the hash is not aborted before completion
-   <img width="291" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/ee16b14b-ef31-49b8-8ec4-e57c268689fb">
- <img width="289" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/33970692-e51d-4ec4-a46b-ec1de38c517f">
-  *Ref 5:  The snippet shows the password cracked and password itself*
-  running the show command seperately  also shows the hash and password
-  <img width="473" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/4307bdd1-84b6-4e7f-b657-65c1a0e6fd62">
## Step SMB Relay
- To perform SMB replay, first i had to identify the smbhost from the DC machine using nmap without activating smbserver
- <img width="421" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/05802604-0189-410f-b997-41383c1e0291">
- Next, i created IP addreses that dont need smb server under targets.txt then disabling SMP and HTTP server
- <img width="291" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/7b42bdf1-c585-4309-b69a-1ea61dd6cfcf">
- <img width="291" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/83448170-7f40-4af9-bce4-ed009b0fcd89">
- verifying the update
- <img width="382" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/0b209e46-cfae-45df-bf67-e3da3b4dcec8">
- Next, will run the targets.txt file  in smb2support  and callout the attackers IP from the local punisher machine  which will fail  because you can't relay to yourself, however,(peterparker -> the SPIDERMAN MACHINE )worked (succed)  and it dumped both the administrator hash and peterparker  hash
- <img width="289" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/bb196d91-976b-4dd4-bb41-9c8b91ff7caf">
- The second options was to run through the ineteractive mode thus –I to produce a client shell
-  <img width="424" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/3f9844a3-1793-4cb6-9fd8-faadf3c17df0">
- Running nc 127.0.0.1 11000 initiates a connection to the local machine (127.0.0.1) on port 11000 using the nc command which is netcat and i was able to view files and and users
- <img width="291" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/9d9f9f54-6997-439b-a25e-92068e14f969">
- I also added whoami for more confirmations
- <img width="291" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/0db45fc0-29d5-4fce-91d1-d9fd70a2b536">
## Step Gaining shell Access with Metasploit
- with enumaration i used metaspoilt >psexec > exploit/windows/smb/psexec, using this command to  executing commands on remote Windows systems. It leverages the Windows SMB (Server Message Block) protocol to authenticate and execute commands on remote systems
- <img width="327" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/bed3f620-4064-4fae-aa34-c6f840f1d0d7">
- Next searched for options and notice the payload, first i update   the payload
- <img width="338" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/c433caa9-6d26-4a6f-a364-ef42224cf0f5">
 *Ref 6:  this also points out the module used as windows/smb/psexec*
 - the payload set up
 - <img width="290" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/7c51c253-ee65-41bd-9952-163f735f5834">
  *Ref 7: the command allows you to gain remote access to a 64-bit Windows system and execute Meterpreter commands on it*
 - similar,i set up all the other requirements like rhost, subdomain, smbuser and smbpass as illustrated below
 -  <img width="290" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/19b9f808-3e54-4fd1-86d1-f1e71a8351d2">
 - Next, running thge explotation
 - <img width="491" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/846cb87d-434f-4376-ab3a-a324401847b4">
 - Now perform an NTML attack using the gained session thus hash attack but first unset the subdomain then set the smbuse as administrator, similarly i called the out half session of the hash generated from responder earlier and set it as smbpass
 - <img width="372" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/3222deab-c70b-45aa-9228-72bbd04e8115">
 after the set up i was able to excute the attack and thus the output
 - <img width="491" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/fce0cbfb-5142-4fdf-8794-60bfe3dc1b28">
 - Then i attempted to gain access to shell remotely using psexec.py with punisher machine and its password and IP address
 - <img width="421" alt="image" src="https://github.com/chromosems/Attacking-active-Directory-with-LLMNR-poisoning-and-SMB-replay/assets/44053943/1af170b8-431d-46ab-a72b-b3f4aa9bf91e">
 -  *Ref 8:  the snippet shows access to the shell as it produces output for whoami*

 





























