1.



chdir("/tmp")



2.



openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY)=3






3.


agrant@vagrant:~$ lsof -p 1541
...
pulseaudi 1541 vagrant    6u      REG                0,1 67108864  32280 /memfd:pulseaudio (deleted)
vagrant@vagrant:~$ echo '' >/proc/1541/fd/6


4.




"Зомби" процессы, в отличии от "сирот" освобождают свои ресурсы, но не освобождают запись в таблице процессов.
запись освободиться при вызове wait() родительским процессом.





5.




root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
root@vagrant:~# /usr/sbin/opensnoop-bpfcc
PID    COMM               FD ERR PATH
15436  systemd-udevd      14   0 /sys/fs/cgroup/unified/system.slice/systemd-udevd.service/cgroup.procs
15436  systemd-udevd      14   0 /sys/fs/cgroup/unified/system.slice/systemd-udevd.service/cgroup.threads
600    irqbalance          6   0 /proc/interrupts
600    irqbalance          6   0 /proc/stat
600    irqbalance          6   0 /proc/irq/20/smp_affinity
600    irqbalance          6   0 /proc/irq/0/smp_affinity
600    irqbalance          6   0 /proc/irq/1/smp_affinity




6. 


Part of the utsname information is also accessible  via  /proc/sys/ker‐
       nel/{ostype, hostname, osrelease, version, domainname}



7.


Запускает команду, стоящую за символом &&, только если команда, стоящая перед этим символом была выполнена успешно
Точка с запятой (;) должна разделять команды в группе последовательных команд. Точка с запятой является специальным символом, приказывающим shell выполнить первую команду, ждать ее завершения, затем выполнить следующую команду, ждать ее завершения и т.д. Это означает, что каждая команда строки выполняется поочередно, слева направо, не проверяя, успешно ли выполнилась предыдущая команда.

set -e - прерывает сессию при любом ненулевом значении исполняемых команд в конвеере кроме последней.
в случае &&  вместе с set -e- вероятно не имеет смысла, так как при ошибке , выполнение команд прекратиться.




8. 



set -e - прекращает выполнение скрипта если команда завершилась ошибкой, выводит в stderr строку с ошибкой. Обойти эту проверку можно добавив в пайплайн к команде true: mycommand | true.

set -u - прекращает выполнение скрипта, если встретилась несуществующая переменная.

set -x - выводит выполняемые команды в stdout перед выполненинем.

set -o pipefail - прекращает выполнение скрипта, даже если одна из частей пайпа завершилась ошибкой. В этом случае bash-скрипт завершит выполнение, если mycommand вернёт ошибку, не смотря на true в конце пайплайна: mycommand | true.



9.



Ss - процесс, ожидающий завершения, лидер сессии
R+ - процесс выполняется, фоновый процесс

