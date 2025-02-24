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
