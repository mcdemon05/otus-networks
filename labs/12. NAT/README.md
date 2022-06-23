## Задание:

Настроить DHCP в офисе Москва
<br>
Настроить синхронизацию времени в офисе Москва
<br>
Настроить NAT в офисе Москва, C.-Перетбруг и Чокурдах

##  Решение:

- [Конфигурационные файлы;](configs/)
- [Сохраненная топология из EVE-NG;](eve-ng_lab_NAT.zip)

### Графическая схема

![](Topology.PNG)

### Адресное пространство:

| **Автономка**       | **IPv4 подсети**                                 | **IPv6 подсети**       |
|---------------------|--------------------------------------------------|------------------------|
| AS520 (Триада)      | 5.20.0.0/16                                      | 2001:DB8:520::/48      |
| AS101 (Киторн)      | 101.0.0.0/16                                     | 2001:DB8:101::/48      |
| AS301 (Ламас)       | 30.1.0.0/16                                      | 2001:DB8:301::/48      |
| AS1001 Москва       | 100.1.0.0/16                                     | 2001:DB8:1001::/48     |
| AS1001 Чокурдах     | 100.1.1.0/24<br>100.1.10.16/28<br>100.1.20.16/28 | 2001:DB8:1001:A00::/56 |
| AS1001 Лабытнанги   | 100.1.2.0/24                                     | 2001:DB8:1001:B00::/56 |
| AS2042 С.-Петербург | 20.42.0.0/16                                     | 2001:DB8:2042::/48     |

### IP интерфейсы:

<details>
  <summary>AS520 (Триада)</summary>

| **Device** |            **Interface**            |                                **IPv4 Address**                                |                                                                                **IPv6 Address**                                                                        |
|:----------:|:-----------------------------------:|:------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|   **R23**  |     Lo1<br>e0/0<br>e0/1<br>e0/2     |         5.20.0.23/32<br>5.20.23.0/31<br>172.16.1.0/31<br>172.16.1.2/31         |                                            2001:DB8:520::23/128<br>FE80::23 link-local<br>FE80::23 link-local<br>FE80::23 link-local                                   |
|   **R24**  | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 5.20.0.24/32<br>5.20.24.0/31<br>172.16.1.4/31<br>172.16.1.3/31<br>5.20.24.2/31 | 2001:DB8:520::24/128<br>FE80::24 link-local, 2001:DB8:520:24E0::24/112<br>FE80::24 link-local<br>FE80::24 link-local<br>FE80::24 link-local, 2001:DB8:520:24E3::24/112 |
|   **R25**  | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 5.20.0.25/32<br>172.16.1.1/31<br>5.20.25.0/31<br>172.16.1.6/31<br>5.20.25.2/31 |                                2001:DB8:520::25/128<br>FE80::25 link-local<br>FE80::25 link-local<br>FE80::25 link-local<br>FE80::25 link-local                        |
|   **R26**  | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 5.20.0.26/32<br>172.16.1.5/31<br>5.20.26.0/31<br>172.16.1.7/31<br>5.20.26.2/31 |                 2001:DB8:520::26/128<br>FE80::26 link-local<br>FE80::26 link-local<br>FE80::26 link-local<br>FE80::26 link-local, 2001:DB8:520:26E3::26/112            |
</details>

<details>
  <summary>AS301 (Ламас)</summary>

| **Device** | **Interface**               | **IPv4 Address**                                               | **IPv6 Address**                                                                                                                                                           |
|------------|-----------------------------|----------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **R21**    | Lo1<br>e0/0<br>e0/1<br>e0/2 | 30.1.0.21/32<br>30.1.100.0/31<br>172.16.1.0/31<br>5.20.24.1/31 | 2001:DB8:301::21/128<br>FE80::21 link-local, 2001:DB8:301:21E0::21/112<br>FE80::21 link-local, 2001:DB8:301:21E1::21/112<br>FE80::21 link-local, 2001:DB8:520:24E0::21/112 |
</details>

<details>
  <summary>AS101 (Киторн)</summary>

| **Device** | **Interface**               | **IPv4 Address**                                                 | **IPv6 Address**                                                                                                                                |
|------------|-----------------------------|------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| **R22**    | Lo1<br>e0/0<br>e0/1<br>e0/2 | 101.0.0.22/32<br>101.0.100.0/31<br>172.16.1.1/31<br>5.20.23.1/31 | 2001:DB8:101::22/128<br>FE80::22 link-local, 2001:DB8:101:22E0::22/112<br>FE80::22 link-local, 2001:DB8:301:21E1::22/112<br>FE80::22 link-local |
</details>

<details>
  <summary>AS1001 Москва</summary>

| **Device** | **Interface**                                  | **IPv4 Address**                                                                                       | **IPv6 Address**                                                                                                                             |
|------------|------------------------------------------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **VPC1**   | eth0                                           | 192.168.10.0/28 gw 192.168.10.1 (DHCP)                                                                 | 2001:DB8:1001:10::/64 (SLAAC)                                                                                                                |
| **VPC7**   | eth0                                           | 192.168.20.0/28 gw 192.168.20.1 (DHCP)                                                                 | 2001:DB8:1001:20::/64 (SLAAC)                                                                                                                |
| **SW2**    | Lo1<br>e0/0<br>e0/1<br>vlan20                  | 100.1.0.2/32<br>172.16.1.27/31<br>172.16.1.23/31<br>192.168.20.1/28                                    | 2001:DB8:1001::2/128<br>FE80::2 link-local<br>FE80::2 link-local<br>2001:DB8:1001:20::1/64                                                   |
| **SW3**    | Lo1<br>e0/0<br>e0/1<br>vlan10                  | 100.1.0.3/32<br>172.16.1.21/31<br>172.16.1.29/31<br>192.168.10.1/28                                    | 2001:DB8:1001::3/128<br>FE80::3 link-local<br>FE80::3 link-local<br>2001:DB8:1001:10::1/64                                                   |
| **SW4**    | Lo1<br>e0/0<br>e0/1<br>e1/0<br>e1/1<br>vlan201 | 100.1.0.4/32<br>172.16.1.20/31<br>172.16.1.22/31<br>172.16.1.13/31<br>172.16.1.19/31<br>172.16.1.24/31 | 2001:DB8:1001::4/128<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local           |
| **SW5**    | Lo1<br>e0/0<br>e0/1<br>e1/0<br>e1/1<br>vlan201 | 100.1.0.5/32<br>172.16.1.26/31<br>172.16.1.28/31<br>172.16.1.17/31<br>172.16.1.15/31<br>172.16.1.25/31 | 2001:DB8:1001::5/128<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local           |
| **R12**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.12/32<br>172.16.1.12/31<br>172.16.1.14/31<br>172.16.1.1/31<br>172.16.1.9/31                    | 2001:DB8:1001::12/128<br>FE80::12 link-local<br>FE80::12 link-local<br>FE80::12 link-local<br>FE80::12 link-local                            |
| **R13**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.13/32<br>172.16.1.16/31<br>172.16.1.18/31<br>172.16.1.7/31<br>172.16.1.3/31                    | 2001:DB8:1001::13/128<br>FE80::13 link-local<br>FE80::13 link-local<br>FE80::13 link-local<br>FE80::13 link-local                            |
| **R14**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.14/32<br>172.16.1.0/31<br>172.16.1.2/31<br>101.0.100.1/31<br>172.16.1.4/31                     | 2001:DB8:1001::14/128<br>FE80::14 link-local<br>FE80::14 link-local<br>FE80::14 link-local, 2001:DB8:101:22E0::14/112<br>FE80::14 link-local |
| **R15**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3            | 100.1.0.15/32<br>172.16.1.6/31<br>172.16.1.8/31<br>30.1.100.1/31<br>172.16.1.10/31                     | 2001:DB8:1001::15/128<br>FE80::15 link-local<br>FE80::15 link-local<br>FE80::15 link-local, 2001:DB8:301:21E0::15/112<br>FE80::15 link-local |
| **R19**    | Lo1<br>e0/0                                    | 100.1.0.19/32<br>172.16.1.5/31                                                                         | 2001:DB8:1001::19/128<br>FE80::19 link-local                                                                                                 |
| **R20**    | Lo1<br>e0/0                                    | 100.1.0.20/32<br>172.16.1.11/31                                                                        | 2001:DB8:1001::20/128<br>FE80::20 link-local                                                                                                 |
</details>

<details>
  <summary>AS1001 Чокурдах</summary>

| **Device** | **Interface**                   | **IPv4 Address**                                                    | **IPv6 Address**                                                                                   |
|------------|---------------------------------|---------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **VPC30**  | eth0                            | 192.168.10.2/28 gw 192.168.10.1                                     | 2001:DB8:1001:A10::/64 (SLAAC)                                                                     |
| **VPC31**  | eth0                            | 192.168.20.2/28 gw 192.168.20.1                                     | 2001:DB8:1001:A20::/64 (SLAAC)                                                                     |
| **R28**    | Lo1<br>e0/0<br>e0/1<br>e0/2     | 100.1.1.28<br>5.20.26.1/31<br>5.20.25.3/31<br>172.16.1.0/31         | 2001:DB8:1001:AA1::28<br>FE80::28 link-local<br>FE80::28 link-local<br>FE80::28 link-local         |
| **SW29**   | Lo1<br>e0/2<br>vlan10<br>vlan20 | 100.1.1.29<br>172.16.1.1/31<br>192.168.10.17/28<br>192.168.20.17/28 | 2001:DB8:1001:AA1::29<br>FE80::29 link-local<br>2001:DB8:1001:A10::1/64<br>2001:DB8:1001:A20::1/64 |
</details>

<details>
  <summary>AS1001 Лабытнанги</summary>

| **Device** | **Interface** | **IPv4 Address**              | **IPv6 Address**                                 |
|------------|---------------|-------------------------------|--------------------------------------------------|
| **R27**    | Lo1<br>e0/0   | 100.1.2.27/32<br>5.20.25.1/31 | 2001:DB8:1001:BB2::27/128<br>FE80::27 link-local |
</details>

<details>
  <summary>AS2042 С.-Петербург</summary>

| **Device** | **Interface**                            | **IPv4 Address**                                                                      | **IPv6 Address**                                                                                                                                                        |
|------------|------------------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **VPC**    | eth0                                     | 192.168.10.2/28 gw 192.168.10.1                                                       | 2001:DB8:2042:10::/64 (SLAAC)                                                                                                                                           |
| **VPC8**   | eth0                                     | 192.168.20.2/28 gw 192.168.20.1                                                       | 2001:DB8:2042:20::/64 (SLAAC)                                                                                                                                           |
| **SW9**    | Lo1<br>e0/3<br>e1/0<br>vlan10<br>vlan251 | 20.42.0.9/32<br>172.16.1.11/31<br>172.16.1.7/31<br>192.168.10.1/28<br>172.16.1.14/31  | 2001:DB8:2042::9/128<br>FE80::9 link-local<br>FE80::9 link-local<br>2001:DB8:2042:10::1/64<br>FE80::9 link-local                                                        |
| **SW10**   | Lo1<br>e0/3<br>e1/0<br>vlan20<br>vlan251 | 100.1.0.10/32<br>172.16.1.5/31<br>172.16.1.13/31<br>192.168.20.1/28<br>172.16.1.15/31 | 2001:DB8:2042::10/128<br>FE80::10 link-local<br>FE80::10 link-local<br>2001:DB8:2042:20::1/64<br>FE80::10 link-local                                                    |
| **R16**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3      | 20.42.0.16/32<br>172.16.1.4/31<br>172.16.1.1/31<br>172.16.1.6/31<br>172.16.1.8/31     | 2001:DB8:2042::16/128<br>FE80::16 link-local<br>FE80::16 link-local<br>FE80::16 link-local<br>FE80::16 link-local                                                       |
| **R17**    | Lo1<br>e0/0<br>e0/1<br>e0/2              | 20.42.0.17/32<br>172.16.1.10/31<br>172.16.1.3/31<br>172.16.1.12/31                    | 2001:DB8:2042::17/128<br>FE80::17 link-local<br>FE80::17 link-local<br>FE80::17 link-local                                                                              |
| **R18**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3      | 20.42.0.18/32<br>172.16.1.0/31<br>172.16.1.2/31<br>5.20.24.3/31<br>5.20.26.3/31       | 2001:DB8:2042::18/128<br>FE80::18 link-local<br>FE80::18 link-local<br>FE80::18 link-local, 2001:DB8:520:24E3::18/112<br>FE80::18 link-local, 2001:DB8:520:26E3::18/112 |
| **R32**    | Lo1<br>e0/0                              | 20.42.0.32/32<br>172.16.1.9/31                                                        | 2001:DB8:2042::32/128<br>FE80::32 link-local                                                                                                                            |
</details>

<hr>

### Внесение изменений в конфигурацию:
<details>
  <summary>SW2</summary>
<pre>
!
interface Vlan20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 100.1.0.13
!
ntp server 100.1.0.12
ntp server 100.1.0.13 prefer
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>SW3</summary>
<pre>
!
interface Vlan10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 100.1.0.12
!
ntp server 100.1.0.12 prefer
ntp server 100.1.0.13
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>SW4</summary>
<pre>
!
ntp server 100.1.0.12 prefer
ntp server 100.1.0.13
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>SW5</summary>
<pre>
!
ntp server 100.1.0.12
ntp server 100.1.0.13 prefer
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>R12</summary>
<pre>
!
ip dhcp excluded-address 192.168.10.1
ip dhcp excluded-address 192.168.10.3 192.168.10.14
!
ip dhcp pool SW3-vlan10
 network 192.168.10.0 255.255.255.0
 domain-name otus.com
 default-router 192.168.10.1 
 lease 0 0 30
!
ntp master 3
ntp update-calendar
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>R13</summary>
<pre>
!
ip dhcp excluded-address 192.168.20.1
ip dhcp excluded-address 192.168.20.3 192.168.20.14
!
ip dhcp pool SW2-vlan20
 network 192.168.20.0 255.255.255.0
 domain-name otus.com
 default-router 192.168.20.1 
 lease 0 0 30
!
ntp master 3
ntp update-calendar
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>R14</summary>
<pre>
!
interface Ethernet0/0
 ip nat inside
!
interface Ethernet0/1
 ip nat inside
!
interface Ethernet0/2
 ip nat outside
!
interface Ethernet0/3
 ip nat inside
!
ip nat pool NAT-POOL 100.1.200.1 100.1.200.1 prefix-length 30
ip nat inside source list NAT pool NAT-POOL overload
ip nat inside source static 192.168.0.19 100.1.200.19
ip nat inside source static 192.168.0.20 100.1.200.20
!
no ip route 100.1.10.16 255.255.255.240 101.0.100.0 201
no ip route 100.1.20.16 255.255.255.240 101.0.100.0 201
!
ip access-list standard NAT
 permit 192.168.10.0 0.0.0.255
 permit 192.168.20.0 0.0.0.255
!
ntp server 100.1.0.12 prefer
ntp server 100.1.0.13
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>R15</summary>
<pre>
!
interface Ethernet0/0
 ip nat inside
!
interface Ethernet0/1
 ip nat inside
!
interface Ethernet0/2
 ip nat outside
!
interface Ethernet0/3
 ip nat inside
!
ip nat pool NAT-POOL 100.1.200.1 100.1.200.1 prefix-length 30
ip nat inside source list NAT pool NAT-POOL overload
ip nat inside source static 192.168.0.19 100.1.200.19
ip nat inside source static 192.168.0.20 100.1.200.20
!
no ip route 100.1.10.16 255.255.255.240 30.1.100.0
no ip route 100.1.20.16 255.255.255.240 30.1.100.0
!
ip access-list standard NAT
 permit 192.168.10.0 0.0.0.255
 permit 192.168.20.0 0.0.0.255
!
ntp server 100.1.0.12
ntp server 100.1.0.13 prefer
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>R19</summary>
<pre>
!
interface Loopback2
 no shutdown
 ip address 192.168.0.19 255.255.255.255
 ip ospf 1 area 101
!
line vty 0 4
 transport input telnet
!
ntp server 100.1.0.12 prefer
ntp server 100.1.0.13
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>R20</summary>
<pre>
!
interface Loopback2
 no shutdown
 ip address 192.168.0.20 255.255.255.255
 ip ospf 1 area 102
!
ntp server 100.1.0.12
ntp server 100.1.0.13 prefer
!
clock timezone VLAD 10 0
!
</pre>
</details>

<details>
  <summary>SW9</summary>
<pre>
!
interface Vlan10
 ip address 192.168.10.1 255.255.255.0
!
</pre>
</details>

<details>
  <summary>SW10</summary>
<pre>
!
interface Vlan20
 ip address 192.168.20.1 255.255.255.0
!
</pre>
</details>

<details>
  <summary>R18</summary>
<pre>
!
interface Ethernet0/0
 ip nat inside
!
interface Ethernet0/1
 ip nat inside
!
interface Ethernet0/2
 ip nat outside
!
interface Ethernet0/3
 ip nat outside
!
ip nat pool NAT-POOL 20.42.200.1 20.42.200.6 prefix-length 29
ip nat inside source list NAT pool NAT-POOL overload
!
ip access-list standard NAT
 permit 192.168.10.0 0.0.0.255
 permit 192.168.20.0 0.0.0.255
!
</pre>
</details>

<details>
  <summary>R25</summary>
<pre>
!
no ip route 100.1.10.16 255.255.255.240 5.20.25.3
no ip route 100.1.20.16 255.255.255.240 5.20.25.3
!
</pre>
</details>

<details>
  <summary>R26</summary>
<pre>
!
no ip route 100.1.10.16 255.255.255.240 5.20.26.1
no ip route 100.1.20.16 255.255.255.240 5.20.26.1
!
</pre>
</details>

<details>
  <summary>R28</summary>
<pre>
!
interface Ethernet0/0
 ip nat outside
!
interface Ethernet0/1
 ip nat outside
!
interface Ethernet0/2
 ip nat inside
 no ip policy route-map office
!
ip nat inside source static 192.168.10.2 100.1.1.102
ip nat inside source static 192.168.20.2 100.1.1.202
!
ip route 192.168.0.0 255.255.0.0 172.16.1.1
no ip route 100.1.10.16 255.255.255.240 172.16.1.1
no ip route 100.1.20.16 255.255.255.240 172.16.1.1
!
no ip access-list standard VPC30
no ip access-list standard VPC31
!
no route-map office
!
</pre>
</details>

<details>
  <summary>SW29</summary>
<pre>
!
interface Vlan10
 ip address 192.168.10.1 255.255.255.0
!
interface Vlan20
 ip address 192.168.20.1 255.255.255.0
!
</pre>
</details>
