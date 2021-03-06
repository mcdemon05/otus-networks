## Задание:

Настроить GRE между офисами Москва и С.-Петербург
<br>
Настроить DMVPN между офисами Москва и Чокурдах, Лабытнанги

##  Решение:

- [Конфигурационные файлы;](configs/)
- [Сохраненная топология из EVE-NG;](eve-ng_lab_VPN.zip)

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

| **Device** | **Interface**                                         | **IPv4 Address**                                                                                                    | **IPv6 Address**                                                                                                                                       |
|------------|-------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| **VPC1**   | eth0                                                  | 192.168.10.2/28 gw 192.168.10.1 (DHCP)                                                                              | 2001:DB8:1001:10::/64 (SLAAC)                                                                                                                          |
| **VPC7**   | eth0                                                  | 192.168.20.2/28 gw 192.168.20.1 (DHCP)                                                                              | 2001:DB8:1001:20::/64 (SLAAC)                                                                                                                          |
| **SW2**    | Lo1<br>e0/0<br>e0/1<br>vlan20                         | 100.1.0.2/32<br>172.16.1.27/31<br>172.16.1.23/31<br>192.168.20.1/28                                                 | 2001:DB8:1001::2/128<br>FE80::2 link-local<br>FE80::2 link-local<br>2001:DB8:1001:20::1/64                                                             |
| **SW3**    | Lo1<br>e0/0<br>e0/1<br>vlan10                         | 100.1.0.3/32<br>172.16.1.21/31<br>172.16.1.29/31<br>192.168.10.1/28                                                 | 2001:DB8:1001::3/128<br>FE80::3 link-local<br>FE80::3 link-local<br>2001:DB8:1001:10::1/64                                                             |
| **SW4**    | Lo1<br>e0/0<br>e0/1<br>e1/0<br>e1/1<br>vlan201        | 100.1.0.4/32<br>172.16.1.20/31<br>172.16.1.22/31<br>172.16.1.13/31<br>172.16.1.19/31<br>172.16.1.24/31              | 2001:DB8:1001::4/128<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local<br>FE80::4 link-local                     |
| **SW5**    | Lo1<br>e0/0<br>e0/1<br>e1/0<br>e1/1<br>vlan201        | 100.1.0.5/32<br>172.16.1.26/31<br>172.16.1.28/31<br>172.16.1.17/31<br>172.16.1.15/31<br>172.16.1.25/31              | 2001:DB8:1001::5/128<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local<br>FE80::5 link-local                     |
| **R12**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3                   | 100.1.0.12/32<br>172.16.1.12/31<br>172.16.1.14/31<br>172.16.1.1/31<br>172.16.1.9/31                                 | 2001:DB8:1001::12/128<br>FE80::12 link-local<br>FE80::12 link-local<br>FE80::12 link-local<br>FE80::12 link-local                                      |
| **R13**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3                   | 100.1.0.13/32<br>172.16.1.16/31<br>172.16.1.18/31<br>172.16.1.7/31<br>172.16.1.3/31                                 | 2001:DB8:1001::13/128<br>FE80::13 link-local<br>FE80::13 link-local<br>FE80::13 link-local<br>FE80::13 link-local                                      |
| **R14**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3<br>Tun0<br>Tun100 | 100.1.0.14/32<br>172.16.1.0/31<br>172.16.1.2/31<br>101.0.100.1/31<br>172.16.1.4/31<br>10.65.0.1/24<br>10.100.0.2/31 | 2001:DB8:1001::14/128<br>FE80::14 link-local<br>FE80::14 link-local<br>FE80::14 link-local, 2001:DB8:101:22E0::14/112<br>FE80::14 link-local<br>-<br>- |
| **R15**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3<br>Tun0<br>Tun100 | 100.1.0.15/32<br>172.16.1.6/31<br>172.16.1.8/31<br>30.1.100.1/31<br>172.16.1.10/31<br>10.64.0.1/24<br>10.100.0.0/31 | 2001:DB8:1001::15/128<br>FE80::15 link-local<br>FE80::15 link-local<br>FE80::15 link-local, 2001:DB8:301:21E0::15/112<br>FE80::15 link-local<br>-<br>- |
| **R19**    | Lo1<br>e0/0                                           | 100.1.0.19/32<br>172.16.1.5/31                                                                                      | 2001:DB8:1001::19/128<br>FE80::19 link-local                                                                                                           |
| **R20**    | Lo1<br>e0/0                                           | 100.1.0.20/32<br>172.16.1.11/31                                                                                     | 2001:DB8:1001::20/128<br>FE80::20 link-local                                                                                                           |

</details>

<details>
  <summary>AS1001 Чокурдах</summary>

| **Device** | **Interface**                               | **IPv4 Address**                                                                            | **IPv6 Address**                                                                                     |
|------------|---------------------------------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| **VPC30**  | eth0                                        | 192.168.10.2/28 gw 192.168.10.1                                                             | 2001:DB8:1001:A10::/64 (SLAAC)                                                                       |
| **VPC31**  | eth0                                        | 192.168.20.2/28 gw 192.168.20.1                                                             | 2001:DB8:1001:A20::/64 (SLAAC)                                                                       |
| **R28**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>Tun0<br>Tun1 | 100.1.1.28<br>5.20.26.1/31<br>5.20.25.3/31<br>172.16.1.0/31<br>10.64.0.2/24<br>10.65.0.2/24 | 2001:DB8:1001:AA1::28<br>FE80::28 link-local<br>FE80::28 link-local<br>FE80::28 link-local<br>-<br>- |
| **SW29**   | Lo1<br>e0/2<br>vlan10<br>vlan20             | 100.1.1.29<br>172.16.1.1/31<br>192.168.10.17/28<br>192.168.20.17/28                         | 2001:DB8:1001:AA1::29<br>FE80::29 link-local<br>2001:DB8:1001:A10::1/64<br>2001:DB8:1001:A20::1/64   |

</details>

<details>
  <summary>AS1001 Лабытнанги</summary>

| **Device** | **Interface**               | **IPv4 Address**                                              | **IPv6 Address**                                           |
|------------|-----------------------------|---------------------------------------------------------------|------------------------------------------------------------|
| **R27**    | Lo1<br>e0/0<br>Tun0<br>Tun1 | 100.1.2.27/32<br>5.20.25.1/31<br>10.64.0.3/24<br>10.65.0.3/24 | 2001:DB8:1001:BB2::27/128<br>FE80::27 link-local<br>-<br>- |

</details>

<details>
  <summary>AS2042 С.-Петербург</summary>

| **Device** | **Interface**                                           | **IPv4 Address**                                                                                                  | **IPv6 Address**                                                                                                                                                                  |
|------------|---------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **VPC**    | eth0                                                    | 192.168.10.2/28 gw 192.168.10.1                                                                                   | 2001:DB8:2042:10::/64 (SLAAC)                                                                                                                                                     |
| **VPC8**   | eth0                                                    | 192.168.20.2/28 gw 192.168.20.1                                                                                   | 2001:DB8:2042:20::/64 (SLAAC)                                                                                                                                                     |
| **SW9**    | Lo1<br>e0/3<br>e1/0<br>vlan10<br>vlan251                | 20.42.0.9/32<br>172.16.1.11/31<br>172.16.1.7/31<br>192.168.10.1/28<br>172.16.1.14/31                              | 2001:DB8:2042::9/128<br>FE80::9 link-local<br>FE80::9 link-local<br>2001:DB8:2042:10::1/64<br>FE80::9 link-local                                                                  |
| **SW10**   | Lo1<br>e0/3<br>e1/0<br>vlan20<br>vlan251                | 100.1.0.10/32<br>172.16.1.5/31<br>172.16.1.13/31<br>192.168.20.1/28<br>172.16.1.15/31                             | 2001:DB8:2042::10/128<br>FE80::10 link-local<br>FE80::10 link-local<br>2001:DB8:2042:20::1/64<br>FE80::10 link-local                                                              |
| **R16**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3                     | 20.42.0.16/32<br>172.16.1.4/31<br>172.16.1.1/31<br>172.16.1.6/31<br>172.16.1.8/31                                 | 2001:DB8:2042::16/128<br>FE80::16 link-local<br>FE80::16 link-local<br>FE80::16 link-local<br>FE80::16 link-local                                                                 |
| **R17**    | Lo1<br>e0/0<br>e0/1<br>e0/2                             | 20.42.0.17/32<br>172.16.1.10/31<br>172.16.1.3/31<br>172.16.1.12/31                                                | 2001:DB8:2042::17/128<br>FE80::17 link-local<br>FE80::17 link-local<br>FE80::17 link-local                                                                                        |
| **R18**    | Lo1<br>e0/0<br>e0/1<br>e0/2<br>e0/3<br>Tun100<br>Tun101 | 20.42.0.18/32<br>172.16.1.0/31<br>172.16.1.2/31<br>5.20.24.3/31<br>5.20.26.3/31<br>10.100.0.1/31<br>10.100.0.3/31 | 2001:DB8:2042::18/128<br>FE80::18 link-local<br>FE80::18 link-local<br>FE80::18 link-local, 2001:DB8:520:24E3::18/112<br>FE80::18 link-local, 2001:DB8:520:26E3::18/112<br>-<br>- |
| **R32**    | Lo1<br>e0/0                                             | 20.42.0.32/32<br>172.16.1.9/31                                                                                    | 2001:DB8:2042::32/128<br>FE80::32 link-local                                                                                                                                      |

</details>

<hr>

### Внесение изменений в конфигурацию:
<details>
  <summary>R14</summary>
<pre>
!
interface Tunnel0
 no shutdown
 ip address 10.65.0.1 255.255.255.0
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast dynamic
 ip nhrp network-id 1001
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel mode gre multipoint
!
interface Tunnel100
 no shutdown
 ip address 10.100.0.2 255.255.255.254
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel destination 20.42.0.18
!
router bgp 1001
 neighbor 10.65.0.2 remote-as 1001
 neighbor 10.100.0.3 remote-as 2042
 !
 address-family ipv4
  network 192.168.10.0
  network 192.168.20.0
  neighbor 10.65.0.2 next-hop-self
  neighbor 10.65.0.2 prefix-list local_only out
  neighbor 10.100.0.3 prefix-list local_only out
  neighbor 101.0.100.0 prefix-list self_summary_only out
 exit-address-family
 !
!
ip prefix-list local_only seq 10 permit 192.168.10.0/24
ip prefix-list local_only seq 11 permit 192.168.20.0/24
ip prefix-list local_only seq 15 deny 0.0.0.0/0
ip prefix-list local_only seq 20 deny 0.0.0.0/0 ge 1
!
ip prefix-list self_summary_only seq 10 permit 100.1.0.0/16
ip prefix-list self_summary_only seq 20 deny 0.0.0.0/0 ge 1
!
</pre>
</details>
<details>
  <summary>R15</summary>
<pre>
!
interface Tunnel0
 no shutdown
 ip address 10.64.0.1 255.255.255.0
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast dynamic
 ip nhrp network-id 1001
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel mode gre multipoint
!
interface Tunnel100
 no shutdown
 ip address 10.100.0.0 255.255.255.254
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel destination 20.42.0.18
!
router bgp 1001
 neighbor 10.64.0.2 remote-as 1001
 neighbor 10.100.0.1 remote-as 2042
 !
 address-family ipv4
  network 192.168.10.0
  network 192.168.20.0
  neighbor 10.64.0.2 next-hop-self
  neighbor 10.64.0.2 prefix-list local_only out
  neighbor 10.100.0.1 prefix-list local_only out
  neighbor 30.1.100.0 prefix-list self_summary_only out
 exit-address-family
 !
!
ip prefix-list local_only seq 10 permit 192.168.10.0/24
ip prefix-list local_only seq 11 permit 192.168.20.0/24
ip prefix-list local_only seq 15 deny 0.0.0.0/0
ip prefix-list local_only seq 20 deny 0.0.0.0/0 ge 1
!
ip prefix-list self_summary_only seq 10 permit 100.1.0.0/16
ip prefix-list self_summary_only seq 20 deny 0.0.0.0/0 ge 1
!
</pre>
</details>

<details>
  <summary>R18</summary>
<pre>
!
interface Tunnel100
 no shutdown
 ip address 10.100.0.1 255.255.255.254
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel destination 100.1.0.15
!
interface Tunnel101
 no shutdown
 ip address 10.100.0.3 255.255.255.254
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel destination 100.1.0.14
!
router bgp 2042
 neighbor 10.100.0.0 remote-as 1001
 neighbor 10.100.0.2 remote-as 1001
 !
 address-family ipv4
  network 192.168.110.0
  network 192.168.120.0
  neighbor 10.100.0.0 prefix-list local_only out
  neighbor 10.100.0.2 prefix-list local_only out
 exit-address-family
 !
!
no ip access-list standard NAT
ip access-list standard NAT
 permit 192.168.110.0 0.0.0.255
 permit 192.168.120.0 0.0.0.255
!
ip prefix-list local_only seq 10 permit 192.168.110.0/24
ip prefix-list local_only seq 11 permit 192.168.120.0/24
ip prefix-list local_only seq 15 deny 0.0.0.0/0
ip prefix-list local_only seq 20 deny 0.0.0.0/0 ge 1
!
</pre>
</details>

<details>
  <summary>SW9</summary>
<pre>
!
interface Vlan10
 ip address 192.168.110.1 255.255.255.0
!
</pre>
</details>

<details>
  <summary>SW10</summary>
<pre>
!
interface Vlan20
 ip address 192.168.120.1 255.255.255.0
!
</pre>
</details>

<details>
  <summary>R27</summary>
<pre>
!
interface Tunnel0
 no shutdown
 ip address 10.64.0.3 255.255.255.0
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast 100.1.0.15
 ip nhrp map 10.64.0.1 100.1.0.15
 ip nhrp network-id 1001
 ip nhrp nhs 10.64.0.1
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel mode gre multipoint
!
interface Tunnel1
 no shutdown
 ip address 10.65.0.3 255.255.255.0
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast 100.1.0.14
 ip nhrp map 10.65.0.1 100.1.0.14
 ip nhrp network-id 1001
 ip nhrp nhs 10.65.0.1
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel mode gre multipoint
!
</pre>
</details>

<details>
  <summary>R28</summary>
<pre>
!
interface Tunnel0
 no shutdown
 ip address 10.64.0.2 255.255.255.0
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast 100.1.0.15
 ip nhrp map 10.64.0.1 100.1.0.15
 ip nhrp network-id 1001
 ip nhrp nhs 10.64.0.1
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel mode gre multipoint
!
interface Tunnel1
 no shutdown
 ip address 10.65.0.2 255.255.255.0
 ip mtu 1400
 ip nhrp authentication OTUS
 ip nhrp map multicast 100.1.0.14
 ip nhrp map 10.65.0.1 100.1.0.14
 ip nhrp network-id 1001
 ip nhrp nhs 10.65.0.1
 ip tcp adjust-mss 1360
 tunnel source Loopback1
 tunnel mode gre multipoint
!
router bgp 1001
 neighbor 10.64.0.1 remote-as 1001
 neighbor 10.65.0.1 remote-as 1001
 !
 address-family ipv4
  network 192.168.210.0
  network 192.168.220.0
  neighbor 10.64.0.1 next-hop-self
  neighbor 10.64.0.1 prefix-list local_only out
  neighbor 10.65.0.1 next-hop-self
  neighbor 10.65.0.1 prefix-list local_only out
 exit-address-family
!
no ip nat inside source static 192.168.10.2 100.1.1.102
no ip nat inside source static 192.168.20.2 100.1.1.202
ip nat inside source static 192.168.210.2 100.1.1.102
ip nat inside source static 192.168.220.2 100.1.1.202

no ip route 192.168.0.0 255.255.0.0 172.16.1.1
ip route 192.168.210.0 255.255.255.0 172.16.1.1
ip route 192.168.220.0 255.255.255.0 172.16.1.1
!
ip prefix-list local_only seq 10 permit 192.168.210.0/24
ip prefix-list local_only seq 11 permit 192.168.220.0/24
ip prefix-list local_only seq 15 deny 0.0.0.0/0
ip prefix-list local_only seq 20 deny 0.0.0.0/0 ge 1
!
</pre>
</details>

<details>
  <summary>SW29</summary>
<pre>
!
interface Vlan10
 ip address 192.168.210.1 255.255.255.0
!
interface Vlan20
 ip address 192.168.220.1 255.255.255.0
!
</pre>
</details>