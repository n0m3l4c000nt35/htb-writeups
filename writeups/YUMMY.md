<div align=center>
  <img src="https://github.com/user-attachments/assets/a475c06b-f2dc-4c07-91d7-bf7e2bcaa6d4" width="500">
</div>

<br>

```bash
ports=$(nmap -p- --min-rate=1000 -T4 10.10.11.36 | grep "^[0-9]" | cut -d '/' -f1 | tr '\n' ',' | sed s/,$//)
nmap -p$ports -sC -sV 10.10.11.36
```

```plaintext
PORT      STATE  SERVICE      VERSION
22/tcp    open   ssh          OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a2ed6577e9c42f134919b0b809eb5636 (ECDSA)
|_  256 bcdf25355c9724f269b4ce6017503cf0 (ED25519)
80/tcp    open   http         Caddy httpd
|_http-server-header: Caddy
|_http-title: Yummy
2306/tcp  closed tappi-boxnet
8999/tcp  closed bctp
11453/tcp closed unknown
15701/tcp closed unknown
18830/tcp closed unknown
29535/tcp closed unknown
41675/tcp closed unknown
44147/tcp closed unknown
48286/tcp closed unknown
48721/tcp closed unknown
60291/tcp closed unknown
62627/tcp closed unknown
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

