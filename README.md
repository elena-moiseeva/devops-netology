1.



vagrant@vagrant:~$ telnet stackoverflow.com 80
Trying 151.101.193.69...
Connected to stackoverflow.com.
Escape character is '^]'.
GET /questions HTTP/1.0
HOST: stackoverflow.com

HTTP/1.1 301 Moved Permanently
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/questions
x-request-guid: 57e43ec2-32f3-40c3-affe-20eb577b3913
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Fri, 31 Dec 2021 11:59:24 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-fra19144-FRA
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1640951964.039622,VS0,VE92
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=b3ad848f-a7a4-5212-26c3-248cca2ebc19; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly


Connection closed by foreign host.

Вернулся код 301 Moved Permanently - это означает, что запрошенный ресурс был перемещен в новое месторасположение, на которое указывает location: https://stackoverflow.com/questions.





2.


 


В ответ получили код 307 (Temporary Redirect)


 


Страница полностью загрузилась за 1.32 сек. Самый долгий запрос - начальная загрузка страницы 501 мс



3.

85.174.200.166




4.


vagrant@vagrant:~$ whois 85.174.200.166 | grep ^descr
descr:          OJSC Rostelecom Macroregional Branch South
descr:          "Sochielectrosvyaz", Sochi, Russia
descr:          OJSC Rostelecom Macroregional Branch South
vagrant@vagrant:~$ whois 85.174.200.166 | grep ^origin
origin:         AS12389




5.


vagrant@vagrant:~$ traceroute -An 8.8.8.8 -I
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  10.0.2.2 [*]  0.377 ms  0.338 ms  0.323 ms
 2  192.168.1.1 [*]  25.476 ms  47.098 ms  47.276 ms
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  142.251.71.194 [AS15169]  73.205 ms * *
10  108.170.232.251 [AS15169]  73.176 ms  75.923 ms  80.689 ms
11  172.253.79.169 [AS15169]  83.835 ms  86.487 ms  69.603 ms
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  8.8.8.8 [AS15169]  65.457 ms  64.432 ms  66.673 ms



6.



                               My traceroute  [v0.93]
vagrant (10.0.2.15)                                        2022-01-02T18:05:29+0000
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                           Packets               Pings
 Host                                    Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. AS???    10.0.2.2                     0.0%    16    0.9   0.7   0.3   1.1   0.2
 2. AS???    192.168.1.1                  0.0%    16    4.5   7.0   3.3  26.2   6.2
 3. AS???    100.96.0.1                  25.0%    16   33.9  46.4  24.0 159.0  46.9
 4. AS12389  94.233.252.179              68.8%    16   25.1  37.1  24.1  84.6  26.6
 5. AS12389  94.233.252.178              31.2%    16   38.0  85.4  36.1 250.7  75.8
 6. (waiting for reply)
 7. (waiting for reply)
 8. AS15169  108.170.250.146             53.3%    16   59.8  87.0  59.8 153.3  36.3
 9. AS15169  142.251.71.194              37.5%    16   78.0  93.1  74.2 188.7  35.7
10. AS15169  108.170.232.251              6.2%    16   68.4  84.5  68.4 147.6  23.2
11. AS15169  172.253.79.169              12.5%    16   73.6  90.5  70.5 241.2  44.3
12. (waiting for reply)
13. (waiting for reply)
14. (waiting for reply)
15. (waiting for reply)
16. (waiting for reply)
17. (waiting for reply)
18. (waiting for reply)
19. (waiting for reply)
20. (waiting for reply)
21. AS15169  8.8.8.8                      0.0%    13   68.7  78.4  68.0 138.2  19.5


Наибольшая задержка на участке AS15169  142.251.71.194 - в этом месте ping составляет 78 мс.





7.


Получаем список DNS серверов:
vagrant@vagrant:~$ dig dns.google NS +noall +answer
dns.google.             21600   IN      NS      ns1.zdns.google.
dns.google.             21600   IN      NS      ns4.zdns.google.
dns.google.             21600   IN      NS      ns3.zdns.google.
dns.google.             21600   IN      NS      ns2.zdns.google.

Получаем список A записей:
vagrant@vagrant:~$ dig dns.google A +noall +answer
dns.google.             654     IN      A       8.8.8.8
dns.google.             654     IN      A       8.8.4.4



8.



vagrant@vagrant:~$ dig -x 8.8.4.4 +noall +answer
4.4.8.8.in-addr.arpa.   25029   IN      PTR     dns.google.
vagrant@vagrant:~$ dig -x 8.8.8.8 +noall +answer
8.8.8.8.in-addr.arpa.   23034   IN      PTR     dns.google.

К обоим IP адресам привязано доменное имя dns.google.


