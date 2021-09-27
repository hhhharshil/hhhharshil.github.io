---
title: File Inclusion 101
date: 2021-09-26
categories: WebApp
---

# File Inclusions
RFI and LFI are common vulnerabilities present in web applications. This occurs when code is written in a manner which allows the user to submit input into files or upload files to the server without proper validation. File Inclusion vulnerabilities may lead to information disclosure or remote code execution.
<br>

# Two Types of File Inclusion
The Two Types of File Inclusion are LFI and RFI.

LFI is a vulnerability which allows attackers to include files that are present on the local machine. LFI is common in php based sites, however; it can also be present in sites based in JSP or ASP. An example of a file which is commonly used as input would be the /etc/passwd file which we will show an example of.

RFI on the other hand is an vulnerability which allows attackers to include remote files as input. Attackers can take advantage of this vulnerability by referencing a malicious file hosted on an external webserver. This file can then be used as a payload to pop a shell on the target machine.

# Exploiting LFI

To exploit LFI first we have to find a file that we can pass input to. Any script that includes a file on a web server is a place to test for LFI.

Please see the example site below:
```sh
www.hhhharshil.com/script.php?page=index.html
```

The site is running php code and uses the script.php file to reference the page parameter which fetches the page index.html which is stored locally on the machine.

We can try testing for LFI by changing the parameter the "page" variable is referencing.

# LFI Example

To showcase an example, I will be using the room "Inclusion" which is available on TryHackMe

[Try Out Inclusion Here](https://tryhackme.com/room/inclusion)

![File Inclusion 1]({{site.url}}/assets/images/file-inclusions/file-inclusion-1.png)

As shown in the bottom left hand corner we have a file "article" which is using the variable name to input a file that is hosted locally on the webserver. This is a great candidate to test our local file inclusion.

<img src="/assets/images/file-inclusions/file-inclusion-2.png">

In the example above we replaced the input which was set to "hacking" which was a reference to a web page stored locally to the /etc/passwd file. The usage of "../" would be dependent on the configuration of the web server. It may need more or less of those characters as the directory can be stored in a different area. This is known as directory traversal.

As you can see from the screenshot above we have successfully been able to test for local file inclusion.
