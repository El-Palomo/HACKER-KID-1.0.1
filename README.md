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




