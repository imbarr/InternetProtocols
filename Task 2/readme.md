Протокол RIP, практика.
=======================

Бараковских Илья, КН-202, n = 2

 

План работы.
------------

-   Построим сеть. Для компьютеров и серверов выставим IP-адреса,

маски подсетей и шлюзы по умолчанию.

 

-   Аналогично настроим IP-адреса и маски подсетей на портах маршрутизаторов.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Пример:

Router>enable
Router#configure terminal
Router(config)#interface FastEthernet0/0
Router(config-if)#no shutdown
Router(config-if)#ip address 192.168.10.1 255.255.255.252
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

-   Запустим протокол RIP на всех маршрутизаторах.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Пример:

Router(config)#router rip
Router(config-router)#network 192.168.1.0
Router(config-router)#network 10.2.1.0
Router(config-router)#network 192.168.10.0
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

-   Командой "write memory" сохраним настройки маршрутизаторов.

 

-   Убедимся в работоспособности сети.

 

-   (Дополнительно) Уберем один из проводов, соединяющих маршрутизаторы, и
    убедимся, что сеть адаптируется к изменениям.

 

Результаты.
-----------

Router1:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is subnetted, 1 subnets
C       10.2.1.0 is directly connected, FastEthernet0/1
C    192.168.1.0/24 is directly connected, FastEthernet1/0
R    192.168.2.0/24 [120/1] via 192.168.10.2, 00:00:18, FastEthernet0/0
R    192.168.3.0/24 [120/1] via 10.2.1.2, 00:00:22, FastEthernet0/1
     192.168.10.0/30 is subnetted, 2 subnets
C       192.168.10.0 is directly connected, FastEthernet0/0
R       192.168.10.4 [120/1] via 192.168.10.2, 00:00:18, FastEthernet0/0

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.1    YES manual up                    up 
FastEthernet0/1        10.2.1.1        YES manual up                    up 
FastEthernet1/0        192.168.1.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

Router2:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

R    10.0.0.0/8 [120/1] via 192.168.10.1, 00:00:26, FastEthernet0/0
                [120/1] via 192.168.10.5, 00:00:22, FastEthernet0/1
R    192.168.1.0/24 [120/1] via 192.168.10.1, 00:00:26, FastEthernet0/0
C    192.168.2.0/24 is directly connected, Ethernet1/0
R    192.168.3.0/24 [120/1] via 192.168.10.5, 00:00:22, FastEthernet0/1
     192.168.10.0/30 is subnetted, 2 subnets
C       192.168.10.0 is directly connected, FastEthernet0/0
C       192.168.10.4 is directly connected, FastEthernet0/1

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.2    YES manual up                    up 
FastEthernet0/1        192.168.10.6    YES manual up                    up 
Ethernet1/0            192.168.2.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

Router3:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is subnetted, 1 subnets
C       10.2.1.0 is directly connected, FastEthernet0/0
R    192.168.1.0/24 [120/1] via 10.2.1.1, 00:00:26, FastEthernet0/0
R    192.168.2.0/24 [120/1] via 192.168.10.6, 00:00:18, FastEthernet0/1
C    192.168.3.0/24 is directly connected, Ethernet1/0
     192.168.10.0/30 is subnetted, 2 subnets
R       192.168.10.0 [120/1] via 192.168.10.6, 00:00:18, FastEthernet0/1
C       192.168.10.4 is directly connected, FastEthernet0/1

Router#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        10.2.1.2        YES manual up                    up 
FastEthernet0/1        192.168.10.5    YES manual up                    up 
Ethernet1/0            192.168.3.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
