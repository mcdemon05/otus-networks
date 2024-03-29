## Задание:

Проектная работа по теме:
<br>
Организация сети до удаленных филиалов компании

##  Решение:

- [Конфигурационные файлы;](configs/)
- [Сохраненная топология из EVE-NG;](eve-ng_lab_Poject.zip)

### Графическая схема

![](Topology.png)

### Адресное пространство:

| **Автономка**       | **IPv4 подсети**   |
|---------------------|--------------------|
| AS100200 (Контора)  | 100.200.0.0/19     |
| AS110 (Провайдер1)  | 1.10.0.0/16        |
| AS220 (Провайдер2)  | 2.20.0.0/16        |

### IP интерфейсы:

<details>
  <summary>AS100200 Москва</summary>

| **Device**   | **Interface**                                                             | **IPv4 Address**                                                                                                                  |
|--------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| **R1**       | Lo0<br>e0/0<br>e0/1<br>e0/2                                               | 100.200.0.1/32<br>10.1.2.1/24<br>10.1.3.1/24<br>2.20.99.1/31                                                                      |
| **R2**       | Lo0<br>e0/0<br>e0/1<br>e0/2                                               | 100.200.0.2/32<br>10.1.2.2/24<br>10.2.4.2/24<br>1.10.99.1/31                                                                      |
| **R3**       | Lo0<br>e0/0<br>e0/1<br>e0/3<br>e1/0.10<br>e1/0.100<br>e1/1.10<br>e1/1.100 | 100.200.0.3/32<br>10.3.4.3/24<br>10.1.3.3/24<br>10.3.5.3/24<br>192.168.0.1/24<br>10.3.25.3/24<br>100.200.1.193/26<br>10.3.27.3/24 |
| **R4**       | Lo0<br>e0/0<br>e0/1<br>e0/3<br>e1/0.10<br>e1/0.100<br>e1/1.10<br>e1/1.100 | 100.200.0.4/32<br>10.3.4.4/24<br>10.2.4.4/24<br>10.4.6.4/24<br>192.168.1.1/24<br>10.4.26.4/24<br>100.200.1.129/26<br>10.4.28.4/24 |
| **R5**       | Lo1<br>e0/3                                                               | 100.200.0.5/32<br>10.3.5.5/24                                                                                                     |
| **R6**       | Lo1<br>Tun0<br>e0/3                                                       | 100.200.0.6/32<br>10.255.0.1/24<br>10.4.6.6/24                                                                                    |
| **SW25**     | vlan100                                                                   | 10.3.25.25/24                                                                                                                     |
| **SW26**     | vlan100                                                                   | 10.4.26.26/24                                                                                                                     |
| **SW27**     | vlan100                                                                   | 10.3.27.27/24                                                                                                                     |
| **SW28**     | vlan100                                                                   | 10.4.28.28/24                                                                                                                     |
| **msk-srv1** | eth0                                                                      | 100.200.1.254/26                                                                                                                  |
| **msk-srv2** | eth0                                                                      | 100.200.1.253/26                                                                                                                  |
| **msk-srv3** | eth0                                                                      | 100.200.1.190/26                                                                                                                  |
| **msk-srv4** | eth0                                                                      | 100.200.1.189/26                                                                                                                  |
| **VPC39**    | eth0                                                                      | DHCP (192.168.0.0/24)                                                                                                             |
| **VPC40**    | eth0                                                                      | DHCP (192.168.0.0/24)                                                                                                             |
| **VPC43**    | eth0                                                                      | DHCP (192.168.1.0/24)                                                                                                             |
| **VPC44**    | eth0                                                                      | DHCP (192.168.1.0/24)                                                                                                             |

</details>

<details>
  <summary>AS100200 Санкт-Петербург</summary>

| **Device**   | **Interface**                                                     | **IPv4 Address**                                                                                                   |
|--------------|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| **R7**       | Lo0<br>e0/0<br>e0/1<br>e0/3                                       | 100.200.2.7/32<br>10.7.8.7/24<br>1.10.98.1/31<br>2.20.98.1/31                                                      |
| **R8**       | Lo0<br>e0/0<br>e0/1<br>e0/2.10<br>e0/2.100<br>e0/3.10<br>e0/3.100 | 100.200.2.8/32<br>10.7.8.8/24<br>10.8.9.8/24<br>192.168.2.1/24<br>10.8.33.8/24<br>100.200.2.193/26<br>10.8.29.8/24 |
| **R9**       | Lo0<br>Tun0<br>e0/1                                               | 100.200.2.9/32<br>10.255.0.2/24<br>10.8.9.9/24                                                                     |
| **SW29**     | vlan100                                                           | 10.8.29.29/24                                                                                                      |
| **SW33**     | vlan100                                                           | 10.8.33.33/24                                                                                                      |
| **spb-srv1** | eth0                                                              | 100.200.2.254/26                                                                                                   |
| **spb-srv2** | eth0                                                              | 100.200.2.253/26                                                                                                   |
| **VPC45**    | eth0                                                              | DHCP (192.168.2.0/24)                                                                                              |
| **VPC46**    | eth0                                                              | DHCP (192.168.2.0/24)                                                                                              |

</details>

<details>
  <summary>AS100200 Новосибирск</summary>

| **Device**   | **Interface**                                             | **IPv4 Address**                                                                                          |
|--------------|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **R10**      | Lo0<br>e0/0.10<br>e0/0.100<br>e0/1<br>e0/3.10<br>e0/3.100 | 100.200.3.10/32<br>192.168.3.1/24<br>10.10.31.10/24<br>2.20.97.1/31<br>100.200.3.193/26<br>10.10.30.10/24 |
| **SW30**     | vlan100                                                   | 10.10.30.30/24                                                                                            |
| **SW31**     | vlan100                                                   | 10.10.31.31/24                                                                                            |
| **nsk-srv1** | eth0                                                      | 100.200.3.254/26                                                                                          |
| **VPC50**    | eth0                                                      | DHCP (192.168.3.0/24)                                                                                     |

</details>

<details>
  <summary>AS100200 Владивосток</summary>

| **Device**   | **Interface**                                             | **IPv4 Address**                                                                                          |
|--------------|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **R11**      | Lo0<br>e0/0.10<br>e0/0.100<br>e0/1.10<br>e0/1.100<br>e0/3 | 100.200.4.11/32<br>100.200.4.193/26<br>10.11.32.11/24<br>192.168.4.1/24<br>10.11.34.11/24<br>1.10.96.1/31 |
| **SW32**     | vlan100                                                   | 10.11.32.32/24                                                                                            |
| **SW34**     | vlan100                                                   | 10.11.34.34/24                                                                                            |
| **vdk-srv1** | eth0                                                      | 100.200.4.254/26                                                                                          |
| **VPC52**    | eth0                                                      | DHCP (192.168.4.0/24)                                                                                     |

</details>

<details>
  <summary>AS110 Провайдер1</summary>

| **Device** | **Interface**                       | **IPv4 Address**                                                              |
|------------|-------------------------------------|-------------------------------------------------------------------------------|
| **R12**    | Lo0<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 1.10.0.12/32<br>1.10.255.0/31<br>1.10.98.0/31<br>1.10.99.0/31<br>1.10.96.0/31 |

</details>

<details>
  <summary>AS220 Провайдер2</summary>

| **Device** | **Interface**                       | **IPv4 Address**                                                              |
|------------|-------------------------------------|-------------------------------------------------------------------------------|
| **R12**    | Lo0<br>e0/0<br>e0/1<br>e0/2<br>e0/3 | 2.20.0.13/32<br>1.10.255.1/31<br>2.20.97.0/31<br>2.20.99.0/31<br>2.20.98.0/31 |

</details>
