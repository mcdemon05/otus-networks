## Задание:

Настроить EIGRP в С.-Петербург;
<br>
Использовать named EIGRP

##  Решение:

- [Конфигурационные файлы;](configs/)
- [Сохраненная топология из EVE-NG;](eve-ng_lab_ISIS.zip)

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

<hr>

### Внесение изменений в конфигурацию и отображение маршрутов:
<details>
  <summary>SW9</summary>
<pre>
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Vlan10
   passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 0.0.0.0
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Vlan10
   passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
 exit-address-family
!
no ip route *
!
no ipv6 route 2001:DB8:2042::10/128 Ethernet1/0 FE80::16 2
no ipv6 route 2001:DB8:2042::10/128 Vlan251 FE80::10
no ipv6 route 2001:DB8:2042::16/128 Ethernet0/3 FE80::17 3
no ipv6 route 2001:DB8:2042::16/128 Vlan251 FE80::10 2
no ipv6 route 2001:DB8:2042::16/128 Ethernet1/0 FE80::16
no ipv6 route 2001:DB8:2042::17/128 Ethernet1/0 FE80::16 3
no ipv6 route 2001:DB8:2042::17/128 Vlan251 FE80::10 2
no ipv6 route 2001:DB8:2042::17/128 Ethernet0/3 FE80::17
no ipv6 route 2001:DB8:2042::32/128 Ethernet0/3 FE80::17 3
no ipv6 route 2001:DB8:2042::32/128 Vlan251 FE80::10 2
no ipv6 route 2001:DB8:2042::32/128 Ethernet1/0 FE80::16
no ipv6 route 2001:DB8:2042:20::/64 Vlan251 FE80::10
no ipv6 route 2001:DB8:2042:20::/64 Ethernet1/0 FE80::16 2
no ipv6 route ::/0 Vlan251 FE80::10 3
no ipv6 route ::/0 Ethernet1/0 FE80::16 2
no ipv6 route ::/0 Ethernet0/3 FE80::17
!
</pre>
</details>
<details>
  <summary>SW9 show ip/ipv6 route</summary>
<pre>
SW9#sh ip route eigrp
...
Gateway of last resort is 172.16.1.10 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/2048000] via 172.16.1.10, 01:36:59, Ethernet0/3
      20.0.0.0/8 is variably subnetted, 9 subnets, 2 masks
D        20.42.0.10/32 [90/10880] via 172.16.1.15, 03:01:53, Vlan251
D        20.42.0.16/32 [90/1024640] via 172.16.1.6, 03:01:53, Ethernet1/0
D        20.42.0.17/32 [90/1024640] via 172.16.1.10, 03:01:58, Ethernet0/3
D        20.42.0.18/32 [90/1536640] via 172.16.1.10, 03:01:53, Ethernet0/3
                       [90/1536640] via 172.16.1.6, 03:01:53, Ethernet1/0
D        20.42.0.32/32 [90/1536640] via 172.16.1.6, 03:01:53, Ethernet1/0
D        20.42.20.0/28 [90/15360] via 172.16.1.15, 03:01:53, Vlan251
      172.16.0.0/16 is variably subnetted, 11 subnets, 2 masks
D        172.16.1.0/31 [90/1536000] via 172.16.1.6, 03:01:53, Ethernet1/0
D        172.16.1.2/31 [90/1536000] via 172.16.1.10, 03:01:53, Ethernet0/3
D        172.16.1.4/31 [90/1029120] via 172.16.1.15, 03:01:53, Vlan251
D        172.16.1.8/31 [90/1536000] via 172.16.1.6, 03:01:53, Ethernet1/0
D        172.16.1.12/31 [90/1029120] via 172.16.1.15, 03:01:53, Vlan251
SW9#sh ipv6 route eigrp
...
EX  ::/0 [170/2048000]
     via FE80::17, Ethernet0/3
D   2001:DB8:2042::10/128 [90/10880]
     via FE80::10, Vlan251
D   2001:DB8:2042::16/128 [90/1024640]
     via FE80::16, Ethernet1/0
D   2001:DB8:2042::17/128 [90/1024640]
     via FE80::17, Ethernet0/3
D   2001:DB8:2042::18/128 [90/1536640]
     via FE80::17, Ethernet0/3
     via FE80::16, Ethernet1/0
D   2001:DB8:2042::32/128 [90/1536640]
     via FE80::16, Ethernet1/0
D   2001:DB8:2042:20::/64 [90/15360]
     via FE80::10, Vlan251
</pre>
</details>

<details>
  <summary>SW10</summary>
<pre>
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Vlan20
   passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 0.0.0.0
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Vlan20
   passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
 exit-address-family
!
no ip route *
!
no ipv6 route 2001:DB8:2042::9/128 Ethernet1/0 FE80::17 2
no ipv6 route 2001:DB8:2042::9/128 Vlan251 FE80::9
no ipv6 route 2001:DB8:2042::16/128 Ethernet1/0 FE80::17 3
no ipv6 route 2001:DB8:2042::16/128 Vlan251 FE80::9 2
no ipv6 route 2001:DB8:2042::16/128 Ethernet0/3 FE80::16
no ipv6 route 2001:DB8:2042::17/128 Ethernet0/3 FE80::16 3
no ipv6 route 2001:DB8:2042::17/128 Vlan251 FE80::9 2
no ipv6 route 2001:DB8:2042::17/128 Ethernet1/0 FE80::17
no ipv6 route 2001:DB8:2042::32/128 Ethernet1/0 FE80::17 2
no ipv6 route 2001:DB8:2042::32/128 Vlan251 FE80::9 2
no ipv6 route 2001:DB8:2042::32/128 Ethernet0/3 FE80::16
no ipv6 route 2001:DB8:2042:10::/64 Ethernet1/0 FE80::17 2
no ipv6 route 2001:DB8:2042:10::/64 Vlan251 FE80::9
no ipv6 route ::/0 Vlan251 FE80::9 3
no ipv6 route ::/0 Ethernet1/0 FE80::17 2
no ipv6 route ::/0 Ethernet0/3 FE80::16
!
</pre>
</details>
<details>
  <summary>SW10 show ip/ipv6 route</summary>
<pre>
SW10#sh ip route eigrp
...
Gateway of last resort is 172.16.1.12 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/2048000] via 172.16.1.12, 01:42:29, Ethernet1/0
      20.0.0.0/8 is variably subnetted, 9 subnets, 2 masks
D        20.42.0.9/32 [90/10880] via 172.16.1.14, 03:07:23, Vlan251
D        20.42.0.16/32 [90/1024640] via 172.16.1.4, 03:07:23, Ethernet0/3
D        20.42.0.17/32 [90/1024640] via 172.16.1.12, 03:07:28, Ethernet1/0
D        20.42.0.18/32 [90/1536640] via 172.16.1.12, 03:07:28, Ethernet1/0
                       [90/1536640] via 172.16.1.4, 03:07:28, Ethernet0/3
D        20.42.0.32/32 [90/1536640] via 172.16.1.4, 03:07:23, Ethernet0/3
D        20.42.10.0/28 [90/15360] via 172.16.1.14, 03:07:23, Vlan251
      172.16.0.0/16 is variably subnetted, 11 subnets, 2 masks
D        172.16.1.0/31 [90/1536000] via 172.16.1.4, 03:07:23, Ethernet0/3
D        172.16.1.2/31 [90/1536000] via 172.16.1.12, 03:07:28, Ethernet1/0
D        172.16.1.6/31 [90/1029120] via 172.16.1.14, 03:07:28, Vlan251
D        172.16.1.8/31 [90/1536000] via 172.16.1.4, 03:07:23, Ethernet0/3
D        172.16.1.10/31 [90/1029120] via 172.16.1.14, 03:07:23, Vlan251
SW10#sh ipv6 route eigrp
...
EX  ::/0 [170/2048000]
     via FE80::17, Ethernet1/0
D   2001:DB8:2042::9/128 [90/10880]
     via FE80::9, Vlan251
D   2001:DB8:2042::16/128 [90/1024640]
     via FE80::16, Ethernet0/3
D   2001:DB8:2042::17/128 [90/1024640]
     via FE80::17, Ethernet1/0
D   2001:DB8:2042::18/128 [90/1536640]
     via FE80::17, Ethernet1/0
     via FE80::16, Ethernet0/3
D   2001:DB8:2042::32/128 [90/1536640]
     via FE80::16, Ethernet0/3
D   2001:DB8:2042:10::/64 [90/15360]
     via FE80::9, Vlan251
</pre>
</details>

<details>
  <summary>R16</summary>
<pre>
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 20.42.0.0 255.255.0.0
   summary-address 172.16.1.0 255.255.255.0
  exit-af-interface
  !
  topology base
   distribute-list prefix FILTER-INTO-R32 out Ethernet0/3
  exit-af-topology
  network 0.0.0.0
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 2001:DB8:2042::/48
  exit-af-interface
  !
  topology base
   distribute-list prefix-list FILTER-INTO-R32 out Ethernet0/3
  exit-af-topology
 exit-address-family
!
no ip route *
!
no ipv6 route 2001:DB8:2042::9/128 Ethernet0/2 FE80::9
no ipv6 route 2001:DB8:2042::9/128 Ethernet0/0 FE80::10 2
no ipv6 route 2001:DB8:2042::10/128 Ethernet0/0 FE80::10
no ipv6 route 2001:DB8:2042::10/128 Ethernet0/2 FE80::9 2
no ipv6 route 2001:DB8:2042::32/128 Ethernet0/3 FE80::32
no ipv6 route 2001:DB8:2042:10::/64 Ethernet0/2 FE80::9
no ipv6 route 2001:DB8:2042:10::/64 Ethernet0/0 FE80::10 2
no ipv6 route 2001:DB8:2042:20::/64 Ethernet0/0 FE80::10
no ipv6 route 2001:DB8:2042:20::/64 Ethernet0/2 FE80::9 2
no ipv6 route ::/0 Ethernet0/1 FE80::18
no ipv6 route ::/0 Ethernet0/2 FE80::9 2
no ipv6 route ::/0 Ethernet0/0 FE80::10 3
!
ip prefix-list FILTER-INTO-R32 seq 10 permit 0.0.0.0/0
!
ipv6 prefix-list FILTER-INTO-R32 seq 10 permit ::/0
!
</pre>
</details>
<details>
  <summary>R16 show ip/ipv6 route</summary>
<pre>
R16#sh ip route eigrp
...
Gateway of last resort is 172.16.1.0 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/1536000] via 172.16.1.0, 00:55:37, Ethernet0/1
      20.0.0.0/8 is variably subnetted, 9 subnets, 3 masks
D        20.42.0.0/16 is a summary, 00:56:49, Null0
D        20.42.0.9/32 [90/1024640] via 172.16.1.7, 00:56:49, Ethernet0/2
D        20.42.0.10/32 [90/1024640] via 172.16.1.5, 00:56:49, Ethernet0/0
D        20.42.0.17/32 [90/1536640] via 172.16.1.7, 00:56:49, Ethernet0/2
                       [90/1536640] via 172.16.1.5, 00:56:49, Ethernet0/0
D        20.42.0.18/32 [90/1024640] via 172.16.1.0, 00:56:45, Ethernet0/1
D        20.42.0.32/32 [90/1024640] via 172.16.1.9, 00:07:47, Ethernet0/3
D        20.42.10.0/28 [90/1029120] via 172.16.1.7, 00:56:49, Ethernet0/2
D        20.42.20.0/28 [90/1029120] via 172.16.1.5, 00:56:49, Ethernet0/0
      172.16.0.0/16 is variably subnetted, 13 subnets, 3 masks
D        172.16.1.0/24 is a summary, 00:56:49, Null0
D        172.16.1.2/31 [90/1536000] via 172.16.1.0, 00:56:49, Ethernet0/1
D        172.16.1.10/31 [90/1536000] via 172.16.1.7, 00:56:49, Ethernet0/2
D        172.16.1.12/31 [90/1536000] via 172.16.1.5, 00:56:49, Ethernet0/0
D        172.16.1.14/31 [90/1029120] via 172.16.1.7, 00:56:49, Ethernet0/2
                        [90/1029120] via 172.16.1.5, 00:56:49, Ethernet0/0
R16#sh ipv6 route eigrp
...
EX  ::/0 [170/1536000]
     via FE80::18, Ethernet0/1
D   2001:DB8:2042::/48 [5/1280]
     via Null0, directly connected
D   2001:DB8:2042::9/128 [90/1024640]
     via FE80::9, Ethernet0/2
D   2001:DB8:2042::10/128 [90/1024640]
     via FE80::10, Ethernet0/0
D   2001:DB8:2042::17/128 [90/1536640]
     via FE80::10, Ethernet0/0
     via FE80::9, Ethernet0/2
D   2001:DB8:2042::18/128 [90/1024640]
     via FE80::18, Ethernet0/1
D   2001:DB8:2042::32/128 [90/1024640]
     via FE80::32, Ethernet0/3
D   2001:DB8:2042:10::/64 [90/1029120]
     via FE80::9, Ethernet0/2
D   2001:DB8:2042:20::/64 [90/1029120]
     via FE80::10, Ethernet0/0
</pre>
</details>

<details>
  <summary>R17</summary>
<pre>
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 20.42.0.0 255.255.0.0
   summary-address 172.16.1.0 255.255.255.0
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 0.0.0.0
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  af-interface Ethernet0/1
   summary-address 2001:DB8:2042::/48
  exit-af-interface
  !
  topology base
  exit-af-topology
 exit-address-family
!
no ip route *
!
no ipv6 route 2001:DB8:2042::9/128 Ethernet0/0 FE80::9
no ipv6 route 2001:DB8:2042::9/128 Ethernet0/2 FE80::10 2
no ipv6 route 2001:DB8:2042::10/128 Ethernet0/2 FE80::10
no ipv6 route 2001:DB8:2042::10/128 Ethernet0/0 FE80::9 2
no ipv6 route 2001:DB8:2042:10::/64 Ethernet0/0 FE80::9
no ipv6 route 2001:DB8:2042:10::/64 Ethernet0/2 FE80::10 2
no ipv6 route 2001:DB8:2042:20::/64 Ethernet0/2 FE80::10
no ipv6 route 2001:DB8:2042:20::/64 Ethernet0/0 FE80::9 2
no ipv6 route ::/0 Ethernet0/1 FE80::18
no ipv6 route ::/0 Ethernet0/2 FE80::10 2
no ipv6 route ::/0 Ethernet0/0 FE80::9 3
!
</pre>
</details>
<details>
  <summary>R17 show ip/ipv6 route</summary>
<pre>
R17#sh ip route eigrp
...
Gateway of last resort is 172.16.1.2 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/1536000] via 172.16.1.2, 01:02:17, Ethernet0/1
      20.0.0.0/8 is variably subnetted, 9 subnets, 3 masks
D        20.42.0.0/16 is a summary, 05:02:03, Null0
D        20.42.0.9/32 [90/1024640] via 172.16.1.11, 05:00:29, Ethernet0/0
D        20.42.0.10/32 [90/1024640] via 172.16.1.13, 05:00:29, Ethernet0/2
D        20.42.0.16/32 [90/1536640] via 172.16.1.13, 00:59:59, Ethernet0/2
                       [90/1536640] via 172.16.1.11, 00:59:59, Ethernet0/0
D        20.42.0.18/32 [90/1024640] via 172.16.1.2, 01:02:17, Ethernet0/1
D        20.42.0.32/32 [90/2048640] via 172.16.1.13, 00:10:58, Ethernet0/2
                       [90/2048640] via 172.16.1.11, 00:10:58, Ethernet0/0
D        20.42.10.0/28 [90/1029120] via 172.16.1.11, 05:00:29, Ethernet0/0
D        20.42.20.0/28 [90/1029120] via 172.16.1.13, 05:00:29, Ethernet0/2
      172.16.0.0/16 is variably subnetted, 12 subnets, 3 masks
D        172.16.1.0/24 is a summary, 05:02:55, Null0
D        172.16.1.0/31 [90/1536000] via 172.16.1.2, 00:59:59, Ethernet0/1
D        172.16.1.4/31 [90/1536000] via 172.16.1.13, 05:00:29, Ethernet0/2
D        172.16.1.6/31 [90/1536000] via 172.16.1.11, 05:00:29, Ethernet0/0
D        172.16.1.8/31 [90/2048000] via 172.16.1.13, 00:59:59, Ethernet0/2
                       [90/2048000] via 172.16.1.11, 00:59:59, Ethernet0/0
D        172.16.1.14/31 [90/1029120] via 172.16.1.13, 05:00:29, Ethernet0/2
                        [90/1029120] via 172.16.1.11, 05:00:29, Ethernet0/0
R17#sh ipv6 route eigrp
...
EX  ::/0 [170/1536000]
     via FE80::18, Ethernet0/1
D   2001:DB8:2042::/48 [5/1280]
     via Null0, directly connected
D   2001:DB8:2042::9/128 [90/1024640]
     via FE80::9, Ethernet0/0
D   2001:DB8:2042::10/128 [90/1024640]
     via FE80::10, Ethernet0/2
D   2001:DB8:2042::16/128 [90/1536640]
     via FE80::10, Ethernet0/2
     via FE80::9, Ethernet0/0
D   2001:DB8:2042::18/128 [90/1024640]
     via FE80::18, Ethernet0/1
D   2001:DB8:2042::32/128 [90/2048640]
     via FE80::9, Ethernet0/0
     via FE80::10, Ethernet0/2
D   2001:DB8:2042:10::/64 [90/1029120]
     via FE80::9, Ethernet0/0
D   2001:DB8:2042:20::/64 [90/1029120]
     via FE80::10, Ethernet0/2
</pre>
</details>

<details>
  <summary>R18</summary>
<pre>
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  topology base
   redistribute static
  exit-af-topology
  network 20.42.0.18 0.0.0.0
  network 172.16.1.0 0.0.0.3
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  topology base
   redistribute static
  exit-af-topology
 exit-address-family
!
no ip route *
ip route 0.0.0.0 0.0.0.0 5.20.24.2
ip route 0.0.0.0 0.0.0.0 5.20.26.2 2
!
no ipv6 route 2001:DB8:2042::9/128 Ethernet0/1 FE80::17
no ipv6 route 2001:DB8:2042::9/128 Ethernet0/0 FE80::16 2
no ipv6 route 2001:DB8:2042::10/128 Ethernet0/0 FE80::16
no ipv6 route 2001:DB8:2042::10/128 Ethernet0/1 FE80::17 2
no ipv6 route 2001:DB8:2042::16/128 Ethernet0/0 FE80::16
no ipv6 route 2001:DB8:2042::16/128 Ethernet0/1 FE80::17 2
no ipv6 route 2001:DB8:2042::17/128 Ethernet0/1 FE80::17
no ipv6 route 2001:DB8:2042::17/128 Ethernet0/0 FE80::16 2
no ipv6 route 2001:DB8:2042::32/128 Ethernet0/0 FE80::16
no ipv6 route 2001:DB8:2042::32/128 Ethernet0/1 FE80::17 2
no ipv6 route 2001:DB8:2042:10::/64 Ethernet0/1 FE80::17
no ipv6 route 2001:DB8:2042:10::/64 Ethernet0/0 FE80::16 2
no ipv6 route 2001:DB8:2042:20::/64 Ethernet0/0 FE80::16
no ipv6 route 2001:DB8:2042:20::/64 Ethernet0/1 FE80::17 2
!
</pre>
</details>
<details>
  <summary>R18 show ip/ipv6 route</summary>
<pre>
R18#sh ip route eigrp
...
Gateway of last resort is 5.20.24.2 to network 0.0.0.0

      20.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
D        20.42.0.0/16 [90/1024640] via 172.16.1.3, 01:01:47, Ethernet0/1
                      [90/1024640] via 172.16.1.1, 01:01:47, Ethernet0/0
      172.16.0.0/16 is variably subnetted, 5 subnets, 3 masks
D        172.16.1.0/24 [90/1536000] via 172.16.1.3, 01:01:47, Ethernet0/1
                       [90/1536000] via 172.16.1.1, 01:01:47, Ethernet0/0
R18#sh ipv6 route eigrp
...
D   2001:DB8:2042::/48 [90/1024640]
     via FE80::17, Ethernet0/1
     via FE80::16, Ethernet0/0
</pre>
</details>

<details>
  <summary>R32</summary>
<pre>
!
router eigrp SPB
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  topology base
  exit-af-topology
  network 0.0.0.0
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2042
  !
  topology base
  exit-af-topology
 exit-address-family
!
no ip route *
!
no ipv6 route ::/0 Ethernet0/0 FE80::16
!
</pre>
</details>
<details>
  <summary>R32 show ip/ipv6 route</summary>
<pre>
R32#sh ip route eigrp
...
Gateway of last resort is 172.16.1.8 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/2048000] via 172.16.1.8, 00:17:31, Ethernet0/0
R32#sh ipv6 route eigrp
...
EX  ::/0 [170/2048000]
     via FE80::16, Ethernet0/0
</pre>
</details>