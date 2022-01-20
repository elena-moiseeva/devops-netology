1.


vagrant@vagrant:~$ a=1
vagrant@vagrant:~$ b=2
vagrant@vagrant:~$ c=a+b
vagrant@vagrant:~$ echo $c
a+b
vagrant@vagrant:~$ d=$a+$b
vagrant@vagrant:~$ echo $d
1+2
vagrant@vagrant:~$ e=$(($a+$b))
vagrant@vagrant:~$ echo $e
3
vagrant@vagrant:~$

c = a+b потому что переменным может быть присвоино целое число либо строка. Т.к. a+b не целое число, значит присвоилась строка.
d = 1+2 потому что переменная d не целочисленная, но т.к. перед a и b знак $, то с присвоилась строка значений a и b.
e = 3 потому что благодаря $(( )) происходит присвоение целочисленной суммы a и b в e.




2.



    1. в условии нехватате закрывающей скобки ) - пометил *
    2. слишком частые проверки забивают файл, нужно добавить sleep $timeout - для задания интервала проверки
    3. нужно добавить проверку успешности чтоб выйти из цикла
       например: else exit
В итоге так:

    while (( 1 == 1 ))
    do
        curl https://localhost:4757
        if (($? != 0))
        then
            date >> curl.log
        else exit
        fi
        sleep 5
    done



vagrant@vagrant:~$ ./bash.sh
curl: (7) Failed to connect to localhost port 4757: Connection refused
curl: (7) Failed to connect to localhost port 4757: Connection refused
curl: (7) Failed to connect to localhost port 4757: Connection refused
curl: (7) Failed to connect to localhost port 4757: Connection refused
curl: (7) Failed to connect to localhost port 4757: Connection refused
curl: (7) Failed to connect to localhost port 4757: Connection refused





3.



#!/usr/bin/env bash
declare -i a
a=0
while (($a<5))
do 
curl 192.168.0.1:80
echo $? >> curl.log
curl 173.194.222.113:80
echo $? >> curl.log
curl 87.250.250.242:80
echo $? >> curl.log
let "a +=1"
done



4.




ip_array=(192.168.0.1:80 173.194.222.113:80 87.250.250.242:80)
while ((1==1))
  do
  for i in ${ip_array[@]}
    do
    curl $i
    if (($? != 0))
      then
      echo $i > error
      break 2
    fi
  done
done

