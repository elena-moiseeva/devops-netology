1.


>>> a = 1
>>> b = '2'
>>> c = a + b
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str'
>>> c=a+int(b)
>>> print(c)
3
>>> c=int(str(a)+str(b))
>>> print(c)
12



2.

#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(os.path.abspath(prepare_result))
#        break

vagrant@vagrant:~/netology/sysadm-homeworks$ ./3script_py.sh
/home/vagrant/netology/sysadm-homeworks/1.md
/home/vagrant/netology/sysadm-homeworks/11.md




3.

#!/usr/bin/env python3
print("Введите полный путь к репозиторию(со знаком '/' в конце):")
path = input()

import os

os.chdir(path)
bash_command = ["git status"]
result_os = os.popen(' && '.join(bash_command)).read()
#is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(path+prepare_result)
#        break


vagrant@vagrant:~/netology/sysadm-homeworks$ ./2script_py.sh
Введите полный путь к репозиторию(со знаком '/' в конце):
/home/vagrant/netology/sysadm-homeworks/
/home/vagrant/netology/sysadm-homeworks/1.md
/home/vagrant/netology/sysadm-homeworks/11.md
vagrant@vagrant:~/netology/sysadm-homeworks$




4.


#!/usr/bin/env python3

import socket
import time

# Наши узлы
hosts = ('drive.google.com', 'mail.google.com', 'google.com')
# Объявляем пустой dict
ip_host = {}
while ( True ):
    #  проходимся по каждому узлу
    for host in hosts:
        # Получаем его ip адреса
        ip = socket.gethostbyname_ex(host)[2]
        # При первой проверке наш dict пустой, поэтому запустим обработку исключений
        # и просто заполним наш dict
        try:
            # Если новый ip не отличается от старого, то просто печатаем инфо 
            if ( ip_host[host] == ip ):
                print(host + " - " + str(ip))
            # Если IP отличается, то печатаем ошибку
            else:
                print("[ERROR] " + host + " IP mismatch: " + ip_host[host] + " " + ip)
            # Обновляем IP адреса
            ip_host[host] = ip
        # Здесь просто заполняем наш dict и выводим сообщения
        except:
            ip_host[host] = ip
            print(host + " - " + str(ip))
        time.sleep(3)




vagrant@vagrant:~$ ./1script_py.sh
drive.google.com - ['142.251.1.194']
mail.google.com - ['216.58.210.133']
google.com - ['216.58.209.174']
drive.google.com - ['142.251.1.194']
mail.google.com - ['216.58.210.133']
google.com - ['216.58.209.174']
drive.google.com - ['142.251.1.194']
mail.google.com - ['216.58.210.133']
google.com - ['216.58.209.174']
drive.google.com - ['142.251.1.194']
mail.google.com - ['216.58.210.133']
google.com - ['216.58.209.174']
drive.google.com - ['142.251.1.194']
mail.google.com - ['216.58.210.133']

