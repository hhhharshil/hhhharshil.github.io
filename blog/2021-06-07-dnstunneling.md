---
title: DNS Tunneling
date: 2021-06-07
categories: Blue Team, Red Team, Persistence
---

# What is DNS TUNNELING?
DNS Tunneling is the use of the DNS protocol to encode/transfer data inside of an organizations network. Attackers primarily use this technique to maintain persistence and for evasion purposes. For example, DNS tunneling is commonly used to maintain CnC traffic as this allows attackers to maintain remote access.

<br>

# Why DNS TUNNELING?
Many networks now lock down outbound access through the use of ACLs on firewalls. DNS Tunneling allows attackers to send malicious traffic out from a compromised machine using the DNS protocol which is commonly enabled. Tunneling allows communication to be sent on top of an existing protocol. This is typically used to hide information hence it can be used for data exfiltration and C2 purposes. DNS is not typically monitored heavily in terms of outbound DNS requests.

<br>

# How to detect DNS TUNNELING? 
Looking for unusual data being sent back and forth can be considered one of the best ways to detect DNS tunneling. If utilizing a SIEM filtering for hostnames can be a great way to threat hunt for DNS tunneling. In addition, looking for an abnormal amount of DNS requests is another way to analyze traffic. I.E. Analyze payloads for any malicious domain names which can be done through the use of threat feeds/signatures. 

<br>

# Example Tool

# DNSCat2
A client/server tool which can be used to set up DNS tunneling. The client is designed to be run on the compromised machine. It is written in C so it can run anywhere without the need of extra add-ons. Must have an authoritative domain which is set then all traffic will be routed from the local DNS server to the authoritative DNS server. This specific tool is great for CnC as all traffic is encrypted by default.

## Reference 
https://github.com/iagox86/dnscat2
