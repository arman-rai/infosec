Description: A Services based room, extracting information from HTTP Services and finding the hidden messages.

## Initial reconnaisance

At first I did a nmap scan 
`nmap -sn 10.10.110.200` and it was alive. This was an easy box so I did an aggressive scan
and I found this using `Nmap 7.95 scan initiated Mon Jul  7 20:24:30 2025 as: /usr/lib/nmap/nmap --privileged -A -T5 -oN nmap.aggro5 10.10.110.200`

`Nmap scan report for 10.10.110.200
Host is up (0.18s latency).
Not shown: 983 filtered tcp ports (no-response), 15 filtered tcp ports (admin-prohibited)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 ba:a2:40:8e:de:c3:7b:c7:f7:b3:7e:0c:1e:ec:9f:b8 (RSA)
|   256 38:28:4c:e1:4a:75:3d:0d:e7:e4:85:64:38:2a:8e:c7 (ECDSA)
|_  256 1a:33:a0:ed:83:ba:09:a5:62:a7:df:ab:2f:ee:d0:99 (ED25519)
80/tcp open  http    nginx 1.14.1
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
|_http-server-header: nginx/1.14.1
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.4 (91%), Android 9 - 10 (Linux 4.9 - 4.14) (91%), Linux 2.6.32 - 3.13 (91%), Linux 3.10 - 4.11 (91%), Linux 3.2 - 4.14 (91%), Linux 4.15 (91%), Linux 4.15 - 5.19 (91%), Linux 2.6.32 - 3.10 (91%), Linux 5.4 (90%), Linux 3.10 - 3.13 (90%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 5 hops

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   38.35 ms  10.17.0.1
2   ... 4
5   197.13 ms 10.10.110.200`

and then I again nmapped on the known services
`Nmap 7.95 scan initiated Mon Jul  7 20:34:27 2025 as: /usr/lib/nmap/nmap --privileged -sCV -p 22,80 -oN nmap.p22_80 -v unstable.thm
Nmap scan report for unstable.thm (10.10.110.200)
Host is up (0.17s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 ba:a2:40:8e:de:c3:7b:c7:f7:b3:7e:0c:1e:ec:9f:b8 (RSA)
|   256 38:28:4c:e1:4a:75:3d:0d:e7:e4:85:64:38:2a:8e:c7 (ECDSA)
|_  256 1a:33:a0:ed:83:ba:09:a5:62:a7:df:ab:2f:ee:d0:99 (ED25519)
80/tcp open  http    nginx 1.14.1
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
|_http-server-header: nginx/1.14.1
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS`



Also I did some fuzzing
`ffuf -u http://unstable.thm/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt                                                          
                                                                                                                                                              
        /'___\  /'___\           /'___\                                                                                                                       
       /\ \__/ /\ \__/  __  __  /\ \__/                                                                                                                       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\                                                                                                                      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/                                                                                                                      
         \ \_\   \ \_\  \ \____/  \ \_\                                                                                                                       
          \/_/    \/_/   \/___/    \/_/        

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://unstable.thm/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

info                    [Status: 200, Size: 148, Words: 29, Lines: 2, Duration: 224ms]
:: Progress: [4586/220560] :: Job [1/1] :: 235 req/sec :: Duration: [0:00:21] :: Errors: 0 ::
[ERR] NOPE
get_image               [Status: 500, Size: 291, Words: 38, Lines: 5, Duration: 175ms]
:: Progress: [220560/220560] :: Job [1/1] :: 231 req/sec :: Duration: [0:17:49] :: Errors: 0 ::`

Then  I tried the `/info` endpoint but stil