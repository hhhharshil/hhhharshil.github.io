---
title: Must Have Tools On Kali Linux for CTFs
date: 2021-05-23
categories: Kali
---

# Updog
 Updog is a replacement for pythons SimpleHTTPServer. It can be used to transfer files from your kali machine on to the target box. It allows downloading files on both the HTTP/HTTPS protocols. Easy installation is available via pip which usually comes preinstalled on Kali. 

Usage: updog [-d DIRECTORY] [-p PORT] [--password PASSWORD] [--ssl]

Example: updog -p 1111 
Serves the files in the current working directory under port 1111

Reference: <a href="https://github.com/sc0tfree/updog">Updog Github</a>


<br>

# Impacket

Impacket is a collection of python scripts that can be used by an attacker to target Windows machines. This tool is primarily used for enumeration purposes such as capturing hashes. Once hashes are captured these can be then used to elevate a users privleges. 

Example: psexec.py - mimics psexec this can be used to move laterally throughout the network through the use of captured hashes.

Reference: <a href="https://github.com/SecureAuthCorp/impacket">Impacket Github</a>

<br>

# PEASS - Privilege Escalation Awesome Scripts SUITE

Both WinPEAS and LinPEAS are enumeration scripts used to elevate privieleges. These are useful since they help find easy methods to get root/admin privelges. Updog can be used to set up a http server then from there on on the target machine must download the corresponding script. 

Reference: <a href="https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite">PEASS Github</a>

<br>

# AutoRecon

AutoRecon is a tool used for recon purposes. It performs automated enumeration of services. It's intentions are to save time during CTFs and other penetration environments such as the OSCP.

Note by default auto exploitation features are disabled to fall in line with the OSCP rules. The tool can be configured in the manner to allow auto exploitation but this is not needed for the most part and is completely optional.

Reference: <a href="https://github.com/Tib3rius/AutoRecon">AutoRecon Github</a>
