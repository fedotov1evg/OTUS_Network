## Часть 1. Проверка сети



## Часть 2. Настройка коммутаторов


#### 2.1 базовая настройка коммутаторов и портов

<details><summary> Код настройки SW1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname SW1
banner motd ##SW1_ENTER_PASSWORD##

exit
copy running-config startup-config
</pre>
</details>


<details><summary> Код настройки SW2 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname SW2
banner motd ##SW2_ENTER_PASSWORD##

exit
copy running-config startup-config
</pre>
</details>


<details><summary> Код настройки SW3 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname SW3
banner motd ##SW3_ENTER_PASSWORD##

exit
copy running-config startup-config
</pre>
</details>


#### 2.2 Настройка LACP (SW 2 SW3)

<details><summary> ЗАДАНИЕ LACP </summary>
<pre>

-  Одинаковая скорость и дуплексный режим.
-  Все интерфейсы группы должны быть отнесены к одной и той же VLAN или настроены как 
магистраль.
-  Магистраль должна поддерживать один диапазон VLAN.

</pre>
</details>

Настройка VLAN 

  <details><summary> VLAN </summary>
  <pre>
   
vlan 10
name VLAN 10 for PC6
end

vlan 11
name VLAN 11 for LT
end


-----настройка SW2------

Interface fa0/23
switchport mode access
switchport access vlan 10
ex

Interface fa0/24
switchport mode access
switchport access vlan 11
ex


-----настройка SW1------

interface range f0/1, g0/1
switchport mode trunk
switchport trunk allowed vlan 1,10,11
end


-----настройка SW3------


interface range f0/1, g0/1
switchport mode trunk
switchport trunk allowed vlan 1,10,11
end


  </pre>
  </details>


Порты для протокола LACP 4 и 5 SW2 и SW3


  <details><summary> LACP SW2 </summary>
  <pre>
   
interface range fastEthernet 0/4-5
shutdown
switchport mode trunk
switchport trunk allowed vlan 1,10,11

channel-group 1 mode active
no shutdown
exit
  </pre>
  </details>


  <details><summary> LACP SW3 </summary>
  <pre>
 
   
interface range fastEthernet 0/4-5
shutdown
switchport mode trunk
switchport trunk allowed vlan 1,10,11

channel-group 1 mode active
no shutdown
exit
  </pre>
  </details>

Проверка LAPC
    show interfaces port-channel 1
    show etherchannel port-channel 



#### 2.3 Настройка PagP  (SW2 SW1)

  <details><summary> PagP  SW1 </summary>
  <pre>
  
interface range fastEthernet 0/6-7
shutdown
switchport mode trunk
switchport trunk allowed vlan 1,10,11

channel-group 2 mode desirable
no shutdown
exit
  </pre>
  </details>
  
  <details><summary> PagP  SW2 </summary>
  <pre>
  
interface range fastEthernet 0/6-7
switchport mode trunk
switchport trunk allowed vlan 1,10,11

channel-group 2 mode auto
exit
  </pre>
  </details>

#### 2.4 Настройка RSTP

  <details><summary> RSTP  SW1 </summary>
  <pre>
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
spanning-tree vlan 11 root secondary
  </pre>
  </details>
  
  
  <details><summary> RSTP  SW2 </summary>
  <pre>
spanning-tree mode rapid-pvst
  </pre>
  </details>
  
  
  <details><summary> RSTP  SW3 </summary>
  <pre>
spanning-tree mode rapid-pvst
spanning-tree vlan 11 root primary
spanning-tree vlan 10 root secondary
  </pre>
  </details>


## Часть 3. Настройка роутеров 


#### 2.1 базовая настройка роутеров и портов

<details><summary> Код настройки R1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R1
banner motd ##R1_ENTER_PASSWORD##

int g0/2
description R1 for R3
ip add 10.1.0.2 255.255.255.252
no shutdown
exit

int g0/0
description R1 for R2
ip add 10.3.0.2 255.255.255.252
no shutdown
exit

exit
copy running-config startup-config
</pre>
</details>


<details><summary> Код настройки R2 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R2
banner motd ##R2_ENTER_PASSWORD##

int g0/2
description R2 for R3
ip add 10.2.0.2 255.255.255.252
no shutdown
exit

int g0/0
description R2 for R1
ip add 10.3.0.2 255.255.255.252
no shutdown
exit


exit
copy running-config startup-config
</pre>
</details>


<details><summary> Код настройки R3 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R3
banner motd ##R3_ENTER_PASSWORD##
line console 0
logging synchronous
password cisco
login
exit
enable secret class
line vty 0 15
password cisco
login
exit
service password-encryption
exit

int g0/0
description R3 for R1
ip add 10.1.0.1 255.255.255.252
no shutdown
exit

int g0/1
description R3 for R2
ip add 10.2.0.1 255.255.255.252
no shutdown
exit

int g0/2
description R3 for WEB
ip add 10.10.10.1 255.255.255.0
no shutdown
exit

exit
copy running-config startup-config
</pre>
</details>





#### 2.2 Настройка HSRP (R1, R2)

Группа HSRP 1

  <details><summary>  HSRP R1 </summary>
  <pre>
  
interface g0/0
standby version 2

ip add 172.17.127.11 255.255.128.0 ?
ip add 192.168.10.11 255.255.255.128 ?

standby 1 ip 192.168.10.126
standby 1 priority 150
standby 1 preempt

standby 2 ip 172.17.127.254
no shutdown
  </pre>
  </details>
  
  <details><summary>  HSRP R2 </summary>
  <pre>
interface g0/2
standby version 2

ip add 172.17.127.11 255.255.128.0 ?
ip add 192.168.10.11 255.255.255.128 ?


standby 1 ip 192.168.10.126

standby 2 ip 172.17.127.254
standby 2 priority 150
standby 2 preempt
no shutdown

  </pre>
  </details>

**Команды проверки**

    

**Команды отладки**

    

#### 2.3 Настройка OSPF

  <details><summary>  OSPF R1 </summary>
  <pre>
   router ospf 20
   router-id 1.1.1.1
   network 10.1.0.2 0.0.0.0 area 0
   network 10.3.0.1 0.0.0.0 area 0
   
   passive-interface g0/1.10
   passive-interface g0/1.11
   ex
   
   interface G0/1.10
   ip ospf 20 area 0
   ex
   
   interface G0/1.11
   ip ospf 20 area 0
   ex
   
      
  </pre>
  </details>

  <details><summary>  OSPF R2 </summary>
  <pre>
     
   router ospf 20
   router-id 2.2.2.2
   network 10.2.0.2 0.0.0.0 area 0
   network 10.3.0.2 0.0.0.0 area 0
   
   passive-interface g0/1.10
   passive-interface g0/1.11
   ex
   
   interface G0/1.10
   ip ospf 20 area 0
   ex
   
   interface G0/1.11
   ip ospf 20 area 0
   ex
       
  </pre>
  </details>

  <details><summary>  OSPF R3 </summary>
  <pre>
  router ospf 20
  router-id 3.3.3.3
  network 10.1.0.1 0.0.0.0 area 0
  network 10.2.0.1 0.0.0.0 area 0
  network 10.10.10.1 0.0.0.0 area 0
  
  passive-interface g0/2
  ex
   
  </pre>
  </details>


#### 3.4 Настройка RoS  (SW1-R1 SW3-R2)

  <details><summary>  RoS  R1 </summary>
  <pre>

interface g0/1.10
Description Default for VLAN 10
encapsulation dot1Q 10
ip add 192.168.10.124 255.255.255.128
exit

interface g0/1.11
Description Default for VLAN 11
encapsulation dot1Q 11
ip add 172.17.127.252 255.255.128.0
exit

interface g0/1
Description Trunk link for SW1
no shut
ex

  </pre>
  </details>
  
  <details><summary>  RoS R3 </summary>
  <pre>
interface g0/1.10
Description Default for VLAN 10
encapsulation dot1Q 10
ip add 192.168.10.125 255.255.255.128
exit

interface g0/1.11
Description Default for VLAN 11
encapsulation dot1Q 11
ip add 172.17.127.253 255.255.128.0
exit

interface g0/1
Description Trunk link for SW3
no shut
ex
  </pre>
  </details>


## Часть 4. Проверка связи и конфигураций
