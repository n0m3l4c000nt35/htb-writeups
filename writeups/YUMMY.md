<div align=center>
  <img src="https://github.com/user-attachments/assets/a475c06b-f2dc-4c07-91d7-bf7e2bcaa6d4" width="500">
</div>

```bash
ping -c 1 10.10.11.36 -R

PING 10.10.11.36 (10.10.11.36) 56(124) bytes of data.
64 bytes from 10.10.11.36: icmp_seq=1 ttl=63 time=176 ms
RR: 	10.10.14.191
	10.10.10.2
	10.10.11.36
	10.10.11.36
	10.10.14.1
	10.10.14.191

--- 10.10.11.36 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 175.923/175.923/175.923/0.000 ms
```

TTL 63: Máquina Linux

```bash
sudo nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.11.36 -oG allPorts

PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 63
80/tcp open  http    syn-ack ttl 63
```

```bash
nmap -p22,80 -sCV 10.10.11.36 -oN targeted

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a2ed6577e9c42f134919b0b809eb5636 (ECDSA)
|_  256 bcdf25355c9724f269b4ce6017503cf0 (ED25519)
80/tcp open  http    Caddy httpd
|_http-title: Yummy
|_http-server-header: Caddy
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Agregar al archivo `/etc/hosts` `10.10.11.36 yummy.htb`

```bash
whatweb http://yummy.htb

http://yummy.htb [200 OK] Bootstrap, Country[RESERVED][ZZ], Email[info@yummy.htb], Frame, HTML5, HTTPServer[Caddy], IP[10.10.11.36], Lightbox, Script, Title[Yummy]
```

Registrar usuario

Loguearse

Reservar mesa

Descargar archivo `.ics`

```bash
file Yummy_reservation_20250224_215007.ics

Yummy_reservation_20250224_215007.ics: vCalendar calendar file
```

```bash
exiftool Yummy_reservation_20250224_215007.ics

ExifTool Version Number         : 12.57
File Name                       : Yummy_reservation_20250224_215007.ics
Directory                       : .
File Size                       : 268 bytes
File Modification Date/Time     : 2025:02:24 18:50:08-03:00
File Access Date/Time           : 2025:02:24 18:50:19-03:00
File Inode Change Date/Time     : 2025:02:24 18:50:11-03:00
File Permissions                : -rw-r--r--
File Type                       : ICS
File Type Extension             : ics
MIME Type                       : text/calendar
VCalendar Version               : 2.0
Software                        : ics.py - http://git.io/lLljaA
Description                     : Email: test@test.com.Number of People: 1.Message: test
Date Time Start                 : 2025:02:24 00:00:00Z
Summary                         : test
UID                             : 74907f64-f16d-4271-a2b8-b8c1c6d6919e@7490.org
```
