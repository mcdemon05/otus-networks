## Задание:

В данной самостоятельной работе необходимо распланировать адресное пространство.
Настроить IP на всех активных портах для дальнейшей работы над проектом.
Адресное пространство должно быть задокументировано.

##  Решение:

- [Конфигурационные файлы;](configs/)
- [Сохраненная топология из EVE-NG;](eve-ng_lab_IPv4_IPv6.zip)

### Графическая схема

![](Topology.PNG)

### Адресное пространство:

| Автономка           | IPv4 подсети                                     | IPv6 подсети           |
|---------------------|--------------------------------------------------|------------------------|
| AS520 (Триада)      | 5.20.0.0/16                                      | 2001:DB8:520::/48      |
| AS101 (Киторн)      | 101.0.0.0/16                                     | 2001:DB8:101::/48      |
| AS301 (Ламас)       | 30.1.0.0/16                                      | 2001:DB8:301::/48      |
| AS1001 Москва       | 100.1.0.0/16                                     | 2001:DB8:1001::/48     |
| AS1001 Чокурдах     | 100.1.1.0/24<br>100.1.10.16/28<br>100.1.20.16/28 | 2001:DB8:1001:A00::/56 |
| AS1001 Лабытнанги   | 100.1.2.0/24                                     | 2001:DB8:1001:B00::/56 |
| AS2042 С.-Петербург | 20.42.0.0/16                                     | 2001:DB8:2042::/48     |

### IP интерфейсы:

AS520 (Триада)

| Device | Interface                           | IPv4 Address                                                                   | IPv6 Address                                                                                                     |
|--------|-------------------------------------|--------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| **R23**| Lo1<br>e0/0<br>e0/1<br>e0/2         | 5.20.0.23/32<br>5.20.23.0/31<br>172.16.1.0/31<br>172.16.1.2/31                 | 2001:DB8:520::23/128<br>FE80::23 link-local<br>FE80::23 link-local<br>FE80::23 link-local                        |
| **R24**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 5.20.0.24/32<br>5.20.24.0/31<br>172.16.1.4/31<br>172.16.1.3/31<br>5.20.24.2/31 | 2001:DB8:520::24/128<br>FE80::24 link-local<br>FE80::24 link-local<br>FE80::24 link-local<br>FE80::24 link-local |
| **R25**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 5.20.0.25/32<br>172.16.1.1/31<br>5.20.25.0/31<br>172.16.1.6/31<br>5.20.25.2/31 | 2001:DB8:520::25/128<br>FE80::25 link-local<br>FE80::25 link-local<br>FE80::25 link-local<br>FE80::25 link-local |
| **R26**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 5.20.0.26/32<br>172.16.1.5/31<br>5.20.26.0/31<br>172.16.1.7/31<br>5.20.26.2/31 | 2001:DB8:520::26/128<br>FE80::26 link-local<br>FE80::26 link-local<br>FE80::26 link-local<br>FE80::26 link-local |
<details>
  <summary>R23</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 5.20.0.23 255.255.255.255
 ipv6 address 2001:DB8:520::23/128
!
interface Ethernet0/0
 no shutdown
 ip address 5.20.23.0 255.255.255.254
 ipv6 address FE80::23 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.0 255.255.255.254
 ipv6 address FE80::23 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.2 255.255.255.254
 ipv6 address FE80::23 link-local
!
ip route 5.20.0.24 255.255.255.255 172.16.1.3
ip route 5.20.0.24 255.255.255.255 172.16.1.1 2
ip route 5.20.0.25 255.255.255.255 172.16.1.1
ip route 5.20.0.25 255.255.255.255 172.16.1.3 2
ip route 5.20.0.26 255.255.255.255 172.16.1.1
ip route 5.20.0.26 255.255.255.255 172.16.1.3 2
ip route 20.42.0.0 255.255.0.0 172.16.1.3
ip route 20.42.0.0 255.255.0.0 172.16.1.1 2
ip route 30.1.0.0 255.255.0.0 172.16.1.3
ip route 30.1.0.0 255.255.0.0 172.16.1.1 2
ip route 30.1.0.0 255.255.0.0 5.20.23.1 3
ip route 100.1.0.0 255.255.0.0 5.20.23.1
ip route 100.1.0.0 255.255.0.0 172.16.1.3 2
ip route 100.1.0.0 255.255.0.0 172.16.1.1 3
ip route 100.1.1.0 255.255.255.0 172.16.1.1
ip route 100.1.1.0 255.255.255.0 172.16.1.3 2
ip route 100.1.2.0 255.255.255.0 172.16.1.1
ip route 100.1.2.0 255.255.255.0 172.16.1.3 2
ip route 100.1.10.16 255.255.255.240 172.16.1.1
ip route 100.1.10.16 255.255.255.240 172.16.1.3 2
ip route 100.1.20.16 255.255.255.240 172.16.1.1
ip route 100.1.20.16 255.255.255.240 172.16.1.3 2
ip route 101.0.0.0 255.255.0.0 5.20.23.1
ip route 101.0.0.0 255.255.0.0 172.16.1.3 2
ip route 101.0.0.0 255.255.0.0 172.16.1.1 3
!
ipv6 route 2001:DB8:101::/48 Ethernet0/0 FE80::22
ipv6 route 2001:DB8:101::/48 Ethernet0/2 FE80::24 2
ipv6 route 2001:DB8:101::/48 Ethernet0/1 FE80::25 3
ipv6 route 2001:DB8:301::/48 Ethernet0/2 FE80::24
ipv6 route 2001:DB8:301::/48 Ethernet0/1 FE80::25 2
ipv6 route 2001:DB8:301::/48 Ethernet0/0 FE80::22 3
ipv6 route 2001:DB8:520::24/128 Ethernet0/2 FE80::24
ipv6 route 2001:DB8:520::24/128 Ethernet0/1 FE80::25 2
ipv6 route 2001:DB8:520::25/128 Ethernet0/1 FE80::25
ipv6 route 2001:DB8:520::25/128 Ethernet0/2 FE80::24 2
ipv6 route 2001:DB8:520::26/128 Ethernet0/1 FE80::25
ipv6 route 2001:DB8:520::26/128 Ethernet0/2 FE80::24 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/2 FE80::24 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/1 FE80::25
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/2 FE80::24 2
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/1 FE80::25
ipv6 route 2001:DB8:1001::/48 Ethernet0/0 FE80::22
ipv6 route 2001:DB8:1001::/48 Ethernet0/2 FE80::24 2
ipv6 route 2001:DB8:1001::/48 Ethernet0/1 FE80::25 3
ipv6 route 2001:DB8:2042::/48 Ethernet0/2 FE80::24
ipv6 route 2001:DB8:2042::/48 Ethernet0/1 FE80::25 2
!
</pre>
</details>

<details>
  <summary>R24</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 5.20.0.24 255.255.255.255
 ipv6 address 2001:DB8:520::24/128
!
interface Ethernet0/0
 no shutdown
 ip address 5.20.24.0 255.255.255.254
 ipv6 address FE80::24 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.4 255.255.255.254
 ipv6 address FE80::24 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.3 255.255.255.254
 ipv6 address FE80::24 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 5.20.24.2 255.255.255.254
 ipv6 address FE80::24 link-local
!
ip route 5.20.0.23 255.255.255.255 172.16.1.2
ip route 5.20.0.23 255.255.255.255 172.16.1.5 2
ip route 5.20.0.25 255.255.255.255 172.16.1.5
ip route 5.20.0.25 255.255.255.255 172.16.1.2 2
ip route 5.20.0.26 255.255.255.255 172.16.1.5
ip route 5.20.0.26 255.255.255.255 172.16.1.2 2
ip route 20.42.0.0 255.255.0.0 5.20.24.3
ip route 20.42.0.0 255.255.0.0 172.16.1.5 2
ip route 20.42.0.0 255.255.0.0 172.16.1.2 3
ip route 30.1.0.0 255.255.0.0 5.20.24.1
ip route 30.1.0.0 255.255.0.0 172.16.1.2 2
ip route 30.1.0.0 255.255.0.0 172.16.1.5 3
ip route 100.1.0.0 255.255.0.0 5.20.24.1
ip route 100.1.0.0 255.255.0.0 172.16.1.2 2
ip route 100.1.0.0 255.255.0.0 172.16.1.5 3
ip route 100.1.1.0 255.255.255.0 172.16.1.5
ip route 100.1.1.0 255.255.255.0 172.16.1.2 2
ip route 100.1.2.0 255.255.255.0 172.16.1.5
ip route 100.1.2.0 255.255.255.0 172.16.1.2 2
ip route 100.1.10.16 255.255.255.240 172.16.1.5
ip route 100.1.10.16 255.255.255.240 172.16.1.2 2
ip route 100.1.20.16 255.255.255.240 172.16.1.5
ip route 100.1.20.16 255.255.255.240 172.16.1.2 2
ip route 101.0.0.0 255.255.0.0 5.20.24.1
ip route 101.0.0.0 255.255.0.0 172.16.1.2 2
ip route 101.0.0.0 255.255.0.0 172.16.1.5 3
!
ipv6 route 2001:DB8:101::/48 Ethernet0/0 FE80::21
ipv6 route 2001:DB8:101::/48 Ethernet0/2 FE80::23 2
ipv6 route 2001:DB8:101::/48 Ethernet0/1 FE80::26 3
ipv6 route 2001:DB8:301::/48 Ethernet0/0 FE80::21
ipv6 route 2001:DB8:301::/48 Ethernet0/2 FE80::23 2
ipv6 route 2001:DB8:301::/48 Ethernet0/1 FE80::26 3
ipv6 route 2001:DB8:520::23/128 Ethernet0/2 FE80::23
ipv6 route 2001:DB8:520::23/128 Ethernet0/1 FE80::26 2
ipv6 route 2001:DB8:520::25/128 Ethernet0/1 FE80::26
ipv6 route 2001:DB8:520::25/128 Ethernet0/2 FE80::23 2
ipv6 route 2001:DB8:520::26/128 Ethernet0/1 FE80::26
ipv6 route 2001:DB8:520::26/128 Ethernet0/2 FE80::23 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/2 FE80::23 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/1 FE80::26
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/2 FE80::23 2
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/1 FE80::26
ipv6 route 2001:DB8:1001::/48 Ethernet0/0 FE80::21
ipv6 route 2001:DB8:1001::/48 Ethernet0/2 FE80::23 2
ipv6 route 2001:DB8:1001::/48 Ethernet0/1 FE80::26 3
ipv6 route 2001:DB8:2042::/48 Ethernet0/3 FE80::18
ipv6 route 2001:DB8:2042::/48 Ethernet0/1 FE80::26 2
ipv6 route 2001:DB8:2042::/48 Ethernet0/2 FE80::23 3
!
</pre>
</details>

<details>
  <summary>R25</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 5.20.0.25 255.255.255.255
 ipv6 address 2001:DB8:520::25/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.1 255.255.255.254
 ipv6 address FE80::25 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 5.20.25.0 255.255.255.254
 ipv6 address FE80::25 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.6 255.255.255.254
 ipv6 address FE80::25 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 5.20.25.2 255.255.255.254
 ipv6 address FE80::25 link-local
!
ip route 5.20.0.23 255.255.255.255 172.16.1.0
ip route 5.20.0.23 255.255.255.255 172.16.1.7 2
ip route 5.20.0.24 255.255.255.255 172.16.1.0
ip route 5.20.0.24 255.255.255.255 172.16.1.7 2
ip route 5.20.0.26 255.255.255.255 172.16.1.7
ip route 5.20.0.26 255.255.255.255 172.16.1.0 2
ip route 20.42.0.0 255.255.0.0 172.16.1.7
ip route 20.42.0.0 255.255.0.0 172.16.1.0 2
ip route 30.1.0.0 255.255.0.0 172.16.1.0
ip route 30.1.0.0 255.255.0.0 172.16.1.7 2
ip route 100.1.0.0 255.255.0.0 172.16.1.0
ip route 100.1.0.0 255.255.0.0 172.16.1.7 2
ip route 100.1.1.0 255.255.255.0 5.20.25.3
ip route 100.1.1.0 255.255.255.0 172.16.1.7 2
ip route 100.1.1.0 255.255.255.0 172.16.1.0 3
ip route 100.1.2.0 255.255.255.0 5.20.25.1
ip route 100.1.10.16 255.255.255.240 5.20.25.3
ip route 100.1.10.16 255.255.255.240 172.16.1.7 2
ip route 100.1.10.16 255.255.255.240 172.16.1.0 3
ip route 100.1.20.16 255.255.255.240 5.20.25.3
ip route 100.1.20.16 255.255.255.240 172.16.1.7 2
ip route 100.1.20.16 255.255.255.240 172.16.1.0 3
ip route 101.0.0.0 255.255.0.0 172.16.1.0
ip route 101.0.0.0 255.255.0.0 172.16.1.7 2
!
ipv6 route 2001:DB8:101::/48 Ethernet0/0 FE80::23
ipv6 route 2001:DB8:101::/48 Ethernet0/2 FE80::26 2
ipv6 route 2001:DB8:301::/48 Ethernet0/0 FE80::23
ipv6 route 2001:DB8:301::/48 Ethernet0/2 FE80::26 2
ipv6 route 2001:DB8:520::23/128 Ethernet0/0 FE80::23
ipv6 route 2001:DB8:520::23/128 Ethernet0/2 FE80::26 2
ipv6 route 2001:DB8:520::24/128 Ethernet0/0 FE80::23
ipv6 route 2001:DB8:520::24/128 Ethernet0/2 FE80::26 2
ipv6 route 2001:DB8:520::26/128 Ethernet0/2 FE80::26
ipv6 route 2001:DB8:520::26/128 Ethernet0/0 FE80::23 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/0 FE80::23 3
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/2 FE80::26 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/3 FE80::28
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/1 FE80::27
ipv6 route 2001:DB8:1001::/48 Ethernet0/0 FE80::23
ipv6 route 2001:DB8:1001::/48 Ethernet0/2 FE80::26 2
ipv6 route 2001:DB8:2042::/48 Ethernet0/2 FE80::26
ipv6 route 2001:DB8:2042::/48 Ethernet0/0 FE80::23 2
!
</pre>
</details>

<details>
  <summary>R26</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 5.20.0.26 255.255.255.255
 ipv6 address 2001:DB8:520::26/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.5 255.255.255.254
 ipv6 address FE80::26 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 5.20.26.0 255.255.255.254
 ipv6 address FE80::26 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.7 255.255.255.254
 ipv6 address FE80::26 link-local
!
ip route 5.20.0.23 255.255.255.255 172.16.1.4
ip route 5.20.0.23 255.255.255.255 172.16.1.6 2
ip route 5.20.0.24 255.255.255.255 172.16.1.4
ip route 5.20.0.24 255.255.255.255 172.16.1.6 2
ip route 5.20.0.25 255.255.255.255 172.16.1.6
ip route 5.20.0.25 255.255.255.255 172.16.1.4 2
ip route 20.42.0.0 255.255.0.0 5.20.26.3
ip route 20.42.0.0 255.255.0.0 172.16.1.4 2
ip route 20.42.0.0 255.255.0.0 172.16.1.6 3
ip route 30.1.0.0 255.255.0.0 172.16.1.4
ip route 30.1.0.0 255.255.0.0 172.16.1.6 2
ip route 100.1.0.0 255.255.0.0 172.16.1.4
ip route 100.1.0.0 255.255.0.0 172.16.1.6 2
ip route 100.1.1.0 255.255.255.0 5.20.26.1
ip route 100.1.1.0 255.255.255.0 172.16.1.6 2
ip route 100.1.1.0 255.255.255.0 172.16.1.4 3
ip route 100.1.2.0 255.255.255.0 172.16.1.6
ip route 100.1.2.0 255.255.255.0 172.16.1.4 2
ip route 100.1.10.16 255.255.255.240 5.20.26.1
ip route 100.1.10.16 255.255.255.240 172.16.1.6 2
ip route 100.1.10.16 255.255.255.240 172.16.1.4 3
ip route 100.1.20.16 255.255.255.240 5.20.26.1
ip route 100.1.20.16 255.255.255.240 172.16.1.6 2
ip route 100.1.20.16 255.255.255.240 172.16.1.4 3
ip route 101.0.0.0 255.255.0.0 172.16.1.4
ip route 101.0.0.0 255.255.0.0 172.16.1.6 2
!
ipv6 route 2001:DB8:101::/48 Ethernet0/0 FE80::24
ipv6 route 2001:DB8:101::/48 Ethernet0/2 FE80::25 2
ipv6 route 2001:DB8:301::/48 Ethernet0/0 FE80::24
ipv6 route 2001:DB8:301::/48 Ethernet0/2 FE80::25 2
ipv6 route 2001:DB8:520::23/128 Ethernet0/0 FE80::24
ipv6 route 2001:DB8:520::23/128 Ethernet0/2 FE80::25 2
ipv6 route 2001:DB8:520::24/128 Ethernet0/0 FE80::24
ipv6 route 2001:DB8:520::24/128 Ethernet0/2 FE80::25 2
ipv6 route 2001:DB8:520::25/128 Ethernet0/2 FE80::25
ipv6 route 2001:DB8:520::25/128 Ethernet0/0 FE80::24 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/0 FE80::24 3
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/2 FE80::25 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/1 FE80::28
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/0 FE80::24 2
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/2 FE80::25
ipv6 route 2001:DB8:1001::/48 Ethernet0/0 FE80::24
ipv6 route 2001:DB8:1001::/48 Ethernet0/2 FE80::25 2
ipv6 route 2001:DB8:2042::/48 Ethernet0/3 FE80::18
ipv6 route 2001:DB8:2042::/48 Ethernet0/0 FE80::24 2
ipv6 route 2001:DB8:2042::/48 Ethernet0/2 FE80::25 3
!
</pre>
</details>
<hr>

AS101 (Киторн)

| Device  | Interface                   | IPv4 Address                                                   | IPv6 Address                                                                              |
|---------|-----------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **R22** | Lo1<br>e0/0<br>e0/1<br>e0/2 | 30.1.0.21/32<br>30.1.100.0/31<br>172.16.1.0/31<br>5.20.24.1/31 | 2001:DB8:301::21/128<br>FE80::21 link-local<br>FE80::21 link-local<br>FE80::21 link-local |
<details>
  <summary>R22</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 30.1.0.21 255.255.255.255
 ipv6 address 2001:DB8:301::21/128
!
interface Ethernet0/0
 no shutdown
 ip address 30.1.100.0 255.255.255.254
 ipv6 address FE80::21 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.0 255.255.255.254
 ipv6 address FE80::21 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 5.20.24.1 255.255.255.254
 ipv6 address FE80::21 link-local
!
ip route 0.0.0.0 0.0.0.0 5.20.24.0
ip route 0.0.0.0 0.0.0.0 172.16.1.1 2
ip route 100.1.0.0 255.255.0.0 30.1.100.1
ip route 100.1.0.0 255.255.0.0 172.16.1.1 2
ip route 100.1.1.0 255.255.255.0 5.20.24.0
ip route 100.1.1.0 255.255.255.0 172.16.1.1 2
ip route 100.1.2.0 255.255.255.0 5.20.24.0
ip route 100.1.2.0 255.255.255.0 172.16.1.1 2
ip route 100.1.10.16 255.255.255.240 5.20.24.0
ip route 100.1.10.16 255.255.255.240 172.16.1.1 2
ip route 100.1.20.16 255.255.255.240 5.20.24.0
ip route 100.1.20.16 255.255.255.240 172.16.1.1 2
ip route 101.0.0.0 255.255.0.0 172.16.1.1
!
ipv6 route 2001:DB8:101::/48 Ethernet0/1 FE80::22
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/1 FE80::22 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/2 FE80::24
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/1 FE80::22 2
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/2 FE80::24
ipv6 route 2001:DB8:1001::/48 Ethernet0/0 FE80::15
ipv6 route 2001:DB8:1001::/48 Ethernet0/1 FE80::22 2
ipv6 route ::/0 Ethernet0/2 FE80::24
ipv6 route ::/0 Ethernet0/1 FE80::22 2
!
</pre>
</details>
<hr>

AS301 (Ламас)

| Device  | Interface                   | IPv4 Address                                                    | IPv6 Address                                                                              |
|---------|-----------------------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **R21** | Lo1<br>e0/0<br>e0/1<br>e0/2 | 101.0.0.22/32<br>101.0.100.0/31<br>172.16.1.1/31<br>5.20.23.1/31 | 2001:DB8:101::22/128<br>FE80::22 link-local<br>FE80::22 link-local<br>FE80::22 link-local |
<details>
  <summary>R21</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 101.0.0.22 255.255.255.255
 ipv6 address 2001:DB8:101::22/128
!
interface Ethernet0/0
 no shutdown
 ip address 101.0.100.0 255.255.255.254
 ipv6 address FE80::22 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.1 255.255.255.254
 ipv6 address FE80::22 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 5.20.23.1 255.255.255.254
 ipv6 address FE80::22 link-local
!
ip route 0.0.0.0 0.0.0.0 5.20.23.0
ip route 0.0.0.0 0.0.0.0 172.16.1.0 2
ip route 30.1.0.0 255.255.0.0 172.16.1.0
ip route 100.1.0.0 255.255.0.0 101.0.100.1
ip route 100.1.0.0 255.255.0.0 172.16.1.0 2
ip route 100.1.1.0 255.255.255.0 5.20.23.0
ip route 100.1.1.0 255.255.255.0 172.16.1.0 2
ip route 100.1.2.0 255.255.255.0 5.20.23.0
ip route 100.1.2.0 255.255.255.0 172.16.1.0 2
ip route 100.1.10.16 255.255.255.240 5.20.23.0
ip route 100.1.10.16 255.255.255.240 172.16.1.0 2
ip route 100.1.20.16 255.255.255.240 5.20.23.0
ip route 100.1.20.16 255.255.255.240 172.16.1.0 2
!
ipv6 route 2001:DB8:301::/48 Ethernet0/1 FE80::21
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/1 FE80::21 2
ipv6 route 2001:DB8:1001:A00::/56 Ethernet0/2 FE80::23
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/1 FE80::21 2
ipv6 route 2001:DB8:1001:B00::/56 Ethernet0/2 FE80::23
ipv6 route 2001:DB8:1001::/48 Ethernet0/0 FE80::14
ipv6 route 2001:DB8:1001::/48 Ethernet0/1 FE80::21 2
ipv6 route ::/0 Ethernet0/2 FE80::23
ipv6 route ::/0 Ethernet0/1 FE80::21 2
!
</pre>
</details>
<hr>

AS1001 Москва

| Device | Interface                                      | IPv4 Address                                                                                           | IPv6 Address                                                                                                                       |
|--------|------------------------------------------------|--------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
|**VPC1**| eth0                                           | 100.1.10.2/28 gw 100.1.10.1                                                                            | 2001:DB8:1001:10::/64 (SLAAC)                                                                                                      |
|**VPC7**| eth0                                           | 100.1.20.2/28 gw 100.1.20.1                                                                            | 2001:DB8:1001:20::/64 (SLAAC)                                                                                                      |
| **SW2**| Lo1<br>e0/0<br>e0/1<br>vlan20                  | 100.1.0.2/32<br>172.16.1.27/31<br>172.16.1.23/31<br>100.1.20.1/28                                      | 2001:DB8:1001::2/128<br>FE80::2 link-local<br>FE80::2 link-local<br>2001:DB8:1001:20::1/64                                         |
| **SW3**| Lo1<br>e0/0<br>e0/1<br>vlan10                  | 100.1.0.3/32<br>172.16.1.21/31<br>172.16.1.29/31<br>100.1.10.1/28                                      | 2001:DB8:1001::3/128<br>FE80::3 link-local<br>FE80::3 link-local<br>2001:DB8:1001:10::1/64                                         |
| **SW4**| Lo1<br>e0/0<br>e0/1<br>e1/0<br>e1/1<br>vlan201 | 100.1.0.4/32<br>172.16.1.20/31<br>172.16.1.22/31<br>172.16.1.13/31<br>172.16.1.19/31<br>172.16.1.24/31 | 2001:DB8:1001::4/128<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local |
| **SW5**| Lo1<br>e0/0<br>e0/1<br>e1/0<br>e1/1<br>vlan201 | 100.1.0.5/32<br>172.16.1.26/31<br>172.16.1.28/31<br>172.16.1.17/31<br>172.16.1.15/31<br>172.16.1.25/31 | 2001:DB8:1001::5/128<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local |
| **R12**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.12/32<br>172.16.1.12/31<br>172.16.1.14/31<br>172.16.1.1/31<br>172.16.1.9/31                    | 2001:DB8:1001::12/128<br>FE80::12 link-local<br>FE80::12 link-local<br>FE80::12 link-local<br>FE80::12 link-local                  |
| **R13**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.13/32<br>172.16.1.16/31<br>172.16.1.18/31<br>172.16.1.7/31<br>172.16.1.3/31                    | 2001:DB8:1001::13/128<br>FE80::13 link-local<br>FE80::13 link-local<br>FE80::13 link-local<br>FE80::13 link-local                  |
| **R14**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.14/32<br>172.16.1.0/31<br>172.16.1.2/31<br>101.0.100.1/31<br>172.16.1.4/31                     | 2001:DB8:1001::14/128<br>FE80::14 link-local<br>FE80::14 link-local<br>FE80::14 link-local<br>FE80::14 link-local                  |
| **R15**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.15/32<br>172.16.1.6/31<br>172.16.1.8/31<br>30.1.100.1/31<br>172.16.1.10/31                     | 2001:DB8:1001::15/128<br>FE80::15 link-local<br>FE80::15 link-local<br>FE80::15 link-local<br>FE80::15 link-local                  |
| **R19**| Lo1<br>e0/0                                    | 100.1.0.19/32<br>172.16.1.5/31                                                                         | 2001:DB8:1001::19/128<br>FE80::19 link-local                                                                                       |
| **R20**| Lo1<br>e0/0                                    | 100.1.0.20/32<br>172.16.1.11/31                                                                        | 2001:DB8:1001::20/128<br>FE80::20 link-local                                                                                       |
<details>
  <summary>SW2</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
vlan 20
!
interface Loopback1
 no shutdown
 ip address 100.1.0.2 255.255.255.255
 ipv6 address 2001:DB8:1001::2/128
!
interface Ethernet0/0
 no shutdown
 no switchport
 ip address 172.16.1.27 255.255.255.254
 duplex auto
 ipv6 address FE80::2 link-local
!
interface Ethernet0/1
 no shutdown
 no switchport
 ip address 172.16.1.23 255.255.255.254
 duplex auto
 ipv6 address FE80::2 link-local
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 20
 switchport mode access
!
interface Vlan20
 no shutdown
 ip address 100.1.20.1 255.255.255.240
 ipv6 address 2001:DB8:1001:20::1/64
 ipv6 nd cache expire 30
 arp timeout 30
!
ip route 0.0.0.0 0.0.0.0 172.16.1.26
ip route 0.0.0.0 0.0.0.0 172.16.1.22 2
ip route 100.1.0.3 255.255.255.255 172.16.1.22
ip route 100.1.0.4 255.255.255.255 172.16.1.22
ip route 100.1.0.12 255.255.255.255 172.16.1.22
ip route 100.1.0.14 255.255.255.255 172.16.1.22
ip route 100.1.0.19 255.255.255.255 172.16.1.22
ip route 100.1.10.0 255.255.255.240 172.16.1.22
!
ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::4
ipv6 route ::/0 Ethernet0/1 FE80::4 2
ipv6 route ::/0 Ethernet0/0 FE80::5
!
</pre>
</details>
<details>
  <summary>SW3</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
vlan 10
!
interface Loopback1
 no shutdown
 ip address 100.1.0.3 255.255.255.255
 ipv6 address 2001:DB8:1001::3/128
!
interface Ethernet0/0
 no shutdown
 no switchport
 ip address 172.16.1.21 255.255.255.254
 duplex auto
 ipv6 address FE80::3 link-local
!
interface Ethernet0/1
 no shutdown
 no switchport
 ip address 172.16.1.29 255.255.255.254
 duplex auto
 ipv6 address FE80::3 link-local
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 10
 switchport mode access
!
interface Vlan10
 no shutdown
 ip address 100.1.10.1 255.255.255.240
 ipv6 address 2001:DB8:1001:10::1/64
 ipv6 nd cache expire 30
 arp timeout 30
!
ip route 0.0.0.0 0.0.0.0 172.16.1.20
ip route 0.0.0.0 0.0.0.0 172.16.1.28 2
ip route 100.1.0.2 255.255.255.255 172.16.1.28
ip route 100.1.0.5 255.255.255.255 172.16.1.28
ip route 100.1.0.13 255.255.255.255 172.16.1.28
ip route 100.1.0.15 255.255.255.255 172.16.1.28
ip route 100.1.0.20 255.255.255.255 172.16.1.28
ip route 100.1.20.0 255.255.255.240 172.16.1.28
!
ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::5
ipv6 route ::/0 Ethernet0/0 FE80::4
ipv6 route ::/0 Ethernet0/1 FE80::5 2
!
</pre>
</details>
<details>
  <summary>SW4</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
vlan 201
 name po1-vlan
!
vlan 1000
!
interface Loopback1
 no shutdown
 ip address 100.1.0.4 255.255.255.255
 ipv6 address 2001:DB8:1001::4/128
!
interface Port-channel1
 no shutdown
 switchport trunk allowed vlan 201,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
!
interface Ethernet0/0
 no shutdown
 no switchport
 ip address 172.16.1.20 255.255.255.254
 duplex auto
 ipv6 address FE80::4 link-local
!
interface Ethernet0/1
 no shutdown
 no switchport
 ip address 172.16.1.22 255.255.255.254
 duplex auto
 ipv6 address FE80::4 link-local
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 201,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 201,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet1/0
 no shutdown
 no switchport
 ip address 172.16.1.13 255.255.255.254
 duplex auto
 ipv6 address FE80::4 link-local
!
interface Ethernet1/1
 no shutdown
 no switchport
 ip address 172.16.1.19 255.255.255.254
 duplex auto
 ipv6 address FE80::4 link-local
!
interface Vlan201
 no shutdown
 ip address 172.16.1.24 255.255.255.254
 ipv6 address FE80::4 link-local
 ipv6 nd cache expire 30
 arp timeout 30
!
ip route 0.0.0.0 0.0.0.0 172.16.1.12
ip route 0.0.0.0 0.0.0.0 172.16.1.18 2
ip route 0.0.0.0 0.0.0.0 172.16.1.25 3
ip route 0.0.0.0 0.0.0.0 172.16.1.23 4
ip route 0.0.0.0 0.0.0.0 172.16.1.21 5
ip route 100.1.0.2 255.255.255.255 172.16.1.23
ip route 100.1.0.2 255.255.255.255 172.16.1.25 2
ip route 100.1.0.2 255.255.255.255 172.16.1.18 3
ip route 100.1.0.2 255.255.255.255 172.16.1.12 4
ip route 100.1.0.2 255.255.255.255 172.16.1.21 5
ip route 100.1.0.3 255.255.255.255 172.16.1.21
ip route 100.1.0.3 255.255.255.255 172.16.1.25 2
ip route 100.1.0.3 255.255.255.255 172.16.1.18 3
ip route 100.1.0.3 255.255.255.255 172.16.1.12 4
ip route 100.1.0.3 255.255.255.255 172.16.1.23 5
ip route 100.1.0.5 255.255.255.255 172.16.1.25
ip route 100.1.0.5 255.255.255.255 172.16.1.18 2
ip route 100.1.0.5 255.255.255.255 172.16.1.23 3
ip route 100.1.0.5 255.255.255.255 172.16.1.12 4
ip route 100.1.0.5 255.255.255.255 172.16.1.21 5
ip route 100.1.0.12 255.255.255.255 172.16.1.12
ip route 100.1.0.12 255.255.255.255 172.16.1.25 2
ip route 100.1.0.12 255.255.255.255 172.16.1.18 3
ip route 100.1.0.12 255.255.255.255 172.16.1.23 4
ip route 100.1.0.12 255.255.255.255 172.16.1.21 5
ip route 100.1.0.13 255.255.255.255 172.16.1.18
ip route 100.1.0.13 255.255.255.255 172.16.1.25 2
ip route 100.1.0.13 255.255.255.255 172.16.1.23 3
ip route 100.1.0.13 255.255.255.255 172.16.1.12 4
ip route 100.1.0.13 255.255.255.255 172.16.1.21 5
ip route 100.1.0.15 255.255.255.255 172.16.1.18
ip route 100.1.0.15 255.255.255.255 172.16.1.25 2
ip route 100.1.0.15 255.255.255.255 172.16.1.12 3
ip route 100.1.0.15 255.255.255.255 172.16.1.23 4
ip route 100.1.0.15 255.255.255.255 172.16.1.21 5
ip route 100.1.0.20 255.255.255.255 172.16.1.18
ip route 100.1.0.20 255.255.255.255 172.16.1.25 2
ip route 100.1.0.20 255.255.255.255 172.16.1.12 3
ip route 100.1.0.20 255.255.255.255 172.16.1.23 4
ip route 100.1.0.20 255.255.255.255 172.16.1.21 5
ip route 100.1.10.0 255.255.255.240 172.16.1.21
ip route 100.1.10.0 255.255.255.240 172.16.1.25 2
ip route 100.1.10.0 255.255.255.240 172.16.1.18 3
ip route 100.1.10.0 255.255.255.240 172.16.1.12 4
ip route 100.1.10.0 255.255.255.240 172.16.1.23 5
ip route 100.1.20.0 255.255.255.240 172.16.1.23
ip route 100.1.20.0 255.255.255.240 172.16.1.25 2
ip route 100.1.20.0 255.255.255.240 172.16.1.18 3
ip route 100.1.20.0 255.255.255.240 172.16.1.12 4
ip route 100.1.20.0 255.255.255.240 172.16.1.21 5
!
ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::2
ipv6 route 2001:DB8:1001::2/128 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001::2/128 Ethernet1/1 FE80::13 3
ipv6 route 2001:DB8:1001::2/128 Ethernet1/0 FE80::12 4
ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::3 5
ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::3
ipv6 route 2001:DB8:1001::3/128 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001::3/128 Ethernet1/1 FE80::13 3
ipv6 route 2001:DB8:1001::3/128 Ethernet1/0 FE80::12 4
ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::2 5
ipv6 route 2001:DB8:1001::5/128 Vlan201 FE80::5
ipv6 route 2001:DB8:1001::5/128 Ethernet1/1 FE80::13 2
ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::2 3
ipv6 route 2001:DB8:1001::5/128 Ethernet1/0 FE80::12 4
ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::3 5
ipv6 route 2001:DB8:1001::12/128 Ethernet1/0 FE80::12
ipv6 route 2001:DB8:1001::12/128 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001::12/128 Ethernet1/1 FE80::13 3
ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::2 4
ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::3 5
ipv6 route 2001:DB8:1001::13/128 Ethernet1/1 FE80::13
ipv6 route 2001:DB8:1001::13/128 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::2 3
ipv6 route 2001:DB8:1001::13/128 Ethernet1/0 FE80::12 4
ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::3 5
ipv6 route 2001:DB8:1001::15/128 Ethernet1/1 FE80::13
ipv6 route 2001:DB8:1001::15/128 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001::15/128 Ethernet1/0 FE80::12 3
ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::2 4
ipv6 route 2001:DB8:1001::15/128 Ethernet0/0 FE80::3 5
ipv6 route 2001:DB8:1001::20/128 Ethernet1/1 FE80::13
ipv6 route 2001:DB8:1001::20/128 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001::20/128 Ethernet1/0 FE80::12 3
ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::2 4
ipv6 route 2001:DB8:1001::20/128 Ethernet0/0 FE80::3 5
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::3
ipv6 route 2001:DB8:1001:10::/64 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001:10::/64 Ethernet1/1 FE80::13 3
ipv6 route 2001:DB8:1001:10::/64 Ethernet1/0 FE80::12 4
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::2 5
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::2
ipv6 route 2001:DB8:1001:20::/64 Vlan201 FE80::5 2
ipv6 route 2001:DB8:1001:20::/64 Ethernet1/1 FE80::13 3
ipv6 route 2001:DB8:1001:20::/64 Ethernet1/0 FE80::12 4
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::3 5
ipv6 route ::/0 Ethernet1/0 FE80::12
ipv6 route ::/0 Ethernet1/1 FE80::13 2
ipv6 route ::/0 Vlan201 FE80::5 3
ipv6 route ::/0 Ethernet0/1 FE80::2 4
ipv6 route ::/0 Ethernet0/0 FE80::3 5
!
</pre>
</details>
<details>
  <summary>SW5</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
vlan 201
 name po1-vlan
!
vlan 1000
!
interface Loopback1
 no shutdown
 ip address 100.1.0.5 255.255.255.255
 ipv6 address 2001:DB8:1001::5/128
!
interface Port-channel1
 no shutdown
 switchport trunk allowed vlan 201,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
!
interface Ethernet0/0
 no shutdown
 no switchport
 ip address 172.16.1.26 255.255.255.254
 duplex auto
 ipv6 address FE80::5 link-local
!
interface Ethernet0/1
 no shutdown
 no switchport
 ip address 172.16.1.28 255.255.255.254
 duplex auto
 ipv6 address FE80::5 link-local
!
interface Ethernet0/2
 no shutdown
 switchport trunk allowed vlan 201,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 201,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet1/0
 no shutdown
 no switchport
 ip address 172.16.1.17 255.255.255.254
 duplex auto
 ipv6 address FE80::5 link-local
!
interface Ethernet1/1
 no shutdown
 no switchport
 ip address 172.16.1.15 255.255.255.254
 duplex auto
 ipv6 address FE80::5 link-local
!
interface Vlan201
 no shutdown
 ip address 172.16.1.25 255.255.255.254
 ipv6 address FE80::5 link-local
 ipv6 nd cache expire 30
 arp timeout 30
!
ip route 0.0.0.0 0.0.0.0 172.16.1.16
ip route 0.0.0.0 0.0.0.0 172.16.1.14 2
ip route 0.0.0.0 0.0.0.0 172.16.1.24 3
ip route 0.0.0.0 0.0.0.0 172.16.1.29 4
ip route 0.0.0.0 0.0.0.0 172.16.1.27 5
ip route 100.1.0.2 255.255.255.255 172.16.1.27
ip route 100.1.0.2 255.255.255.255 172.16.1.24 2
ip route 100.1.0.2 255.255.255.255 172.16.1.14 3
ip route 100.1.0.2 255.255.255.255 172.16.1.16 4
ip route 100.1.0.2 255.255.255.255 172.16.1.29 5
ip route 100.1.0.3 255.255.255.255 172.16.1.29
ip route 100.1.0.3 255.255.255.255 172.16.1.24 2
ip route 100.1.0.3 255.255.255.255 172.16.1.14 3
ip route 100.1.0.3 255.255.255.255 172.16.1.16 4
ip route 100.1.0.3 255.255.255.255 172.16.1.27 5
ip route 100.1.0.4 255.255.255.255 172.16.1.24
ip route 100.1.0.4 255.255.255.255 172.16.1.14 2
ip route 100.1.0.4 255.255.255.255 172.16.1.29 3
ip route 100.1.0.4 255.255.255.255 172.16.1.16 4
ip route 100.1.0.4 255.255.255.255 172.16.1.27 5
ip route 100.1.0.12 255.255.255.255 172.16.1.14
ip route 100.1.0.12 255.255.255.255 172.16.1.24 2
ip route 100.1.0.12 255.255.255.255 172.16.1.29 3
ip route 100.1.0.12 255.255.255.255 172.16.1.16 4
ip route 100.1.0.12 255.255.255.255 172.16.1.27 5
ip route 100.1.0.13 255.255.255.255 172.16.1.16
ip route 100.1.0.13 255.255.255.255 172.16.1.24 2
ip route 100.1.0.13 255.255.255.255 172.16.1.14 3
ip route 100.1.0.13 255.255.255.255 172.16.1.29 4
ip route 100.1.0.13 255.255.255.255 172.16.1.27 5
ip route 100.1.0.14 255.255.255.255 172.16.1.14
ip route 100.1.0.14 255.255.255.255 172.16.1.24 2
ip route 100.1.0.14 255.255.255.255 172.16.1.16 3
ip route 100.1.0.14 255.255.255.255 172.16.1.29 4
ip route 100.1.0.14 255.255.255.255 172.16.1.27 5
ip route 100.1.0.19 255.255.255.255 172.16.1.14
ip route 100.1.0.19 255.255.255.255 172.16.1.24 2
ip route 100.1.0.19 255.255.255.255 172.16.1.16 3
ip route 100.1.0.19 255.255.255.255 172.16.1.29 4
ip route 100.1.0.19 255.255.255.255 172.16.1.27 5
ip route 100.1.10.0 255.255.255.240 172.16.1.29
ip route 100.1.10.0 255.255.255.240 172.16.1.24 2
ip route 100.1.10.0 255.255.255.240 172.16.1.14 3
ip route 100.1.10.0 255.255.255.240 172.16.1.16 4
ip route 100.1.10.0 255.255.255.240 172.16.1.27 5
ip route 100.1.20.0 255.255.255.240 172.16.1.27
ip route 100.1.20.0 255.255.255.240 172.16.1.24 2
ip route 100.1.20.0 255.255.255.240 172.16.1.14 3
ip route 100.1.20.0 255.255.255.240 172.16.1.16 4
ip route 100.1.20.0 255.255.255.240 172.16.1.29 5
!
ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::2
ipv6 route 2001:DB8:1001::2/128 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001::2/128 Ethernet1/1 FE80::12 3
ipv6 route 2001:DB8:1001::2/128 Ethernet1/0 FE80::13 4
ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::3 5
ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::3
ipv6 route 2001:DB8:1001::3/128 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001::3/128 Ethernet1/1 FE80::12 3
ipv6 route 2001:DB8:1001::3/128 Ethernet1/0 FE80::13 4
ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::2 5
ipv6 route 2001:DB8:1001::4/128 Vlan201 FE80::4
ipv6 route 2001:DB8:1001::4/128 Ethernet1/1 FE80::12 2
ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::3 3
ipv6 route 2001:DB8:1001::4/128 Ethernet1/0 FE80::13 4
ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::2 5
ipv6 route 2001:DB8:1001::12/128 Ethernet1/1 FE80::12
ipv6 route 2001:DB8:1001::12/128 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::3 3
ipv6 route 2001:DB8:1001::12/128 Ethernet1/0 FE80::13 4
ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::2 5
ipv6 route 2001:DB8:1001::13/128 Ethernet1/0 FE80::13
ipv6 route 2001:DB8:1001::13/128 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001::13/128 Ethernet1/1 FE80::12 3
ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::3 4
ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::2 5
ipv6 route 2001:DB8:1001::14/128 Ethernet1/1 FE80::12
ipv6 route 2001:DB8:1001::14/128 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001::14/128 Ethernet1/0 FE80::13 3
ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::3 4
ipv6 route 2001:DB8:1001::14/128 Ethernet0/0 FE80::2 5
ipv6 route 2001:DB8:1001::19/128 Ethernet1/1 FE80::12
ipv6 route 2001:DB8:1001::19/128 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001::19/128 Ethernet1/0 FE80::13 3
ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::3 4
ipv6 route 2001:DB8:1001::19/128 Ethernet0/0 FE80::2 5
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::3
ipv6 route 2001:DB8:1001:10::/64 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001:10::/64 Ethernet1/1 FE80::12 3
ipv6 route 2001:DB8:1001:10::/64 Ethernet1/0 FE80::13 4
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::2 5
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::2
ipv6 route 2001:DB8:1001:20::/64 Vlan201 FE80::4 2
ipv6 route 2001:DB8:1001:20::/64 Ethernet1/1 FE80::12 3
ipv6 route 2001:DB8:1001:20::/64 Ethernet1/0 FE80::13 4
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::3 5
ipv6 route ::/0 Ethernet1/0 FE80::13
ipv6 route ::/0 Ethernet1/1 FE80::12 2
ipv6 route ::/0 Vlan201 FE80::4 3
ipv6 route ::/0 Ethernet0/1 FE80::3 4
ipv6 route ::/0 Ethernet0/0 FE80::2 5
!
</pre>
</details>
<details>
  <summary>R12</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.0.12 255.255.255.255
 ipv6 address 2001:DB8:1001::12/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.12 255.255.255.254
 ipv6 address FE80::12 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.14 255.255.255.254
 ipv6 address FE80::12 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.1 255.255.255.254
 ipv6 address FE80::12 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 172.16.1.9 255.255.255.254
 ipv6 address FE80::12 link-local
!
ip route 0.0.0.0 0.0.0.0 172.16.1.0
ip route 0.0.0.0 0.0.0.0 172.16.1.8 2
ip route 0.0.0.0 0.0.0.0 172.16.1.15 3
ip route 0.0.0.0 0.0.0.0 172.16.1.13 4
ip route 100.1.0.2 255.255.255.255 172.16.1.15
ip route 100.1.0.2 255.255.255.255 172.16.1.13 2
ip route 100.1.0.2 255.255.255.255 172.16.1.8 3
ip route 100.1.0.3 255.255.255.255 172.16.1.13
ip route 100.1.0.3 255.255.255.255 172.16.1.15 2
ip route 100.1.0.3 255.255.255.255 172.16.1.8 3
ip route 100.1.0.4 255.255.255.255 172.16.1.13
ip route 100.1.0.4 255.255.255.255 172.16.1.15 2
ip route 100.1.0.4 255.255.255.255 172.16.1.8 3
ip route 100.1.0.5 255.255.255.255 172.16.1.15
ip route 100.1.0.5 255.255.255.255 172.16.1.13 2
ip route 100.1.0.5 255.255.255.255 172.16.1.8 3
ip route 100.1.0.13 255.255.255.255 172.16.1.8 2
ip route 100.1.0.13 255.255.255.255 172.16.1.15 3
ip route 100.1.0.13 255.255.255.255 172.16.1.13 4
ip route 100.1.0.15 255.255.255.255 172.16.1.8
ip route 100.1.0.15 255.255.255.255 172.16.1.0 2
ip route 100.1.0.15 255.255.255.255 172.16.1.15 3
ip route 100.1.0.15 255.255.255.255 172.16.1.13 4
ip route 100.1.0.20 255.255.255.255 172.16.1.8
ip route 100.1.0.20 255.255.255.255 172.16.1.0 2
ip route 100.1.0.20 255.255.255.255 172.16.1.15 3
ip route 100.1.0.20 255.255.255.255 172.16.1.13 4
ip route 100.1.10.0 255.255.255.240 172.16.1.13
ip route 100.1.10.0 255.255.255.240 172.16.1.15 2
ip route 100.1.10.0 255.255.255.240 172.16.1.8 3
ip route 100.1.20.0 255.255.255.240 172.16.1.15
ip route 100.1.20.0 255.255.255.240 172.16.1.13 2
ip route 100.1.20.0 255.255.255.240 172.16.1.8 3
!
ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::4 2
ipv6 route 2001:DB8:1001::2/128 Ethernet0/3 FE80::15 3
ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::4
ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::5 2
ipv6 route 2001:DB8:1001::3/128 Ethernet0/3 FE80::15 3
ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::4
ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::5 2
ipv6 route 2001:DB8:1001::4/128 Ethernet0/3 FE80::15 3
ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::4 2
ipv6 route 2001:DB8:1001::5/128 Ethernet0/3 FE80::15 3
ipv6 route 2001:DB8:1001::13/128 Ethernet0/3 FE80::15 2
ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::5 3
ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::4 4
ipv6 route 2001:DB8:1001::15/128 Ethernet0/3 FE80::15
ipv6 route 2001:DB8:1001::15/128 Ethernet0/2 FE80::14 2
ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::5 3
ipv6 route 2001:DB8:1001::15/128 Ethernet0/0 FE80::4 4
ipv6 route 2001:DB8:1001::20/128 Ethernet0/3 FE80::15
ipv6 route 2001:DB8:1001::20/128 Ethernet0/2 FE80::14 2
ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::5 3
ipv6 route 2001:DB8:1001::20/128 Ethernet0/0 FE80::4 4
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::4
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::5 2
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/3 FE80::15 3
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::5
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::4 2
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/3 FE80::15 3
ipv6 route ::/0 Ethernet0/2 FE80::14
ipv6 route ::/0 Ethernet0/3 FE80::15 2
ipv6 route ::/0 Ethernet0/1 FE80::5 3
ipv6 route ::/0 Ethernet0/0 FE80::4 4
!
</pre>
</details>
<details>
  <summary>R13</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.0.13 255.255.255.255
 ipv6 address 2001:DB8:1001::13/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.16 255.255.255.254
 ipv6 address FE80::13 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.18 255.255.255.254
 ipv6 address FE80::13 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.7 255.255.255.254
 ipv6 address FE80::13 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 172.16.1.3 255.255.255.254
 ipv6 address FE80::13 link-local
!
ip route 0.0.0.0 0.0.0.0 172.16.1.6
ip route 0.0.0.0 0.0.0.0 172.16.1.2 2
ip route 0.0.0.0 0.0.0.0 172.16.1.19 3
ip route 0.0.0.0 0.0.0.0 172.16.1.17 4
ip route 100.1.0.2 255.255.255.255 172.16.1.17
ip route 100.1.0.2 255.255.255.255 172.16.1.19 2
ip route 100.1.0.2 255.255.255.255 172.16.1.2 3
ip route 100.1.0.3 255.255.255.255 172.16.1.19
ip route 100.1.0.3 255.255.255.255 172.16.1.17 2
ip route 100.1.0.3 255.255.255.255 172.16.1.2 3
ip route 100.1.0.4 255.255.255.255 172.16.1.19
ip route 100.1.0.4 255.255.255.255 172.16.1.17 2
ip route 100.1.0.4 255.255.255.255 172.16.1.2 3
ip route 100.1.0.5 255.255.255.255 172.16.1.17
ip route 100.1.0.5 255.255.255.255 172.16.1.19 2
ip route 100.1.0.5 255.255.255.255 172.16.1.2 3
ip route 100.1.0.12 255.255.255.255 172.16.1.2
ip route 100.1.0.12 255.255.255.255 172.16.1.6 2
ip route 100.1.0.12 255.255.255.255 172.16.1.17 3
ip route 100.1.0.12 255.255.255.255 172.16.1.19 4
ip route 100.1.0.14 255.255.255.255 172.16.1.2
ip route 100.1.0.14 255.255.255.255 172.16.1.6 2
ip route 100.1.0.14 255.255.255.255 172.16.1.17 3
ip route 100.1.0.14 255.255.255.255 172.16.1.19 4
ip route 100.1.0.19 255.255.255.255 172.16.1.2
ip route 100.1.0.19 255.255.255.255 172.16.1.6 2
ip route 100.1.0.19 255.255.255.255 172.16.1.17 3
ip route 100.1.0.19 255.255.255.255 172.16.1.19 4
ip route 100.1.10.0 255.255.255.240 172.16.1.19
ip route 100.1.10.0 255.255.255.240 172.16.1.17 2
ip route 100.1.10.0 255.255.255.240 172.16.1.2 3
ip route 100.1.20.0 255.255.255.240 172.16.1.17
ip route 100.1.20.0 255.255.255.240 172.16.1.19 2
ip route 100.1.20.0 255.255.255.240 172.16.1.2 3
!
ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::5
ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::4 2
ipv6 route 2001:DB8:1001::2/128 Ethernet0/3 FE80::14 3
ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::5 2
ipv6 route 2001:DB8:1001::3/128 Ethernet0/3 FE80::14 3
ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::5 2
ipv6 route 2001:DB8:1001::4/128 Ethernet0/3 FE80::14 3
ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::5
ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::4 2
ipv6 route 2001:DB8:1001::5/128 Ethernet0/3 FE80::14 3
ipv6 route 2001:DB8:1001::12/128 Ethernet0/3 FE80::14
ipv6 route 2001:DB8:1001::12/128 Ethernet0/2 FE80::15 2
ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::5 3
ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::4 4
ipv6 route 2001:DB8:1001::14/128 Ethernet0/3 FE80::14
ipv6 route 2001:DB8:1001::14/128 Ethernet0/2 FE80::15 2
ipv6 route 2001:DB8:1001::14/128 Ethernet0/0 FE80::5 3
ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::4 4
ipv6 route 2001:DB8:1001::19/128 Ethernet0/3 FE80::14
ipv6 route 2001:DB8:1001::19/128 Ethernet0/2 FE80::15 2
ipv6 route 2001:DB8:1001::19/128 Ethernet0/0 FE80::5 3
ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::4 4
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::4
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::5 2
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/3 FE80::14 3
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::5
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::4 2
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/3 FE80::14 3
ipv6 route ::/0 Ethernet0/2 FE80::15
ipv6 route ::/0 Ethernet0/3 FE80::14 2
ipv6 route ::/0 Ethernet0/1 FE80::4 3
ipv6 route ::/0 Ethernet0/0 FE80::5 4
!
</pre>
</details>
<details>
  <summary>R14</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.0.14 255.255.255.255
 ipv6 address 2001:DB8:1001::14/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.0 255.255.255.254
 ipv6 address FE80::14 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.2 255.255.255.254
 ipv6 address FE80::14 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 101.0.100.1 255.255.255.254
 ipv6 address FE80::14 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 172.16.1.4 255.255.255.254
 ipv6 address FE80::14 link-local
!
ip route 0.0.0.0 0.0.0.0 101.0.100.0
ip route 0.0.0.0 0.0.0.0 172.16.1.3 2
ip route 0.0.0.0 0.0.0.0 172.16.1.1 3
ip route 100.1.0.2 255.255.255.255 172.16.1.3
ip route 100.1.0.2 255.255.255.255 172.16.1.1 2
ip route 100.1.0.3 255.255.255.255 172.16.1.1
ip route 100.1.0.3 255.255.255.255 172.16.1.3 2
ip route 100.1.0.4 255.255.255.255 172.16.1.1
ip route 100.1.0.4 255.255.255.255 172.16.1.3 2
ip route 100.1.0.5 255.255.255.255 172.16.1.3
ip route 100.1.0.5 255.255.255.255 172.16.1.1 2
ip route 100.1.0.12 255.255.255.255 172.16.1.1
ip route 100.1.0.12 255.255.255.255 172.16.1.3 2
ip route 100.1.0.13 255.255.255.255 172.16.1.3
ip route 100.1.0.13 255.255.255.255 172.16.1.1 2
ip route 100.1.0.15 255.255.255.255 172.16.1.3
ip route 100.1.0.15 255.255.255.255 172.16.1.1 2
ip route 100.1.0.19 255.255.255.255 172.16.1.5
ip route 100.1.0.20 255.255.255.255 172.16.1.3
ip route 100.1.0.20 255.255.255.255 172.16.1.1 2
ip route 100.1.10.0 255.255.255.240 172.16.1.1
ip route 100.1.10.0 255.255.255.240 172.16.1.3 2
ip route 100.1.20.0 255.255.255.240 172.16.1.3
ip route 100.1.20.0 255.255.255.240 172.16.1.1 2
!
ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::13
ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::12 2
ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::12
ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::13 2
ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::12
ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::13 2
ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::13
ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::12 2
ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::12
ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::13 2
ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::13
ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::12 2
ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::13
ipv6 route 2001:DB8:1001::15/128 Ethernet0/0 FE80::12 2
ipv6 route 2001:DB8:1001::19/128 Ethernet0/3 FE80::19
ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::13
ipv6 route 2001:DB8:1001::20/128 Ethernet0/0 FE80::12 2
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::12
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::13 2
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::13
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::12 2
ipv6 route ::/0 Ethernet0/2 FE80::22
ipv6 route ::/0 Ethernet0/1 FE80::13 2
ipv6 route ::/0 Ethernet0/0 FE80::12 3
!
</pre>
</details>
<details>
  <summary>R15</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.0.15 255.255.255.255
 ipv6 address 2001:DB8:1001::15/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.6 255.255.255.254
 ipv6 address FE80::15 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.8 255.255.255.254
 ipv6 address FE80::15 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 30.1.100.1 255.255.255.254
 ipv6 address FE80::15 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 172.16.1.10 255.255.255.254
 ipv6 address FE80::15 link-local
!
ip route 0.0.0.0 0.0.0.0 30.1.100.0
ip route 0.0.0.0 0.0.0.0 172.16.1.9 2
ip route 0.0.0.0 0.0.0.0 172.16.1.7 3
ip route 100.1.0.2 255.255.255.255 172.16.1.7
ip route 100.1.0.2 255.255.255.255 172.16.1.9 2
ip route 100.1.0.3 255.255.255.255 172.16.1.9
ip route 100.1.0.3 255.255.255.255 172.16.1.7 2
ip route 100.1.0.4 255.255.255.255 172.16.1.9
ip route 100.1.0.4 255.255.255.255 172.16.1.7 2
ip route 100.1.0.5 255.255.255.255 172.16.1.7
ip route 100.1.0.5 255.255.255.255 172.16.1.9 2
ip route 100.1.0.12 255.255.255.255 172.16.1.9
ip route 100.1.0.12 255.255.255.255 172.16.1.7 2
ip route 100.1.0.13 255.255.255.255 172.16.1.7
ip route 100.1.0.13 255.255.255.255 172.16.1.9 2
ip route 100.1.0.14 255.255.255.255 172.16.1.9
ip route 100.1.0.14 255.255.255.255 172.16.1.7 2
ip route 100.1.0.19 255.255.255.255 172.16.1.9
ip route 100.1.0.19 255.255.255.255 172.16.1.7 2
ip route 100.1.0.20 255.255.255.255 172.16.1.11
ip route 100.1.10.0 255.255.255.240 172.16.1.9
ip route 100.1.10.0 255.255.255.240 172.16.1.7 2
ip route 100.1.20.0 255.255.255.240 172.16.1.7
ip route 100.1.20.0 255.255.255.240 172.16.1.9 2
!
ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::13
ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::12 2
ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::12
ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::13 2
ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::12
ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::13 2
ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::13
ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::12 2
ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::12
ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::13 2
ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::13
ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::12 2
ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::12
ipv6 route 2001:DB8:1001::14/128 Ethernet0/0 FE80::13 2
ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::12
ipv6 route 2001:DB8:1001::19/128 Ethernet0/0 FE80::13 2
ipv6 route 2001:DB8:1001::20/128 Ethernet0/3 FE80::20
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::12
ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::13 2
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::13
ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::12 2
ipv6 route ::/0 Ethernet0/2 FE80::21
ipv6 route ::/0 Ethernet0/1 FE80::12 2
ipv6 route ::/0 Ethernet0/0 FE80::13 3
!
</pre>
</details>
<details>
  <summary>R19</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.0.19 255.255.255.255
 ipv6 address 2001:DB8:1001::19/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.5 255.255.255.254
 ipv6 address FE80::19 link-local
!
ip route 0.0.0.0 0.0.0.0 172.16.1.4
!
ipv6 route ::/0 Ethernet0/0 FE80::14
!
</pre>
</details>
<details>
  <summary>R20</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.0.20 255.255.255.255
 ipv6 address 2001:DB8:1001::20/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.11 255.255.255.254
 ipv6 address FE80::20 link-local
!
ip route 0.0.0.0 0.0.0.0 172.16.1.10
!
ipv6 route ::/0 Ethernet0/0 FE80::15
!
</pre>
</details>
<hr>

AS1001 Чокурдах

| Device  | Interface                       | IPv4 Address                                                    | IPv6 Address                                                                                       |
|---------|---------------------------------|-----------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
|**VPC30**| eth0                            | 100.1.10.18/28 gw 100.1.10.17                                   | 2001:DB8:1001:A10::/64 (SLAAC)                                                                     |
|**VPC31**| eth0                            | 100.1.20.18/28 gw 100.1.20.17                                   | 2001:DB8:1001:A20::/64 (SLAAC)                                                                     |
| **R28** | Lo1<br>e0/0<br>e0/1<br>e0/2     | 100.1.1.28<br>5.20.26.1/31<br>5.20.25.3/31<br>172.16.1.0/31     | 2001:DB8:1001:AA1::28<br>FE80::28 link-local<br>FE80::28 link-local<br>FE80::28 link-local         |
| **SW29**| Lo1<br>e0/2<br>vlan10<br>vlan20 | 100.1.1.29<br>172.16.1.1/31<br>100.1.10.17/28<br>100.1.20.17/28 | 2001:DB8:1001:AA1::29<br>FE80::29 link-local<br>2001:DB8:1001:A10::1/64<br>2001:DB8:1001:A20::1/64 |
<details>
  <summary>R28</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.1.28 255.255.255.255
 ipv6 address 2001:DB8:1001:AA1::28/128
!
interface Ethernet0/0
 no shutdown
 ip address 5.20.26.1 255.255.255.254
 ipv6 address FE80::28 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 5.20.25.3 255.255.255.254
 ipv6 address FE80::28 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.0 255.255.255.254
 ipv6 address FE80::28 link-local
!
ip route 0.0.0.0 0.0.0.0 5.20.26.0
ip route 0.0.0.0 0.0.0.0 5.20.25.2 2
ip route 100.1.1.29 255.255.255.255 172.16.1.1
ip route 100.1.10.16 255.255.255.240 172.16.1.1
ip route 100.1.20.16 255.255.255.240 172.16.1.1
!
ipv6 route 2001:DB8:1001:A10::/64 Ethernet0/2 FE80::29
ipv6 route 2001:DB8:1001:A20::/64 Ethernet0/2 FE80::29
ipv6 route 2001:DB8:1001:AA1::29/128 Ethernet0/2 FE80::29
ipv6 route ::/0 Ethernet0/0 FE80::26
ipv6 route ::/0 Ethernet0/1 FE80::25 2
!
</pre>
</details>
<details>
  <summary>SW29</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
vlan10
!
vlan20
!
interface Loopback1
 no shutdown
 ip address 100.1.1.29 255.255.255.255
 ipv6 address 2001:DB8:1001:AA1::29/128
!
interface Ethernet0/0
 no shutdown
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/1
 no shutdown
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/2
 no shutdown
 no switchport
 ip address 172.16.1.1 255.255.255.254
 duplex auto
 ipv6 address FE80::29 link-local
!
interface Vlan10
 no shutdown
 ip address 100.1.10.17 255.255.255.240
 ipv6 address 2001:DB8:1001:A10::1/64
 arp timeout 30
!
interface Vlan20
 no shutdown
 ip address 100.1.20.17 255.255.255.240
 ipv6 address 2001:DB8:1001:A20::1/64
 ipv6 nd cache expire 30
 arp timeout 30
!
ip route 0.0.0.0 0.0.0.0 172.16.1.0
!
ipv6 route ::/0 Ethernet0/2 FE80::28
!
</pre>
</details>
<hr>

AS1001 Лабытнанги

| Device  | Interface   | IPv4 Address                  | IPv6 Address                                     |
|---------|-------------|-------------------------------|--------------------------------------------------|
| **R27** | Lo1<br>e0/0 | 100.1.2.27/32<br>5.20.25.1/31 | 2001:DB8:1001:BB2::27/128<br>FE80::27 link-local |
<details>
  <summary>R27</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 100.1.2.27 255.255.255.255
 ipv6 address 2001:DB8:1001:BB2::27/128
!
interface Ethernet0/0
 no shutdown
 ip address 5.20.25.1 255.255.255.254
 ipv6 address FE80::27 link-local
!
ip route 0.0.0.0 0.0.0.0 5.20.25.0
!
ipv6 route ::/0 Ethernet0/0 FE80::25
!
</pre>
</details>
<hr>

AS2042 С.-Петербург

| Device | Interface                                | IPv4 Address                                                                        | IPv6 Address                                                                                                         |
|--------|------------------------------------------|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **VPC**| eth0                                     | 20.42.10.2/28 gw 20.42.10.1                                                         | 2001:DB8:2042:10::/64 (SLAAC)                                                                                        |
|**VPC8**| eth0                                     | 20.42.20.2/28 gw 20.42.20.1                                                         | 2001:DB8:2042:20::/64 (SLAAC)                                                                                        |
| **SW9**| Lo1<br>e0/3<br>e1/0<br>vlan10<br>vlan251 | 20.42.0.9/32<br>172.16.1.11/31<br>172.16.1.7/31<br>20.42.10.1/28<br>172.16.1.14/31  | 2001:DB8:2042::9/128<br>FE80::9 link-local<br>FE80::9 link-local<br>2001:DB8:2042:10::1/64<br>FE80::9 link-local     |
|**SW10**| Lo1<br>e0/3<br>e1/0<br>vlan20<br>vlan251 | 100.1.0.10/32<br>172.16.1.5/31<br>172.16.1.13/31<br>100.1.20.1/28<br>172.16.1.15/31 | 2001:DB8:2042::10/128<br>FE80::10 link-local<br>FE80::10 link-local<br>2001:DB8:2042:20::1/64<br>FE80::10 link-local |
| **R16**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3      | 20.42.0.16/32<br>172.16.1.4/31<br>172.16.1.1/31<br>172.16.1.6/31<br>172.16.1.8/31   | 2001:DB8:2042::16/128<br>FE80::16 link-local<br>FE80::16 link-local<br>FE80::16 link-local<br>FE80::16 link-local    |
| **R17**| Lo1<br>e0/0<br>e0/1<br>e0/2              | 20.42.0.17/32<br>172.16.1.10/31<br>172.16.1.3/31<br>172.16.1.12/31                  | 2001:DB8:2042::17/128<br>FE80::17 link-local<br>FE80::17 link-local<br>FE80::17 link-local                           |
| **R18**| Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3      | 20.42.0.18/32<br>172.16.1.0/31<br>172.16.1.2/31<br>5.20.24.3/31<br>5.20.26.3/31     | 2001:DB8:2042::18/128<br>FE80::18 link-local<br>FE80::18 link-local<br>FE80::18 link-local<br>FE80::18 link-local    |
| **R32**| Lo1<br>e0/0                              | 20.42.0.32/32<br>172.16.1.9/31                                                      | 2001:DB8:2042::32/128<br>FE80::32 link-local                                                                         |
<details>
  <summary>SW9</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
vlan10
!
vlan251
 name po1-vlan
!
vlan 1000
!
interface Loopback1
 no shutdown
 ip address 20.42.0.9 255.255.255.255
 ipv6 address 2001:DB8:2042::9/128
!
interface Port-channel1
 no shutdown
 switchport trunk allowed vlan 251,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 251,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 251,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 10
 switchport mode access
!
interface Ethernet0/3
 no shutdown
 no switchport
 ip address 172.16.1.11 255.255.255.254
 duplex auto
 ipv6 address FE80::9 link-local
!
interface Ethernet1/0
 no shutdown
 no switchport
 ip address 172.16.1.7 255.255.255.254
 duplex auto
 ipv6 address FE80::9 link-local
!
interface Vlan10
 no shutdown
 ip address 20.42.10.1 255.255.255.240
 ipv6 address 2001:DB8:2042:10::1/64
 ipv6 nd cache expire 30
 arp timeout 30
!
interface Vlan251
 no shutdown
 ip address 172.16.1.14 255.255.255.254
 ipv6 address FE80::9 link-local
 ipv6 nd cache expire 30
 arp timeout 30
!
ip route 0.0.0.0 0.0.0.0 172.16.1.10
ip route 0.0.0.0 0.0.0.0 172.16.1.6 2
ip route 0.0.0.0 0.0.0.0 172.16.1.15 3
ip route 20.42.0.10 255.255.255.255 172.16.1.15
ip route 20.42.0.10 255.255.255.255 172.16.1.6 2
ip route 20.42.0.16 255.255.255.255 172.16.1.6
ip route 20.42.0.16 255.255.255.255 172.16.1.15 2
ip route 20.42.0.16 255.255.255.255 172.16.1.10 3
ip route 20.42.0.17 255.255.255.255 172.16.1.10
ip route 20.42.0.17 255.255.255.255 172.16.1.15 2
ip route 20.42.0.17 255.255.255.255 172.16.1.6 3
ip route 20.42.0.32 255.255.255.255 172.16.1.6
ip route 20.42.0.32 255.255.255.255 172.16.1.15 2
ip route 20.42.0.32 255.255.255.255 172.16.1.10 3
ip route 20.42.20.0 255.255.255.240 172.16.1.15
ip route 20.42.20.0 255.255.255.240 172.16.1.6 2
!
ipv6 route 2001:DB8:2042::10/128 Ethernet1/0 FE80::16 2
ipv6 route 2001:DB8:2042::10/128 Vlan251 FE80::10
ipv6 route 2001:DB8:2042::16/128 Ethernet0/3 FE80::17 3
ipv6 route 2001:DB8:2042::16/128 Vlan251 FE80::10 2
ipv6 route 2001:DB8:2042::16/128 Ethernet1/0 FE80::16
ipv6 route 2001:DB8:2042::17/128 Ethernet1/0 FE80::16 3
ipv6 route 2001:DB8:2042::17/128 Vlan251 FE80::10 2
ipv6 route 2001:DB8:2042::17/128 Ethernet0/3 FE80::17
ipv6 route 2001:DB8:2042::32/128 Ethernet0/3 FE80::17 3
ipv6 route 2001:DB8:2042::32/128 Vlan251 FE80::10 2
ipv6 route 2001:DB8:2042::32/128 Ethernet1/0 FE80::16
ipv6 route 2001:DB8:2042:20::/64 Vlan251 FE80::10
ipv6 route 2001:DB8:2042:20::/64 Ethernet1/0 FE80::16 2
ipv6 route ::/0 Vlan251 FE80::10 3
ipv6 route ::/0 Ethernet1/0 FE80::16 2
ipv6 route ::/0 Ethernet0/3 FE80::17
!
</pre>
</details>
<details>
  <summary>SW10</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
vlan20
!
vlan251
 name po1-vlan
!
vlan 1000
!
interface Loopback1
 no shutdown
 ip address 20.42.0.10 255.255.255.255
 ipv6 address 2001:DB8:2042::10/128
!
interface Port-channel1
 no shutdown
 switchport trunk allowed vlan 251,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
!
interface Ethernet0/0
 no shutdown
 switchport trunk allowed vlan 251,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 251,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 20
 switchport mode access
!
interface Ethernet0/3
 no shutdown
 no switchport
 ip address 172.16.1.5 255.255.255.254
 duplex auto
 ipv6 address FE80::10 link-local
!
interface Ethernet1/0
 no shutdown
 no switchport
 ip address 172.16.1.13 255.255.255.254
 duplex auto
 ipv6 address FE80::10 link-local
!
interface Vlan20
 no shutdown
 ip address 20.42.20.1 255.255.255.240
 ipv6 address 2001:DB8:2042:20::1/64
 ipv6 nd cache expire 30
 arp timeout 30
!
interface Vlan251
 no shutdown
 ip address 172.16.1.15 255.255.255.254
 ipv6 address FE80::10 link-local
 ipv6 nd cache expire 30
 arp timeout 30
!
ip route 0.0.0.0 0.0.0.0 172.16.1.4
ip route 0.0.0.0 0.0.0.0 172.16.1.12 2
ip route 0.0.0.0 0.0.0.0 172.16.1.14 3
ip route 20.42.0.9 255.255.255.255 172.16.1.14
ip route 20.42.0.9 255.255.255.255 172.16.1.12 2
ip route 20.42.0.16 255.255.255.255 172.16.1.4
ip route 20.42.0.16 255.255.255.255 172.16.1.14 2
ip route 20.42.0.16 255.255.255.255 172.16.1.12 3
ip route 20.42.0.17 255.255.255.255 172.16.1.12
ip route 20.42.0.17 255.255.255.255 172.16.1.14 2
ip route 20.42.0.17 255.255.255.255 172.16.1.4 3
ip route 20.42.0.32 255.255.255.255 172.16.1.4
ip route 20.42.0.32 255.255.255.255 172.16.1.14 2
ip route 20.42.0.32 255.255.255.255 172.16.1.12 2
ip route 20.42.10.0 255.255.255.240 172.16.1.14
ip route 20.42.10.0 255.255.255.240 172.16.1.12 2
!
ipv6 route 2001:DB8:2042::9/128 Ethernet1/0 FE80::17 2
ipv6 route 2001:DB8:2042::9/128 Vlan251 FE80::9
ipv6 route 2001:DB8:2042::16/128 Ethernet1/0 FE80::17 3
ipv6 route 2001:DB8:2042::16/128 Vlan251 FE80::9 2
ipv6 route 2001:DB8:2042::16/128 Ethernet0/3 FE80::16
ipv6 route 2001:DB8:2042::17/128 Ethernet0/3 FE80::16 3
ipv6 route 2001:DB8:2042::17/128 Vlan251 FE80::9 2
ipv6 route 2001:DB8:2042::17/128 Ethernet1/0 FE80::17
ipv6 route 2001:DB8:2042::32/128 Ethernet1/0 FE80::17 2
ipv6 route 2001:DB8:2042::32/128 Vlan251 FE80::9 2
ipv6 route 2001:DB8:2042::32/128 Ethernet0/3 FE80::16
ipv6 route 2001:DB8:2042:10::/64 Ethernet1/0 FE80::17 2
ipv6 route 2001:DB8:2042:10::/64 Vlan251 FE80::9
ipv6 route ::/0 Vlan251 FE80::9 3
ipv6 route ::/0 Ethernet1/0 FE80::17 2
ipv6 route ::/0 Ethernet0/3 FE80::16
!
</pre>
</details>
<details>
  <summary>R16</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 20.42.0.16 255.255.255.255
 ipv6 address 2001:DB8:2042::16/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.4 255.255.255.254
 ipv6 address FE80::16 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.1 255.255.255.254
 ipv6 address FE80::16 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.6 255.255.255.254
 ipv6 address FE80::16 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 172.16.1.8 255.255.255.254
 ipv6 address FE80::16 link-local
!
ip route 0.0.0.0 0.0.0.0 172.16.1.0
ip route 0.0.0.0 0.0.0.0 172.16.1.7 2
ip route 0.0.0.0 0.0.0.0 172.16.1.5 3
ip route 20.42.0.9 255.255.255.255 172.16.1.7
ip route 20.42.0.9 255.255.255.255 172.16.1.5 2
ip route 20.42.0.10 255.255.255.255 172.16.1.5
ip route 20.42.0.10 255.255.255.255 172.16.1.7 2
ip route 20.42.0.32 255.255.255.255 172.16.1.9
ip route 20.42.10.0 255.255.255.240 172.16.1.7
ip route 20.42.10.0 255.255.255.240 172.16.1.5 2
ip route 20.42.20.0 255.255.255.240 172.16.1.5
ip route 20.42.20.0 255.255.255.240 172.16.1.7 2
!
ipv6 route 2001:DB8:2042::9/128 Ethernet0/2 FE80::9
ipv6 route 2001:DB8:2042::9/128 Ethernet0/0 FE80::10 2
ipv6 route 2001:DB8:2042::10/128 Ethernet0/0 FE80::10
ipv6 route 2001:DB8:2042::10/128 Ethernet0/2 FE80::9 2
ipv6 route 2001:DB8:2042::32/128 Ethernet0/3 FE80::32
ipv6 route 2001:DB8:2042:10::/64 Ethernet0/2 FE80::9
ipv6 route 2001:DB8:2042:10::/64 Ethernet0/0 FE80::10 2
ipv6 route 2001:DB8:2042:20::/64 Ethernet0/0 FE80::10
ipv6 route 2001:DB8:2042:20::/64 Ethernet0/2 FE80::9 2
ipv6 route ::/0 Ethernet0/1 FE80::18
ipv6 route ::/0 Ethernet0/2 FE80::9 2
ipv6 route ::/0 Ethernet0/0 FE80::10 3
!
</pre>
</details>
<details>
  <summary>R17</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 20.42.0.17 255.255.255.255
 ipv6 address 2001:DB8:2042::17/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.10 255.255.255.254
 ipv6 address FE80::17 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.3 255.255.255.254
 ipv6 address FE80::17 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 172.16.1.12 255.255.255.254
 ipv6 address FE80::17 link-local
!
ip route 0.0.0.0 0.0.0.0 172.16.1.2
ip route 0.0.0.0 0.0.0.0 172.16.1.13 2
ip route 0.0.0.0 0.0.0.0 172.16.1.11 3
ip route 20.42.0.9 255.255.255.255 172.16.1.11
ip route 20.42.0.9 255.255.255.255 172.16.1.13 2
ip route 20.42.0.10 255.255.255.255 172.16.1.13
ip route 20.42.0.10 255.255.255.255 172.16.1.11 2
ip route 20.42.10.0 255.255.255.240 172.16.1.11
ip route 20.42.10.0 255.255.255.240 172.16.1.13 2
ip route 20.42.20.0 255.255.255.240 172.16.1.13
ip route 20.42.20.0 255.255.255.240 172.16.1.11 2
!
ipv6 route 2001:DB8:2042::9/128 Ethernet0/0 FE80::9
ipv6 route 2001:DB8:2042::9/128 Ethernet0/2 FE80::10 2
ipv6 route 2001:DB8:2042::10/128 Ethernet0/2 FE80::10
ipv6 route 2001:DB8:2042::10/128 Ethernet0/0 FE80::9 2
ipv6 route 2001:DB8:2042:10::/64 Ethernet0/0 FE80::9
ipv6 route 2001:DB8:2042:10::/64 Ethernet0/2 FE80::10 2
ipv6 route 2001:DB8:2042:20::/64 Ethernet0/2 FE80::10
ipv6 route 2001:DB8:2042:20::/64 Ethernet0/0 FE80::9 2
ipv6 route ::/0 Ethernet0/1 FE80::18
ipv6 route ::/0 Ethernet0/2 FE80::10 2
ipv6 route ::/0 Ethernet0/0 FE80::9 3
!
</pre>
</details>
<details>
  <summary>R18</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 20.42.0.18 255.255.255.255
 ipv6 address 2001:DB8:2042::18/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.0 255.255.255.254
 ipv6 address FE80::18 link-local
!
interface Ethernet0/1
 no shutdown
 ip address 172.16.1.2 255.255.255.254
 ipv6 address FE80::18 link-local
!
interface Ethernet0/2
 no shutdown
 ip address 5.20.24.3 255.255.255.254
 ipv6 address FE80::18 link-local
!
interface Ethernet0/3
 no shutdown
 ip address 5.20.26.3 255.255.255.254
 ipv6 address FE80::18 link-local
!
ip route 0.0.0.0 0.0.0.0 5.20.24.2
ip route 0.0.0.0 0.0.0.0 5.20.26.2 2
ip route 20.42.0.9 255.255.255.255 172.16.1.3
ip route 20.42.0.9 255.255.255.255 172.16.1.1 2
ip route 20.42.0.10 255.255.255.255 172.16.1.1
ip route 20.42.0.10 255.255.255.255 172.16.1.3 2
ip route 20.42.0.16 255.255.255.255 172.16.1.1
ip route 20.42.0.16 255.255.255.255 172.16.1.3 2
ip route 20.42.0.17 255.255.255.255 172.16.1.3
ip route 20.42.0.17 255.255.255.255 172.16.1.1 2
ip route 20.42.0.32 255.255.255.255 172.16.1.1
ip route 20.42.0.32 255.255.255.255 172.16.1.3 2
ip route 20.42.10.0 255.255.255.240 172.16.1.3
ip route 20.42.10.0 255.255.255.240 172.16.1.1 2
ip route 20.42.20.0 255.255.255.240 172.16.1.1
ip route 20.42.20.0 255.255.255.240 172.16.1.3 2
!
ipv6 route 2001:DB8:2042::9/128 Ethernet0/1 FE80::17
ipv6 route 2001:DB8:2042::9/128 Ethernet0/0 FE80::16 2
ipv6 route 2001:DB8:2042::10/128 Ethernet0/0 FE80::16
ipv6 route 2001:DB8:2042::10/128 Ethernet0/1 FE80::17 2
ipv6 route 2001:DB8:2042::16/128 Ethernet0/0 FE80::16
ipv6 route 2001:DB8:2042::16/128 Ethernet0/1 FE80::17 2
ipv6 route 2001:DB8:2042::17/128 Ethernet0/1 FE80::17
ipv6 route 2001:DB8:2042::17/128 Ethernet0/0 FE80::16 2
ipv6 route 2001:DB8:2042::32/128 Ethernet0/0 FE80::16
ipv6 route 2001:DB8:2042::32/128 Ethernet0/1 FE80::17 2
ipv6 route 2001:DB8:2042:10::/64 Ethernet0/1 FE80::17
ipv6 route 2001:DB8:2042:10::/64 Ethernet0/0 FE80::16 2
ipv6 route 2001:DB8:2042:20::/64 Ethernet0/0 FE80::16
ipv6 route 2001:DB8:2042:20::/64 Ethernet0/1 FE80::17 2
ipv6 route ::/0 Ethernet0/2 FE80::24
ipv6 route ::/0 Ethernet0/3 FE80::26 2
!
</pre>
</details>
<details>
  <summary>R32</summary>
<pre>
!
ip cef
ipv6 unicast-routing
ipv6 cef
!
interface Loopback1
 no shutdown
 ip address 20.42.0.32 255.255.255.255
 ipv6 address 2001:DB8:2042::32/128
!
interface Ethernet0/0
 no shutdown
 ip address 172.16.1.9 255.255.255.254
 ipv6 address FE80::32 link-local
!
ip route 0.0.0.0 0.0.0.0 172.16.1.8
!
ipv6 route ::/0 Ethernet0/0 FE80::16
!
</pre>
</details>