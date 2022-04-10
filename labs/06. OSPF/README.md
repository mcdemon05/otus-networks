## Задание:

Настроить OSPF офисе Москва
<br>
Разделить сеть на зоны
<br>
Настроить фильтрацию между зонами

##  Решение:

- [Конфигурационные файлы;](configs/)
- [Сохраненная топология из EVE-NG;](eve-ng_lab_PBR.zip)

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

<hr>

### Внесение изменений в конфигурацию:
<details>
  <summary>SW2</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Vlan20
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
router ospf 1
 passive-interface Vlan20
!
ipv6 router ospf 1
 passive-interface Vlan20
!
no ip route *
!
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::4
no ipv6 route ::/0 Ethernet0/1 FE80::4 2
no ipv6 route ::/0 Ethernet0/0 FE80::5
!
</pre>
</details>

<details>
  <summary>SW3</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Vlan20
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
router ospf 1
 passive-interface Vlan10
!
ipv6 router ospf 1
 passive-interface Vlan10
!
no ip route *
!
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::5
no ipv6 route ::/0 Ethernet0/0 FE80::4
no ipv6 route ::/0 Ethernet0/1 FE80::5 2
!
</pre>
</details>

<details>
  <summary>SW4</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet1/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet1/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Vlan201
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
router ospf 1
!
ipv6 router ospf 1
!
no ip route *
!
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::2
no ipv6 route 2001:DB8:1001::2/128 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001::2/128 Ethernet1/1 FE80::13 3
no ipv6 route 2001:DB8:1001::2/128 Ethernet1/0 FE80::12 4
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::3 5
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::3
no ipv6 route 2001:DB8:1001::3/128 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001::3/128 Ethernet1/1 FE80::13 3
no ipv6 route 2001:DB8:1001::3/128 Ethernet1/0 FE80::12 4
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::2 5
no ipv6 route 2001:DB8:1001::5/128 Vlan201 FE80::5
no ipv6 route 2001:DB8:1001::5/128 Ethernet1/1 FE80::13 2
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::2 3
no ipv6 route 2001:DB8:1001::5/128 Ethernet1/0 FE80::12 4
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::3 5
no ipv6 route 2001:DB8:1001::12/128 Ethernet1/0 FE80::12
no ipv6 route 2001:DB8:1001::12/128 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001::12/128 Ethernet1/1 FE80::13 3
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::2 4
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::3 5
no ipv6 route 2001:DB8:1001::13/128 Ethernet1/1 FE80::13
no ipv6 route 2001:DB8:1001::13/128 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::2 3
no ipv6 route 2001:DB8:1001::13/128 Ethernet1/0 FE80::12 4
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::3 5
no ipv6 route 2001:DB8:1001::15/128 Ethernet1/1 FE80::13
no ipv6 route 2001:DB8:1001::15/128 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001::15/128 Ethernet1/0 FE80::12 3
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::2 4
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/0 FE80::3 5
no ipv6 route 2001:DB8:1001::20/128 Ethernet1/1 FE80::13
no ipv6 route 2001:DB8:1001::20/128 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001::20/128 Ethernet1/0 FE80::12 3
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::2 4
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/0 FE80::3 5
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::3
no ipv6 route 2001:DB8:1001:10::/64 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001:10::/64 Ethernet1/1 FE80::13 3
no ipv6 route 2001:DB8:1001:10::/64 Ethernet1/0 FE80::12 4
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::2 5
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::2
no ipv6 route 2001:DB8:1001:20::/64 Vlan201 FE80::5 2
no ipv6 route 2001:DB8:1001:20::/64 Ethernet1/1 FE80::13 3
no ipv6 route 2001:DB8:1001:20::/64 Ethernet1/0 FE80::12 4
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::3 5
no ipv6 route ::/0 Ethernet1/0 FE80::12
no ipv6 route ::/0 Ethernet1/1 FE80::13 2
no ipv6 route ::/0 Vlan201 FE80::5 3
no ipv6 route ::/0 Ethernet0/1 FE80::2 4
no ipv6 route ::/0 Ethernet0/0 FE80::3 5
!
</pre>
</details>

<details>
  <summary>SW5</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet1/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet1/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Vlan201
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
router ospf 1
!
ipv6 router ospf 1
!
no ip route *
!
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::2
no ipv6 route 2001:DB8:1001::2/128 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001::2/128 Ethernet1/1 FE80::12 3
no ipv6 route 2001:DB8:1001::2/128 Ethernet1/0 FE80::13 4
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::3 5
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::3
no ipv6 route 2001:DB8:1001::3/128 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001::3/128 Ethernet1/1 FE80::12 3
no ipv6 route 2001:DB8:1001::3/128 Ethernet1/0 FE80::13 4
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::2 5
no ipv6 route 2001:DB8:1001::4/128 Vlan201 FE80::4
no ipv6 route 2001:DB8:1001::4/128 Ethernet1/1 FE80::12 2
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::3 3
no ipv6 route 2001:DB8:1001::4/128 Ethernet1/0 FE80::13 4
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::2 5
no ipv6 route 2001:DB8:1001::12/128 Ethernet1/1 FE80::12
no ipv6 route 2001:DB8:1001::12/128 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::3 3
no ipv6 route 2001:DB8:1001::12/128 Ethernet1/0 FE80::13 4
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::2 5
no ipv6 route 2001:DB8:1001::13/128 Ethernet1/0 FE80::13
no ipv6 route 2001:DB8:1001::13/128 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001::13/128 Ethernet1/1 FE80::12 3
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::3 4
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::2 5
no ipv6 route 2001:DB8:1001::14/128 Ethernet1/1 FE80::12
no ipv6 route 2001:DB8:1001::14/128 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001::14/128 Ethernet1/0 FE80::13 3
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::3 4
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/0 FE80::2 5
no ipv6 route 2001:DB8:1001::19/128 Ethernet1/1 FE80::12
no ipv6 route 2001:DB8:1001::19/128 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001::19/128 Ethernet1/0 FE80::13 3
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::3 4
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/0 FE80::2 5
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::3
no ipv6 route 2001:DB8:1001:10::/64 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001:10::/64 Ethernet1/1 FE80::12 3
no ipv6 route 2001:DB8:1001:10::/64 Ethernet1/0 FE80::13 4
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::2 5
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::2
no ipv6 route 2001:DB8:1001:20::/64 Vlan201 FE80::4 2
no ipv6 route 2001:DB8:1001:20::/64 Ethernet1/1 FE80::12 3
no ipv6 route 2001:DB8:1001:20::/64 Ethernet1/0 FE80::13 4
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::3 5
no ipv6 route ::/0 Ethernet1/0 FE80::13
no ipv6 route ::/0 Ethernet1/1 FE80::12 2
no ipv6 route ::/0 Vlan201 FE80::4 3
no ipv6 route ::/0 Ethernet0/1 FE80::3 4
no ipv6 route ::/0 Ethernet0/0 FE80::2 5
!
</pre>
</details>

<details>
  <summary>R12</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/2
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
router ospf 1
!
ipv6 router ospf 1
!
no ip route *
!
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::4 2
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/3 FE80::15 3
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::4
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::5 2
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/3 FE80::15 3
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::4
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::5 2
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/3 FE80::15 3
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::4 2
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/3 FE80::15 3
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/3 FE80::15 2
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::5 3
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::4 4
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/3 FE80::15
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/2 FE80::14 2
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::5 3
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/0 FE80::4 4
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/3 FE80::15
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/2 FE80::14 2
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::5 3
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/0 FE80::4 4
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::4
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::5 2
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/3 FE80::15 3
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::5
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::4 2
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/3 FE80::15 3
no ipv6 route ::/0 Ethernet0/2 FE80::14
no ipv6 route ::/0 Ethernet0/3 FE80::15 2
no ipv6 route ::/0 Ethernet0/1 FE80::5 3
no ipv6 route ::/0 Ethernet0/0 FE80::4 4
!
</pre>
</details>

<details>
  <summary>R13</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/2
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
router ospf 1
!
ipv6 router ospf 1
!
no ip route *
!
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::5
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::4 2
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/3 FE80::14 3
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::5 2
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/3 FE80::14 3
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::5 2
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/3 FE80::14 3
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::5
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::4 2
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/3 FE80::14 3
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/3 FE80::14
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/2 FE80::15 2
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::5 3
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::4 4
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/3 FE80::14
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/2 FE80::15 2
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/0 FE80::5 3
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::4 4
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/3 FE80::14
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/2 FE80::15 2
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/0 FE80::5 3
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::4 4
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::4
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::5 2
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/3 FE80::14 3
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::5
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::4 2
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/3 FE80::14 3
no ipv6 route ::/0 Ethernet0/2 FE80::15
no ipv6 route ::/0 Ethernet0/3 FE80::14 2
no ipv6 route ::/0 Ethernet0/1 FE80::4 3
no ipv6 route ::/0 Ethernet0/0 FE80::5 4
!
</pre>
</details>

<details>
  <summary>R14</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 101
 ipv6 ospf 1 area 101
 ipv6 ospf network point-to-point
!
router ospf 1
 area 101 stub no-summary
 default-information originate
!
ipv6 router ospf 1
 area 101 stub no-summary
 default-information originate
!
no ip route *
ip route 0.0.0.0 0.0.0.0 101.0.100.0
!
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::13
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::12 2
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::12
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::13 2
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::12
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::13 2
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::13
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::12 2
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::12
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::13 2
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::13
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::12 2
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/1 FE80::13
no ipv6 route 2001:DB8:1001::15/128 Ethernet0/0 FE80::12 2
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/3 FE80::19
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/1 FE80::13
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/0 FE80::12 2
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::12
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::13 2
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::13
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::12 2
no ipv6 route ::/0 Ethernet0/1 FE80::13 2
no ipv6 route ::/0 Ethernet0/0 FE80::12 3
!
</pre>
</details>

<details>
  <summary>R15</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 0
 ipv6 ospf 1 area 0
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/1
 ip ospf network point-to-point
 ip ospf 1 area 10
 ipv6 ospf 1 area 10
 ipv6 ospf network point-to-point
!
interface Ethernet0/3
 ip ospf network point-to-point
 ip ospf 1 area 102
 ipv6 ospf 1 area 102
 ipv6 ospf network point-to-point
!
router ospf 1
 area 102 filter-list prefix FILTER-INTO-AREA-102 in
 default-information originate
!
ip prefix-list FILTER-INTO-AREA-102 seq 10 deny 100.1.0.19/32
ip prefix-list FILTER-INTO-AREA-102 seq 20 deny 172.16.1.4/31
ip prefix-list FILTER-INTO-AREA-102 seq 30 permit 0.0.0.0/0 le 32
!
ipv6 router ospf 1
 area 102 filter-list prefix FILTER-INTO-AREA-102 in
 default-information originate
!
ipv6 prefix-list FILTER-INTO-AREA-102 seq 10 deny 2001:DB8:1001::19/128
ipv6 prefix-list FILTER-INTO-AREA-102 seq 20 permit ::/0 le 128
!
no ip route *
ip route 0.0.0.0 0.0.0.0 30.1.100.0
!
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/0 FE80::13
no ipv6 route 2001:DB8:1001::2/128 Ethernet0/1 FE80::12 2
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/1 FE80::12
no ipv6 route 2001:DB8:1001::3/128 Ethernet0/0 FE80::13 2
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/1 FE80::12
no ipv6 route 2001:DB8:1001::4/128 Ethernet0/0 FE80::13 2
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/0 FE80::13
no ipv6 route 2001:DB8:1001::5/128 Ethernet0/1 FE80::12 2
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/1 FE80::12
no ipv6 route 2001:DB8:1001::12/128 Ethernet0/0 FE80::13 2
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/0 FE80::13
no ipv6 route 2001:DB8:1001::13/128 Ethernet0/1 FE80::12 2
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/1 FE80::12
no ipv6 route 2001:DB8:1001::14/128 Ethernet0/0 FE80::13 2
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/1 FE80::12
no ipv6 route 2001:DB8:1001::19/128 Ethernet0/0 FE80::13 2
no ipv6 route 2001:DB8:1001::20/128 Ethernet0/3 FE80::20
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/1 FE80::12
no ipv6 route 2001:DB8:1001:10::/64 Ethernet0/0 FE80::13 2
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/0 FE80::13
no ipv6 route 2001:DB8:1001:20::/64 Ethernet0/1 FE80::12 2
no ipv6 route ::/0 Ethernet0/1 FE80::12 2
no ipv6 route ::/0 Ethernet0/0 FE80::13 3
!
</pre>
</details>

<details>
  <summary>R19</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 101
 ipv6 ospf 1 area 101
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 101
 ipv6 ospf 1 area 101
 ipv6 ospf network point-to-point
!
router ospf 1
 area 101 stub
!
ipv6 router ospf 1
 area 101 stub
!
no ip route *
!
no ipv6 route ::/0 Ethernet0/0 FE80::14
!
</pre>
</details>

<details>
  <summary>R20</summary>
<pre>
!
interface Loopback1
 ip ospf 1 area 102
 ipv6 ospf 1 area 102
!
interface Ethernet0/0
 ip ospf network point-to-point
 ip ospf 1 area 102
 ipv6 ospf 1 area 102
 ipv6 ospf network point-to-point
!
ipv6 router ospf 1
!
no ip route *
!
no ipv6 route ::/0 Ethernet0/0 FE80::15
!
</pre>
</details>

