1.

В Linux:
vagrant@vagrant:~$ ip -br link show
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
eth0             UP             08:00:27:73:60:cf <BROADCAST,MULTICAST,UP,LOWER_UP>

В Windows:

PS C:\Users\ACER\Desktop> netsh interface show interface

Состояние адм.  Состояние     Тип              Имя интерфейса
---------------------------------------------------------------------
Разрешен       Подключен      Выделенный       VirtualBox Host-Only Network
Разрешен       Подключен      Выделенный       Беспроводная сеть
Разрешен       Отключен       Выделенный       Ethernet




2.


Протокол LLDP, открытая альтернатива проприетарному протоколу CDP от Cisco.

Пакет lldpd.

Командой lldpctl можно посмотреть соседей. То же самое можно увидеть командой lldpcli sh neigh

lldpcli sh stat sum покажет общую статистику по всем интерфейсам: переданные, полученные пакеты и тд.

lldpcli sh int покажет информацию по интерфейсам, на которых запущен lldpd.


3.


Для разделения коммутатора на несколько виртуальных сетей используется технология VLAN.

В Linux может быть использован следующий пакет apt install vlan. Для настройки VLAN через командную строку можно использовать предустановленный пакет iproute2. Пример настройки через iproute2:

root@vagrant:~# ip -br link
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
eth0             UP             08:00:27:73:60:cf <BROADCAST,MULTICAST,UP,LOWER_UP>
root@vagrant:~# ip link add link eth0 name eth0.10 type vlan id 10
root@vagrant:~# ip -br link
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
eth0             UP             08:00:27:73:60:cf <BROADCAST,MULTICAST,UP,LOWER_UP>
eth0.10@eth0     DOWN           08:00:27:73:60:cf <BROADCAST,MULTICAST>
root@vagrant:~# ip link set dev eth0.10 up
root@vagrant:~# ip -br link
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
eth0             UP             08:00:27:73:60:cf <BROADCAST,MULTICAST,UP,LOWER_UP>
eth0.10@eth0     UP             08:00:27:73:60:cf <BROADCAST,MULTICAST,UP,LOWER_UP>



Настройка VLAN может быть задана через файл /etc/network/interfaces:

auto eth0.20
iface eth0.20 inet static
address 192.168.10.100
netmask 255.255.255.0
vlan_raw_device eth0





4.


root@vagrant:~# modinfo bonding | grep mode:
parm:           mode:Mode of operation; 0 for balance-rr, 1 for active-backup, 2 for balance-xor, 3 for broadcast, 4 for 802.3ad, 5 for balance-tlb, 6 for balance-alb (charp)


Обеспечивают только фейловер, или фейловер и балансировку:

active-backup и broadcast обеспечивают только отказоустойчивость
balance-tlb, balance-alb, balance-rr, balance-xor и 802.3ad обеспечат отказоустойчивость и балансировку
Можно настроить только с одной стороны, или потребуют настройки хоста и свича:

active-backup, balance-tlb и balance-alb работают "сами по себе", можно настроить только на одном хосте
broadcast, balance-rr, balance-xor и 802.3ad потребуют настройки ещё и коммутатора.

Рабочие конфиги:

active-backup на отказоустойчивость:

 network:
   version: 2
   renderer: networkd
   ethernets:
     ens3:
       dhcp4: no 
       optional: true
     ens5: 
       dhcp4: no 
       optional: true
   bonds:
     bond0: 
       dhcp4: yes 
       interfaces:
         - ens3
         - ens5
       parameters:
         mode: active-backup
         primary: ens3
         mii-monitor-interval: 2
balance-alb, балансировка

   bonds:
     bond0: 
       dhcp4: yes 
       interfaces:
         - ens3
         - ens5
       parameters:
         mode: balance-alb
         mii-monitor-interval: 2






5.


В сети с маской /29 - 8 ip адресов, 6 из них можно использовать для хостов.

root@vagrant:~# ipcalc 10.10.10.0/29
Address:   10.10.10.0           00001010.00001010.00001010.00000 000
Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
Wildcard:  0.0.0.7              00000000.00000000.00000000.00000 111
=>
Network:   10.10.10.0/29        00001010.00001010.00001010.00000 000
HostMin:   10.10.10.1           00001010.00001010.00001010.00000 001
HostMax:   10.10.10.6           00001010.00001010.00001010.00000 110
Broadcast: 10.10.10.7           00001010.00001010.00001010.00000 111
Hosts/Net: 6                     Class A, Private Internet


Подсетей с маской /29 в сети с маской /24 - 32 подсети.

root@vagrant:~# ipcalc 10.10.10.0/24 -s 6 6
Address:   10.10.10.0           00001010.00001010.00001010. 00000000
Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
=>
Network:   10.10.10.0/24        00001010.00001010.00001010. 00000000
HostMin:   10.10.10.1           00001010.00001010.00001010. 00000001
HostMax:   10.10.10.254         00001010.00001010.00001010. 11111110
Broadcast: 10.10.10.255         00001010.00001010.00001010. 11111111
Hosts/Net: 254                   Class A, Private Internet

1. Requested size: 6 hosts
Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
Network:   10.10.10.0/29        00001010.00001010.00001010.00000 000
HostMin:   10.10.10.1           00001010.00001010.00001010.00000 001
HostMax:   10.10.10.6           00001010.00001010.00001010.00000 110
Broadcast: 10.10.10.7           00001010.00001010.00001010.00000 111
Hosts/Net: 6                     Class A, Private Internet

2. Requested size: 6 hosts
Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
Network:   10.10.10.8/29        00001010.00001010.00001010.00001 000
HostMin:   10.10.10.9           00001010.00001010.00001010.00001 001
HostMax:   10.10.10.14          00001010.00001010.00001010.00001 110
Broadcast: 10.10.10.15          00001010.00001010.00001010.00001 111
Hosts/Net: 6                     Class A, Private Internet

Needed size:  16 addresses.
Used network: 10.10.10.0/28
Unused:
10.10.10.16/28
10.10.10.32/27
10.10.10.64/26
10.10.10.128/25





6.





Диапазон 100.64.0.0/10 (RFC 6598): возьмем сеть 100.64.0.0/26


root@vagrant:~# ipcalc 100.64.0.0/26
Address:   100.64.0.0           01100100.01000000.00000000.00 000000
Netmask:   255.255.255.192 = 26 11111111.11111111.11111111.11 000000
Wildcard:  0.0.0.63             00000000.00000000.00000000.00 111111
=>
Network:   100.64.0.0/26        01100100.01000000.00000000.00 000000
HostMin:   100.64.0.1           01100100.01000000.00000000.00 000001
HostMax:   100.64.0.62          01100100.01000000.00000000.00 111110
Broadcast: 100.64.0.63          01100100.01000000.00000000.00 111111
Hosts/Net: 62

Маска для диапазонов будет /26, она позволит подключить 62 хоста





7.



Проверить таблицу можно так:

Linux: ip neigh, arp -n
Windows: arp -a
Очистить кеш так:

Linux: ip neigh flush
Windows: arp -d *
Удалить один IP так:

Linux: ip neigh delete <IP> dev <INTERFACE>, arp -d <IP>
Windows: arp -d <IP>




