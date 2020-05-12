# gobuster

## Semplice guida all'uso
  
Uno dei primi step nell'attaccare un'applicazione web è quella di riuscire a vedere le directory e i file nascosti.

### Vantaggi

Gobuster rispetto ad altri (DirBuster e DIRB) essendo implementato in Go risulta essere più veloce.

### Svantaggi

Risulta più lento rispetto altri altri nel caso in cui si debbano fare ricerche ricorsive cioè i percorsi annidati anche solo di un livello.

---

#### Uso normale

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
