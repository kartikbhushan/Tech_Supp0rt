# Tech_Supp0rt
Try Hack me room - Tech_Supp0rt 

## IP address - 10.10.69.82

## Nmap Scan - 
```
Ports 
22 - ssh - Protected 
80 - Http Apache 
139 - samba share
445 - netbios - samba
```

## Enumeration - 
```
GoBuster 

Found 
/test
/wordpress
```

## Running Samba enum - 
```
smbmap -H 10.10.69.82

[+] Guest session   	IP: 10.10.69.82:445	Name: 10.10.69.82                                       
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	websvr                                            	READ ONLY	
	IPC$                                              	NO ACCESS	IPC Service (TechSupport server (Samba, Ubuntu))


```

#### We Can see that We have Read access using share Websvr 
```
smbclient //10.10.69.82/websvr                                                                                                                                                      130 ⨯
Enter WORKGROUP\kali's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat May 29 03:17:38 2021
  ..                                  D        0  Sat May 29 03:03:47 2021
  enter.txt                           N      273  Sat May 29 03:17:38 2021

		8460484 blocks of size 1024. 5698120 blocks available
smb: \> get enter.txt .txt
getting file \enter.txt of size 273 as .txt (0.4 KiloBytes/sec) (average 0.4 KiloBytes/sec)
smb: \> get enter.txt
getting file \enter.txt of size 273 as enter.txt (0.4 KiloBytes/sec) (average 0.4 KiloBytes/sec)
smb: \> exit 

```

Upon Catting out the enter.txt file 

```
GOALS
=====
1)Make fake popup and host it online on Digital Ocean server
2)Fix subrion site, /subrion doesn't work, edit from panel
3)Edit wordpress website

IMP
===
Subrion creds
|->admin:7sKvntXdPEJaxazce9PXi24zaFrLiKWCk [cooked with magical formula]
Wordpress creds
|->

```

We Can see a new endpoint Subrion
```
On Searching on Github found Subrion to me a CMS and there is robots.txt file 

Intially invlaid request on intercepting request with burpsuite pings wrong host ip . Changed the IP and was able to access robots.txt


tried the End point 
/subrion/panel - Gives a Login page
```


Trying to crack the Password in the "enter.txt file from SMB share webSvr"
```
"
Subrion creds
|->admin:7sKvntXdPEJaxazce9PXi24zaFrLiKWCk [cooked with magical formula]
"

Using Cyberchef and MAgic Function 

Found out it was Base58 Encoding 
From_Base58('123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz',false)
From_Base32('A-Z2-7=',false)
From_Base64('A-Za-z0-9+/=',true)

Result - Scam2021

```


Ok We are in the Admin Account


Now we need access to the machine 

### Running Searchspoilt on subrion version 4.2.1 (listed in the weblogin page)

```
searchsploit subrion          
------------------------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                                                                              |  Path
------------------------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
Subrion 3.x - Multiple Vulnerabilities                                                                                                                      | php/webapps/38525.txt
Subrion 4.2.1 - 'Email' Persistant Cross-Site Scripting                                                                                                     | php/webapps/47469.txt
Subrion Auto Classifieds - Persistent Cross-Site Scripting                                                                                                  | php/webapps/14391.txt
SUBRION CMS - Multiple Vulnerabilities                                                                                                                      | php/webapps/17390.txt
Subrion CMS 2.2.1 - Cross-Site Request Forgery (Add Admin)                                                                                                  | php/webapps/21267.txt
subrion CMS 2.2.1 - Multiple Vulnerabilities                                                                                                                | php/webapps/22159.txt
Subrion CMS 4.0.5 - Cross-Site Request Forgery (Add Admin)                                                                                                  | php/webapps/47851.txt
Subrion CMS 4.0.5 - Cross-Site Request Forgery Bypass / Persistent Cross-Site Scripting                                                                     | php/webapps/40553.txt
Subrion CMS 4.0.5 - SQL Injection                                                                                                                           | php/webapps/40202.txt
Subrion CMS 4.2.1 - 'avatar[path]' XSS                                                                                                                      | php/webapps/49346.txt
Subrion CMS 4.2.1 - Arbitrary File Upload                                                                                                                   | php/webapps/49876.py
Subrion CMS 4.2.1 - Cross-Site Scripting 
```

We need Subrion CMS 4.2.1 - Arbitrary File Upload | php/webapps/49876.py

Mirrioing the exploit script and trying the Admin credentials :) 


