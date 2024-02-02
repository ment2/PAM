
Создаём пользователя otusadm и otus:
[root@pam ~]# sudo useradd otusadm && sudo useradd otus

Создаём пользователям пароли:
[root@pam ~]# echo 'Otus2022!' | sudo passwd --stdin otusadm && echo 'Otus2022!' | sudo passwd --stdin otus
Changing password for user otusadm.
passwd: all authentication tokens updated successfully.
Changing password for user otus.
passwd: all authentication tokens updated successfully.

Создаём группу admin:
[root@pam ~]# sudo groupadd -f admin

Добавляем пользователей vagrant,root и otusadm в группу admin:
[root@pam ~]# usermod otusadm -a -G admin && usermod root -a -G admin && usermod vagrant -a -G admin


Пытаемся подключиться с хостовой машины:
root@Ubuntu:~# ssh otus@192.168.57.10
The authenticity of host '192.168.57.10 (192.168.57.10)' can't be established.
ED25519 key fingerprint is SHA256:jPQ7kFhniDW0qOUeaYHXV3GfY+NZtve6MOD6MVZ3llQ.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.57.10' (ED25519) to the list of known hosts.
otus@192.168.57.10's password: 
[otus@pam ~]$ exit

Проверим, что пользователи root, vagrant и otusadm есть в группе admin:
[root@pam ~]# cat /etc/group | grep admin
printadmin:x:997:
admin:x:1003:otusadm,root,vagrant

Создаем скрипт:
[root@pam ~]# vi /usr/local/bin/login.sh

Даем права на его выполнение:
[root@pam ~]# chmod +x /usr/local/bin/login.sh

Добавляем модуль pam_exec и наш скрипт:
[root@pam ~]# vi /etc/pam.d/sshd

Переводим дату на субботу:
[root@pam ~]# sudo date 020302032024.00

Sat Feb  3 02:03:00 UTC 2024

Пытаемся подключиться с хоста:
root@Ubuntu:~# ssh otus@192.168.57.10
otus@192.168.57.10's password: 
Permission denied, please try again.
otus@192.168.57.10's password: 
Permission denied, please try again.
otus@192.168.57.10's password: 
otus@192.168.57.10: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).

Возвращаем время на пятницу:
[root@pam ~]# sudo date 020202032024.00
Fri Feb  2 02:03:00 UTC 2024

Пытемся подключиться:
root@Ubuntu:~# ssh otus@192.168.57.10
otus@192.168.57.10's password: 
Last failed login: Sat Feb  3 02:04:20 UTC 2024 from 192.168.57.1 on ssh:notty
There were 3 failed login attempts since the last successful login.
Last login: Fri Feb  2 12:33:59 2024 from 192.168.57.1
[otus@pam ~]$ exit

Переводим опять на субботу:
[root@pam ~]# sudo date 020302032024.00
Sat Feb  3 02:03:00 UTC 2024

Пытаемся подключиться:
Connection to 192.168.57.10 closed.
root@Ubuntu:~# ssh otus@192.168.57.10
otus@192.168.57.10's password: 
Permission denied, please try again.



