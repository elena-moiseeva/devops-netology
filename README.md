1.

Команда cd встроенная.
Встроенная, потому что, работать внутри сессии терминала логичнее менять указатель на текущую директорию внутренней функцией, 
Если использовать внешний вызов, то он будет работать со своим окружением, и менять  текущий каталог внутри своего окружения, а на вызвавший shell влиять не будет.  



2.


vagrant@vagrant:~$ grep Actress name.txt | wc -l
11
vagrant@vagrant:~$ grep Actress name.txt -c
11

-c, --count
Подавляет обычный вывод; вместо этого выводит подсчет совпадающих строк для каждого входного файла.



3.


pstree   - отображение дерева процессов
-p         - идентификаторы PID 

vagrant@vagrant:~$ pstree -p
systemd(1)─┬─VBoxService(795)─┬─{VBoxService}(797)
           │                  ├─{VBoxService}(798)
           │                  ├─{VBoxService}(799)
           │                  ├─{VBoxService}(800)
           │                  ├─{VBoxService}(801)
           │                  ├─{VBoxService}(802)
           │                  ├─{VBoxService}(803)
           │                  └─{VBoxService}(804)



4.


$ id
$ tty
$ ls -l `tty`
$ screen
ctrl-a d
$ ls -l \root 2>/dev/pts/1
$ who
vagrant  pts/0        2021-11-20 17:49 (10.0.2.2)
vagrant  pts/1        2021-11-20 19:44 (:pts/0:S.0)
$ screen -ls
$ screen –r 1590.pts-0.vagrant
vagrant@vagrant:~$ ls: cannot access 'root': No such file or directory



5.


vagrant@vagrant:~$  find / -name core > /tmp/testfile
vagrant@vagrant:~$ ls -l
total 24


6.


vagrant@vagrant:~$ echo "hello world" >file.input
vagrant@vagrant:~$ cat <file.input> file.output
vagrant@vagrant:~$ cat file.output
hello world
vagrant@vagrant:~$





7.


bash 5>&1 - Создаст дескриптор с 5 и перенатправит его в stdout


echo netology > /proc/$$/fd/5 - выведет в дескриптор "5", который был пернеаправлен в stdout



8.


vagrant@vagrant:~$ ls /fakdir 6>&1 7>&2 2>&6 1>&9 | wc -l
1



9.


Будут выведены переменные окружения:
можно получить тоже самое (только с разделением по переменным по строкам):
printenv
env



10.



/proc/<PID>/cmdline - полный путь до исполняемого файла процесса [PID]  

/proc/<PID>/exe - содержит ссылку до файла запущенного для процесса [PID], 
                        cat выведет содержимое запущенного файла, 
                        запуск этого файла,  запустит еще одну копию самого файла




11.



SSE 4.2
vagrant@vagrant:~$ grep sse /proc/cpuinfo



12.


при подключении ожидается пользователь, а не другой процесс, и нет локального tty в данный момент. 
для запуска можно добавить -t - , и команда исполняется c принудительным созданием псевдотерминала



13.

Открываю 2 параллельные ssh сессии.

В одной сессии запускаю, например top. Отсоединяю его от текущей сесcии путем bg, disown top.

   vagrant@vagrant:~$ ps -a
        PID TTY          TIME CMD
          2638 pts/0    00:00:00 top
           2688 pts/0    00:00:00 ps
Перехожу в другой терминал, выполняю reptyr 2638 (это PID моего процесса).

Во время выполнения столкнулся с проблемой, Unable to attach to pid 2638: Operation not permitted The kernel denied permission while attaching. If your uid matches the target's, check the value of /proc/sys/kernel/yama/ptrace_scope. For more information, see /etc/sysctl.d/10-ptrace.conf

После изменения в /proc/sys/kernel/yama/ptrace_scope значения kernel.yama.ptrace_scope с 1 на 0 - передача процесса в другую сессию заработала.
echo 0|sudo tee /proc/sys/kernel/yama/ptrace_scope

reptyr 2638
отображается таблица с процессами и информацией по ним.









14.



команда tee -  считывает ввод команды, далее подает результат на стандартный вывод (в терминал), а так же записывает его в файл. Таким образом мы можем наблюдать что у нас будет записано в файл, не открывая его. Так же можно записать результат выполнения в несколько файлов.

  echo является встроенной командой оболочки, а не простой командой, следовательно параметр sudo на неё не распространяется, чтобы выполнить echo с правами sudo, надо 
  запустить саму оболочку от имени su. А вот tee уже является сторонней по отношению к shell командой, не встроенной, по этому её можно запустить под sudo. По этому команда
  sudo echo не будет работать, т.к. она будет выполняться от имени обычного пользователя, у которого нету прав на доступ к /root/new_file, а команда tee уже будет обращаться
  к репозиторию /root/ от имени su, следовательно права на доступ у неё будут.





