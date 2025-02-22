<div align=center>
  <img src="https://github.com/user-attachments/assets/a475c06b-f2dc-4c07-91d7-bf7e2bcaa6d4" width="500">
</div>

<br>

```bash
nmap -p- --min-rate 10000 10.10.11.36
```

```plaintext
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

```bash
nmap -p22,80 -sCV 10.10.11.36
```

```plaintext
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a2ed6577e9c42f134919b0b809eb5636 (ECDSA)
|_  256 bcdf25355c9724f269b4ce6017503cf0 (ED25519)
80/tcp open  http    Caddy httpd
|_http-title: Did not follow redirect to http://yummy.htb/
|_http-server-header: Caddy
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```bash
sudo nano /etc/hosts
```

```plaintext
<target-ip> yummy.htb
```
