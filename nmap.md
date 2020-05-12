# nmap

## Semplice guida all'uso
  
Uno dei primi step nell'attaccare un'applicazione web è quella di riuscire a vedere le le porte e i servizi sulla macchina.

---

### Uso normale

-A, to enable OS and version detection, script scanning, and traceroute; -T4 for faster execution;

-sn (No port scan)
This option tells Nmap not to do a port scan after host discovery, and only print out the available hosts that responded to the host discovery probes. .

The default host discovery done with -sn consists of an ICMP echo request, TCP SYN to port 443, TCP ACK to port 80, and an ICMP timestamp request by default. When executed by an unprivileged user, only SYN packets are sent (using a connect call) to ports 80 and 443 on the target. When a privileged user tries to scan targets on a local ethernet network, ARP requests are used unless --send-ip was specified.

-sS (TCP SYN scan)
SYN scan is the default and most popular scan option for good reasons. It can be performed quickly, scanning thousands of ports per second on a fast network not hampered by restrictive firewalls. It is also relatively unobtrusive and stealthy since it never completes TCP connections.

This technique is often referred to as half-open scanning, because you don't open a full TCP connection. You send a SYN packet, as if you are going to open a real connection and then wait for a response. A SYN/ACK indicates the port is listening (open), while a RST (reset) is indicative of a non-listener.

-F (Fast (limited port) scan)
Specifies that you wish to scan fewer ports than the default. Normally Nmap scans the most common 1,000 ports for each scanned protocol. With -F, this is reduced to 100.

Nmap needs an nmap-services file with frequency information in order to know which ports are the most common (see the section called “Well Known Port List: nmap-services” for more about port frequencies). If port frequency information isn't available, perhaps because of the use of a custom nmap-services file, Nmap scans all named ports plus ports 1-1024. In that case, -F means to scan only ports that are named in the services file.


-r (Don't randomize ports)
By default, Nmap randomizes the scanned port order (except that certain commonly accessible ports are moved near the beginning for efficiency reasons). This randomization is normally desirable, but you can specify -r for sequential (sorted from lowest to highest) port scanning instead.

Point Nmap at a remote machine and it might tell you that ports 25/tcp, 80/tcp, and 53/udp are open. Using its nmap-services database of about 2,200 well-known services, Nmap would report that those ports probably correspond to a mail server (SMTP), web server (HTTP), and name server (DNS) respectively.


https://nmap.org/book/man-version-detection.html

Per effettuare una scansione della macchina si utilizza un file già presente in kali linux presente nella directory `/usr/share/wordlists`:

`gobuster -u http://<IP macchina>/<directory>/ -w /usr/share/wordlists/common.txt`

La scansione ritorna il nome delle directory, dei file e l'HTTP response code.

```bash
gobuster -u http://10.10.0.50/dvwa/ -w common.txt

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://10.10.0.50/dvwa/
[+] Threads      : 10
[+] Wordlist     : common.txt
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2019/05/06 11:50:25 Starting gobuster
=====================================================
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/.hta (Status: 403)
/README (Status: 200)
/config (Status: 301)
/docs (Status: 301)
/about (Status: 302)
/external (Status: 301)
/cmd (Status: 200)
/index (Status: 302)
/index.php (Status: 302)
/php.ini (Status: 200)
/logout (Status: 302)
/robots (Status: 200)
/robots.txt (Status: 200)
/login (Status: 200)
/phpinfo.php (Status: 302)
/security (Status: 302)
=====================================================
2019/05/06 11:50:38 Finished
=====================================================
```

Response [https://en.wikipedia.org/wiki/List_of_HTTP_status_codes]

---

#### Aggiungendo il parametro -l (lenght)

`gobuster -u http://<IP macchina>/<directory>/ -w /usr/share/wordlists/common.txt -l`

Ci fornisce la **dimensione** del body.

Se la dimesione è zero di solito non vale la pena andare a guardare.

```bash
gobuster -u http://10.10.0.50/dvwa/ -w common.txt -l

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://10.10.0.50/dvwa/
[+] Threads      : 10
[+] Wordlist     : common.txt
[+] Status codes : 200,204,301,302,307,403
[+] Show length  : true
[+] Timeout      : 10s
=====================================================
2019/05/06 11:52:41 Starting gobuster
=====================================================
/.hta (Status: 403) [Size: 292]
/.htaccess (Status: 403) [Size: 297]
/.htpasswd (Status: 403) [Size: 297]
/README (Status: 200) [Size: 4934]
/config (Status: 301) [Size: 319]
/docs (Status: 301) [Size: 317]
/external (Status: 301) [Size: 321]
/favicon.ico (Status: 200) [Size: 1406]
2019/05/06 11:52:52 [!] Get http://10.10.0.50/dvwa/about: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
/php.ini (Status: 200) [Size: 148]
2019/05/06 11:52:54 [!] Get http://10.10.0.50/dvwa/cmd: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
/robots (Status: 200) [Size: 26]
/robots.txt (Status: 200) [Size: 26]
2019/05/06 11:53:00 [!] Get http://10.10.0.50/dvwa/logout: net/http: request canceled (Client.Timeout exceeded while awaiting headers)
/phpinfo.php (Status: 302) [Size: 0]
/phpinfo (Status: 302) [Size: 0]
/security (Status: 302) [Size: 0]
=====================================================
2019/05/06 11:53:01 Finished
=====================================================
```

---

#### Aggiungendo il parametro -s e l'http status code

`gobuster -u http://<IP macchina>/<directory>/ -w /usr/share/wordlists/common.txt -s 200`

Ovviamente restituisce solo i file che hanno http status 200 o 302.

&nbsp;

```bash
gobuster -u http://10.10.0.50/dvwa/ -w common.txt -s 200,301

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://10.10.0.50/dvwa/
[+] Threads      : 10
[+] Wordlist     : common.txt
[+] Status codes : 200
[+] Timeout      : 10s
=====================================================
2019/05/06 11:54:16 Starting gobuster
=====================================================
/README (Status: 200)
/favicon.ico (Status: 200)
/cmd (Status: 200)
/php.ini (Status: 200)
/login (Status: 200)
/robots (Status: 200)
/robots.txt (Status: 200)
/setup (Status: 200)
=====================================================
2019/05/06 11:54:27 Finished
=====================================================
```

---

#### Se voglio vedere solo i file o le directory

`gobuster -u http://<IP macchina>/<directory>/ -w /usr/share/wordlists/common.txt -q -n`

```bash
~/gobuster# gobuster -u http://10.10.0.50/dvwa/ -w common.txt -q -n

/.hta
/.htpasswd
/.htaccess
/about
/cmd
/php.ini
/index.php
/logout
/robots.txt
/login
/phpinfo.php
/phpinfo
/setup
/security
```

---

#### Se voglio solo file con una determinata estensione (php, txt, html)

`gobuster -u http://<IP macchina>/<directory>/ -w /usr/share/wordlists/common.txt -x .php,.txt,.html`

---

#### Se voglio ridirigere l'output su file

`gobuster -u http://<IP macchina>/<directory>/ -w /usr/share/wordlists/common.txt -o result`

```bash
gobuster -u http://10.10.0.50/dvwa/ -w common.txt -o result

/.hta
/.htpasswd
/.htaccess
/about
/cmd
/php.ini
/index.php
/logout
/robots.txt
/login
/phpinfo.php
/phpinfo
/setup
/security
```

---

#### Tutti i comandi

```~/gobuster# gobuster -h

Usage of gobuster:
  -P string
        Password for Basic Auth (dir mode only)
  -U string
        Username for Basic Auth (dir mode only)
  -a string
        Set the User-Agent string (dir mode only)
  -c string
        Cookies to use for the requests (dir mode only)
  -cn
        Show CNAME records (dns mode only, cannot be used with '-i' option)
  -e    Expanded mode, print full URLs
  -f    Append a forward-slash to each directory request (dir mode only)
  -fw
        Force continued operation when wildcard found
  -i    Show IP addresses (dns mode only)
  -k    Skip SSL certificate verification
  -l    Include the length of the body in the output (dir mode only)
  -m string
        Directory/File mode (dir) or DNS mode (dns) (default "dir")
  -n    Don't print status codes
  -np
        Don't display progress
  -o string
        Output file to write results to (defaults to stdout)
  -p string
        Proxy to use for requests [http(s)://host:port] (dir mode only)
  -q    Don't print the banner and other noise
  -r    Follow redirects
  -s string
        Positive status codes (dir mode only) (default "200,204,301,302,307,403")
  -t int
        Number of concurrent threads (default 10)
  -to duration
        HTTP Timeout in seconds (dir mode only) (default 10s)
  -u string
        The target URL or Domain
  -v    Verbose output (errors)
  -w string
        Path to the wordlist
  -x string
        File extension(s) to search for (dir mode only)
  ```

---

#### Fonti

* [repository github](https://github.com/OJ/gobuster) repository github
* [Gobuster](https://null-byte.wonderhowto.com/how-to/scan-websites-for-interesting-directories-files-with-gobuster-0197226/) - Guida gobuster <https://null-byte.wonderhowto.com>
* [Gobuster](https://redteamtutorials.com/2018/11/19/gobuster-cheatsheet/) - Guida gobuster <https://redteamtutorials.com>
* [Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#html) Guida per scrtivere questo documento
