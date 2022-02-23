## Задание:

1. Настроить DHCPv4
2. Настроить DHCPv6

##  Решение:

- [Конфигурационные файлы;](configs/)
- [Сохраненная топология из EVE-NG;](eve-ng_lab_DHCP.zip)

### Графическая схема

![](Topology.PNG)

### Таблица VLAN

| VLAN |     Name    | Interface Assigned |
|:----:|:-----------:|:------------------:|
| 1    | N/A         | S2: e0/0-3         |
| 100  | Clients     | S1: e0/2           |
| 200  | Management  | S1: VLAN 200       |
| 999  | Parking_Lot | S1: e0/0-1         |
| 1000 | Native      |                    |

<details>
  <summary>S1:</summary>
<pre>
!
vlan 100
 name Clients
!
vlan 200
 name Management
!
vlan 999
 name ParkingLot
!
vlan 1000
 name Native
!
!
interface Ethernet0/0
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface Ethernet0/1
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 100
 switchport mode access
!
interface Ethernet0/3
 no shutdown
 switchport trunk allowed vlan 100,200,999,1000
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 1000
 switchport mode trunk
 switchport nonegotiate
!
interface Vlan200
 no shutdown
 ip address 192.168.1.66 255.255.255.192
!
ip default-gateway 192.168.1.65
!
</pre>
</details>

<details>
  <summary>S2:</summary>
<pre>
!
interface Ethernet0/0
 switchport mode access
 shutdown
!
interface Ethernet0/1
 switchport mode access
 shutdown
!
interface Ethernet0/2
 no shutdown
!
interface Ethernet0/3
 no shutdown
!
interface Vlan1
 no shutdown
 ip address 192.168.1.98 255.255.255.240
!
ip default-gateway 192.168.1.97
!
</pre>
</details>

------------

<h3>№ 1. DHCPv4</h3>

------------

|  Device   | Interface |  IP Address  |   Subnet Mask   | Default Gateway |
|:---------:|:---------:|:------------:|:---------------:|:---------------:|
| R1        | e0/0      | 10.0.0.1     | 255.255.255.252 |                 |
|           | e0/1      |              |                 |                 |
|           | e0/1.100  | 192.168.1.1  | 255.255.255.192 |                 |
|           | e0/1.200  | 192.168.1.65 | 255.255.255.224 |                 |
|           | e0/1.1000 |              |                 |                 |
| R2        | e0/0      | 10.0.0.2     | 255.255.255.252 |                 |
|           | e0/1      | 192.168.1.97 | 255.255.255.240 |                 |
| S1        | VLAN 200  | 192.168.1.66 | 255.255.255.224 | 192.168.1.65    |
| S2        | VLAN 1    | 192.168.1.98 | 255.255.255.240 | 192.168.1.97    |
| R-Client1 | e0/0      | DHCP         | DHCP            | DHCP            |
| R-Client2 | e0/0      | DHCP         | DHCP            | DHCP            |

<details>
  <summary>R1:</summary>
<pre>
!
ip dhcp excluded-address 192.168.1.1 192.168.1.5
ip dhcp excluded-address 192.168.1.97 192.168.1.101
!
ip dhcp pool R1-Client_LAN
 network 192.168.1.0 255.255.255.192
 default-router 192.168.1.1 
 domain-name ccna-lab.com
 lease 2 12 30
!
ip dhcp pool R2-Client_LAN
 network 192.168.1.96 255.255.255.240
 default-router 192.168.1.97 
 domain-name ccna-lab.com
 lease 2 12 30
!
!
interface Ethernet0/0
 no shutdown
 ip address 10.0.0.1 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 no ip address
!
interface Ethernet0/1.100
 no shutdown
 description Clients
 encapsulation dot1Q 100
 ip address 192.168.1.1 255.255.255.192
!
interface Ethernet0/1.200
 no shutdown
 description Management
 encapsulation dot1Q 200
 ip address 192.168.1.65 255.255.255.224
!
interface Ethernet0/1.1000
 no shutdown
 description Native
 encapsulation dot1Q 1000 native
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
!
ip route 0.0.0.0 0.0.0.0 10.0.0.2
!
</pre>
</details>

<details>
  <summary>R2:</summary>
<pre>
!
interface Ethernet0/0
 no shutdown
 ip address 10.0.0.2 255.255.255.252
!
interface Ethernet0/1
 no shutdown
 ip address 192.168.1.97 255.255.255.240
 ip helper-address 10.0.0.1
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
!
ip route 0.0.0.0 0.0.0.0 10.0.0.1
!
</pre>
</details>

<details>
  <summary>R-Client1:</summary>
<pre>
!
interface Ethernet0/0
 no shutdown
 ip address dhcp
!
</pre>
</details>
<blockquote><pre>
R-Client1# sh ip int e0/0
Ethernet0/0 is up, line protocol is up
  Internet address is 192.168.1.6/26
  Broadcast address is 255.255.255.255
  Address determined by DHCP
...
R-Client1#sh ip dns view
...
DNS Resolver settings:
  Default domain name: ccna-lab.com
...
</pre></blockquote>

<details>
  <summary>R-Client2:</summary>
<pre>
!
interface Ethernet0/0
 no shutdown
 ip address dhcp
!
</pre>
</details>
<blockquote><pre>
R-Client2#sh ip int e0/0
Ethernet0/0 is up, line protocol is up
  Internet address is 192.168.1.102/28
  Broadcast address is 255.255.255.255
  Address determined by DHCP
...
R-Client2#sh ip dns view
...
DNS Resolver settings:
  Default domain name: ccna-lab.com
...
</pre></blockquote>

------------

<h3>2. DHCPv6</h3>

------------


|  Device   | Interface |      IPv6 Address      |
|:---------:|:---------:|:----------------------:|
| R1        | e0/0      | 2001:db8:acad:2::1 /64 |
|           | e0/0      | fe80::1                |
|           | e0/1      | 2001:db8:acad:1::1/64  |
|           | e0/1      | fe80::1                |
| R2        | e0/0      | 2001:db8:acad:2::2/64  |
|           | e0/0      | fe80::2                |
|           | e0/1      | 2001:db8:acad:3::1 /64 |
|           | e0/1      | fe80::1                |
| R-Client1 | e0/0      | DHCP                   |
| R-Client2 | e0/0      | DHCP                   |

<details>
  <summary>R1:</summary>
<pre>
!
ipv6 unicast-routing
ipv6 dhcp pool R1-STATELESS
 dns-server 2001:DB8:ACAD::254
 domain-name STATELESS.com
!
ipv6 dhcp pool R2-STATEFUL
 address prefix 2001:DB8:ACAD:3:AAA::/80
 dns-server 2001:DB8:ACAD::254
 domain-name STATEFUL.com
!
!
interface Ethernet0/0
 no shutdown
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:2::1/64
 ipv6 dhcp server R2-STATEFUL
!
interface Ethernet0/1
 no shutdown
!
interface Ethernet0/1.100
 no shutdown
 description Clients
 encapsulation dot1Q 100
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
 ipv6 nd other-config-flag
 ipv6 dhcp server R1-STATELESS
!
!
ipv6 route ::/0 2001:DB8:ACAD:2::2
!
</pre>
</details>

<details>
  <summary>R2:</summary>
<pre>
!
ipv6 unicast-routing
!
!
interface Ethernet0/0
 no shutdown
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD:2::2/64
!
interface Ethernet0/1
 no shutdown
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:3::1/64
 ipv6 nd managed-config-flag
 ipv6 dhcp relay destination 2001:DB8:ACAD:2::1 Ethernet0/0
!
!
ipv6 route ::/0 2001:DB8:ACAD:2::1
!
</pre>
</details>

<details>
  <summary>R-Client1:</summary>
<pre>
!
interface Ethernet0/0
 no shutdown
 ipv6 address dhcp
 ipv6 address autoconfig
!
</pre>
</details>
<blockquote><pre>
R-Client1#sh ipv6 int
Ethernet0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::A8BB:CCFF:FE00:5000
  No Virtual link-local address(es):
  Stateless address autoconfig enabled
  Global unicast address(es):
    2001:DB8:ACAD:1:A8BB:CCFF:FE00:5000, subnet is 2001:DB8:ACAD:1::/64 [EUI/CAL/PRE]
...
R-Client1#sh ipv6 dhcp interface
Ethernet0/0 is in client mode
  Prefix State is IDLE (0)
  Address State is SOLICIT (51)
...
      DNS server: 2001:DB8:ACAD::254
      Domain name: STATELESS.com
...
</pre></blockquote>

<details>
  <summary>R-Client2:</summary>
<pre>
!
interface Ethernet0/0
 no shutdown
 ipv6 address dhcp
 ipv6 address autoconfig
!
</pre>
</details>
<blockquote><pre>
R-Client2#sh ipv6 int
Ethernet0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::A8BB:CCFF:FE00:7000
  No Virtual link-local address(es):
  Stateless address autoconfig enabled
  Global unicast address(es):
    2001:DB8:ACAD:3:AAA:CDD8:90D6:54B4, subnet is 2001:DB8:ACAD:3:AAA:CDD8:90D6:54B4/128
    2001:DB8:ACAD:3:A8BB:CCFF:FE00:7000, subnet is 2001:DB8:ACAD:3::/64 [EUI/CAL/PRE]
...
sh ipv6 dhcp interface
Ethernet0/0 is in client mode
  Prefix State is IDLE
  Address State is OPEN
...
        Address: 2001:DB8:ACAD:3:AAA:CDD8:90D6:54B4/128
...
      DNS server: 2001:DB8:ACAD::254
      Domain name: STATEFUL.com
...
</pre></blockquote>