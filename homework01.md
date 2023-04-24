Задание 1. *Необходимо продемонстрировать изоляцию одного и того же приложения (как мы решили на семинаре - командного интерпретатора) в различных пространствах имен.*

**Chroot**

Создаем директорию: **for_chroot_sh**
- vboxuser@LINUX-VIRTUAL:~$ sudo su - root
- [sudo] пароль для vboxuser: 
- root@LINUX-VIRTUAL:~# 
- root@LINUX-VIRTUAL:~# mkdir for_chroot_sh

Копируем исполняемый файл в каталог for_chroot_sh
- root@LINUX-VIRTUAL:~# cp --parents /usr/bin/sh for_chroot_sh/

Просматриваем общие библиотеки
- root@LINUX-VIRTUAL:~# ldd /usr/bin/sh

Нужные библиотеки, пропускаем - linux-vdso.so.1
- linux-vdso.so.1 (0x00007fff1efec000)

	linux-vdso.so.1 (0x00007ffd7b7f1000)

	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 

    (0x00007f5717800000)

	/lib64/ld-linux-x86-64.so.2 (0x00007f5717a81000)

Копируем
- root@LINUX-VIRTUAL:~# cp --parents /lib/x86_64-linux-gnu/libc.so.6 for_chroot_sh/

- root@LINUX-VIRTUAL:~# cp --parents /lib64/ld-linux-x86-64.so.2 for_chroot_sh/

Далее запускаем
- root@LINUX-VIRTUAL:~# chroot for_chroot_sh/ /usr/bin/sh
# 
**Рекурсивный просмотр for_chroot_sh**
- root@LINUX-VIRTUAL:~# ll -R for_chroot_sh/

for_chroot_sh/:

total 20
- drwxr-xr-x 5 root root 4096 мар 25 22:30 ./
- drwx------ 5 root root 4096 мар 25 22:27 ../
- drwxr-xr-x 3 root root 4096 мар 25 22:29 lib/
- drwxr-xr-x 2 root root 4096 мар 25 22:30 lib64/
- drwxr-xr-x 3 root root 4096 мар 25 22:28 usr/


for_chroot_sh/lib:

total 12
- drwxr-xr-x 3 root root 4096 мар 25 22:29 ./
- drwxr-xr-x 5 root root 4096 мар 25 22:30 ../
- drwxr-xr-x 2 root root 4096 мар 25 22:29 x86_64-linux-gnu/

for_chroot_sh/lib/x86_64-linux-gnu:

total 2176
- drwxr-xr-x 2 root root    4096 мар 25 22:29 ./
- drwxr-xr-x 3 root root    4096 мар 25 22:29 ../
-rw-r--r-- 1 root root 2216304 мар 25 22:29 libc.so.6

for_chroot_sh/lib64:

total 244
- drwxr-xr-x 2 root root   4096 мар 25 22:30 ./
- drwxr-xr-x 5 root root   4096 мар 25 22:30 ../
-rwxr-xr-x 1 root root 240936 мар 25 22:30 ld-linux-x86-64.so.2*

for_chroot_sh/usr:

total 12
- drwxr-xr-x 3 root root 4096 мар 26 10:28 ./
- drwxr-xr-x 5 root root 4096 мар 26 10:30 ../
- drwxr-xr-x 2 root root 4096 мар 26 10:28 bin/

for_chroot_sh/usr/bin:

total 132

- drwxr-xr-x 2 root root   4096 мар 26 10:28 ./
- drwxr-xr-x 3 root root   4096 мар 26 10:28 ../
-rwxr-xr-x 1 root root 125688 мар 26 10:28 sh*
