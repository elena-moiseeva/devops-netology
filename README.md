1.




vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ curl http://localhost:9100/metrics
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0
go_gc_duration_seconds{quantile="0.25"} 0
go_gc_duration_seconds{quantile="0.5"} 0
go_gc_duration_seconds{quantile="0.75"} 0
go_gc_duration_seconds{quantile="1"} 0
go_gc_duration_seconds_sum 0
go_gc_duration_seconds_count 0
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 8
# HELP go_info Information about the Go environment.
# TYPE go_info gauge
go_info{version="go1.17.3"} 1
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 1.384792e+06
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 1.384792e+06
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 1.445703e+06
# HELP go_memstats_frees_total Total number of frees.
# TYPE go_memstats_frees_total counter


vagrant@vagrant:~$ ps -e |grep node_exporter   
   3001 pts/1        00:00:00 node_exporter
vagrant@vagrant:~$ systemctl stop node_exporter
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to stop 'node_exporter.service'.
Authenticating as: vagrant,,, (vagrant)
Password: 
==== AUTHENTICATION COMPLETE ===
vagrant@vagrant:~$ ps -e |grep node_exporter
vagrant@vagrant:~$ systemctl start node_exporter
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to start 'node_exporter.service'.
Authenticating as: vagrant,,, (vagrant)
Password: 
==== AUTHENTICATION COMPLETE ===
vagrant@vagrant:~$ ps -e |grep node_exporter
  3063 pts/1         00:00:00 node_exporter



vagrant@vagrant:~$ cat /etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter

[Service]
ExecStart=/opt/node_exporter/node_exporter
EnvironmentFile=/etc/default/node_exporter

[Install]
WantedBy=default.target



vagrant@vagrant:/etc/systemd/system$ sudo cat /proc/3052/environ
SHELL=/bin/bashLANGUAGE=en_US:PWD=/home/vagrant/node_exporter-1.3.1.linux-amd64LOGNAME=vagrantXDG_SESSION_TYPE=ttyMOTD_SHOWN=pamHOME=/home/vagrantLANG=en_US.UTF-8LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:SSH_CONNECTION=10.0.2.2 58884 10.0.2.15 22LESSCLOSE=/usr/bin/lesspipe %s %sXDG_SESSION_CLASS=userTERM=xterm-256colorLESSOPEN=| /usr/bin/lesspipe %sUSER=vagrantSHLVL=1XDG_SESSION_ID=22XDG_RUNTIME_DIR=/run/user/1000SSH_CLIENT=10.0.2.2 58884 22XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktopPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/binDBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/busSSH_TTY=/dev/pts/0_=/usr/bin/screenOLDPWD=/home/vagrant











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






