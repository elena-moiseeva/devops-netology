1.




cd /opt
sudo wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
sudo tar xzf node_exporter-1.2.2.linux-amd64.tar.gz
sudo rm -f node_exporter-1.2.2.linux-amd64.tar.gz
sudo touch node_exporter-1.2.2.linux-amd64/node_exporter.env
echo "EXTRA_OPTS=\"--log.level=info\"" | sudo tee node_exporter-1.2.2.linux-amd64/node_exporter.env
sudo mkdir -p /usr/local/lib/systemd/system/
sudo touch /usr/local/lib/systemd/system/node_exporter.service
sudo systemctl daemon-reload
sudo systemctl enable node_exporter.service
unit-файл:

[Unit]
Description="Netology course node_exporer service file"

[Service]
EnvironmentFile=/opt/node_exporter-1.2.2.linux-amd64/node_exporter.env
ExecStart=/opt/node_exporter-1.2.2.linux-amd64/node_exporter $EXTRA_OPTS
StandardOutput=file:/var/log/node_explorer.log
StandardError=file:/var/log/node_explorer.log

[Install]
WantedBy=multi-user.target
добавления опций к запускаемому процессу через внешний файл
можно добавить через файл /opt/node_exporter-1.2.2.linux-amd64/node_exporter.env, в переменной EXTRA_OPTS

EXTRA_OPTS="--log.level=info"

vagrant@vagrant:~$ ps -e |grep node_exporter
   2329 ?        00:00:00 node_exporter
vagrant@vagrant:~$ sudo systemctl stop node_exporter
vagrant@vagrant:~$ ps -e |grep node_exporter
vagrant@vagrant:~$ systemctl start node_exporter
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to start 'node_exporter.service'.
Authenticating as: vagrant,,, (vagrant)
Password:
==== AUTHENTICATION COMPLETE ===
vagrant@vagrant:~$ ps -e |grep node_exporter
   2447 ?        00:00:00 node_exporter
vagrant@vagrant:~$ sudo cat /proc/2447/environ
LANG=en_US.UTF-8LANGUAGE=en_US:PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/binINVOCATION_ID=96f3ca4b9e01466cb67b585d912085e5EXTRA_OPTS=--log.level=info


$ journalctl -u node_exporter.service
Dec 27 21:27:15 vagrant systemd[1]: Started "Netology course node_exporer s>
Dec 27 21:42:46 vagrant systemd[1]: Stopping "Netology course node_exporer >
Dec 27 21:42:46 vagrant systemd[1]: node_exporter.service: Succeeded.
Dec 27 21:42:46 vagrant systemd[1]: Stopped "Netology course node_exporer s>
Dec 27 21:43:23 vagrant systemd[1]: Started "Netology course node_exporer s>










2



CPU:
    node_cpu_seconds_total{cpu="0",mode="idle"} 2238.49
    node_cpu_seconds_total{cpu="0",mode="system"} 16.72
    node_cpu_seconds_total{cpu="0",mode="user"} 6.86
    process_cpu_seconds_total
    
Memory:
    node_memory_MemAvailable_bytes 
    node_memory_MemFree_bytes
    
Disk(если несколько дисков то для каждого):
    node_disk_io_time_seconds_total{device="sda"} 
    node_disk_read_bytes_total{device="sda"} 
    node_disk_read_time_seconds_total{device="sda"} 
    node_disk_write_time_seconds_total{device="sda"}
    
Network(так же для каждого активного адаптера):
    node_network_receive_errs_total{device="eth0"} 
    node_network_receive_bytes_total{device="eth0"} 
    node_network_transmit_bytes_total{device="eth0"}
    node_network_transmit_errs_total{device="eth0"}








3.



vagrant@vagrant:~$ sudo lsof -i :19999
COMMAND PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
netdata 613 netdata    4u  IPv4  23176      0t0  TCP *:19999 (LISTEN)
netdata 613 netdata   32u  IPv4  25513      0t0  TCP vagrant:19999->_gateway:60627 (ESTABLISHED)

 






4.




Можно, так как есть соответствующая строка:
vagrant@vagrant:~$ dmesg |grep virtualiz
[    0.002191] CPU MTRRs all blank - virtualized system.
[    0.105094] Booting paravirtualized kernel on KVM
[    5.137509] systemd[1]: Detected virtualization oracle.








5.






vagrant@vagrant:~$ /sbin/sysctl -n fs.nr_open
1048576
максимальное число открытых дескрипторов для ядра (системы), для пользователя задать больше этого числа нельзя (если не менять). 
Число задается кратное 1024, в данном случае =1024*1024.


vagrant@vagrant:~$ cat /proc/sys/fs/file-max
9223372036854775807
макс. предел ОС

vagrant@vagrant:~$ ulimit -Sn
1024

мягкий лимит (так же ulimit -n)на пользователя (может быть увеличен процессов в процессе работы)

vagrant@vagrant:~$ ulimit -Hn
1048576

жесткий лимит на пользователя (не может быть увеличен, только уменьшен)





6.




root@vagrant:~# unshare --pid --fork --mount-proc sleep 1h &
[1] 1439
root@vagrant:~# lsns
        NS TYPE   NPROCS   PID USER            COMMAND
4026531835 cgroup    105     1 root            /sbin/init
4026531836 pid       104     1 root            /sbin/init
4026531837 user      105     1 root            /sbin/init
4026531838 uts       103     1 root            /sbin/init
4026531839 ipc       105     1 root            /sbin/init
4026531840 mnt        93     1 root            /sbin/init
4026531860 mnt         1    21 root            kdevtmpfs
4026531992 net       105     1 root            /sbin/init
4026532162 mnt         1   392 root            /lib/systemd/systemd-udevd
4026532163 uts         1   392 root            /lib/systemd/systemd-udevd
4026532164 mnt         1   398 systemd-network /lib/systemd/systemd-networkd
4026532183 mnt         1   554 systemd-resolve /lib/systemd/systemd-resolved
4026532184 mnt         2  1439 root            unshare --pid --fork --mount-
4026532185 pid         1  1440 root            sleep 1h
4026532249 mnt         1   581 root            /usr/sbin/irqbalance --foregr
4026532250 mnt         1   592 root            /lib/systemd/systemd-logind
4026532251 uts         1   592 root            /lib/systemd/systemd-logind
4026532252 mnt         4   613 netdata         /usr/sbin/netdata -D
root@vagrant:~# nsenter -a -t 1440
root@vagrant:/# ps
    PID TTY          TIME CMD
      1 pts/0    00:00:00 sleep
      2 pts/0    00:00:00 bash
     11 pts/0    00:00:00 ps





7.




:(){ :|:& };:
 
Написана на баше. Разберем каждый символ
: - пустая операция. Если ввести ее в линуксовую консоль... ничего не произойдет
%имя% (%аргументы%) {%тело%} - оболочка для объявления функции в баше
| - pipe. Этот оператор позволяет перевести вывод (stdout) команды на ввод (stdin) следующей. как пример - "cat file | grep word". cat по дефолту выводит все строки file на консоль, но из-за оператора "|", он передает вывод на команду grep. а команда grep уже выводит строки на консоль.
& - откладывает операцию в фон. программа продолжает выполнятся, но вы сможете вводить в консоль новые команды. вывод также идет на консоль
; - разделение команд. смысл тот же, что и в программировании. Например, если сделать "ls;ls" - будет дважды выведен список директорий
 
 сначала объявляется функция с именем ":", в которой запускается эта же функцию, и откладывается в фон.
А после объявления функции, с помощью последнего ":", ее запускаем.
запускается процесс, который форкается бесконечно пока не используются все доступные номера PID, или система не виснет из-за ухода в своп. 




dmesg показал, что сработали лимиты системы cgroups на уровне user.slice. Видимо сработали ограничения на максимальное число запущенных процессов в оболочке пользователя:
[ 7678.125941] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-4.scope

Текущий лимит на максимальное число запущенных процессов для пользователя, который вошел в систему, можно посмотреть командой (стоит limit: 5012):

vagrant@vagrant:~$ systemctl status user-1000.slice
● user-1000.slice - User Slice of UID 1000
     Loaded: loaded
    Drop-In: /usr/lib/systemd/system/user-.slice.d
             └─10-defaults.conf
     Active: active since Fri 2021-12-17 09:03:14 UTC; 10h ago
       Docs: man:user@.service(5)
      Tasks: 7 (limit: 2356)
     Memory: 11.5M
     CGroup: /user.slice/user-1000.slice

Настройки этого лимита по умолчанию можно посмотреть командой (стоит 33% от системного ограничения):

[Unit]
Description=User Slice of UID %j
Documentation=man:user@.service(5)
After=systemd-user-sessions.service
StopWhenUnneeded=yes

[Slice]
TasksMax=33%

Если установить ulimit -u 40 - число процессов будет ограниченно 40 для пользоователя.






