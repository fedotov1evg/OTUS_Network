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

Порты для протокола LACP 4 и 5 SW2 и SW3
VLAN для протокола 30

  <details><summary> LACP SW2 </summary>
  <pre>
  
vlan 30
name VLAN LACP
ex
   
interface range fastEthernet 0/4-5
switchport mode trunk
switchport trunk allowed vlan 1,30

channel-group 1 mode active 
exit
  </pre>
  </details>


  <details><summary> LACP SW3 </summary>
  <pre>
 
vlan 30
name VLAN LACP
ex
   
interface range fastEthernet 0/4-5
switchport mode trunk
switchport trunk allowed vlan 1,30

channel-group 1 mode active 
exit
  </pre>
  </details>

Проверка LAPC
    show interfaces port-channel 1
    show etherchannel port-channel 



#### 2.3 Настройка PagP  (SW2 SW1)

  <details><summary> PagP  SW1 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>
  
  <details><summary> PagP  SW2 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
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

int g0/2
description R1 for R3
ip add 10.1.0.2 255.255.255.252
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

int g0/2
description R2 for R3
ip add 10.2.0.2 255.255.255.252
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
ip add 172.17.127.252 255.255.128.0
standby version 2
standby 1 ip 172.17.127.254
standby 1 priority 150
standby 1 preempt
no shutdown
  </pre>
  </details>
  
  <details><summary>  HSRP R2 </summary>
  <pre>
interface g0/2
ip add 172.17.127.253 255.255.128.0
standby version 2
standby 1 ip 172.17.127.254
no shutdown

  </pre>
  </details>

**Команды проверки**

    

**Команды отладки**

    

#### 2.3 Настройка OSPF

  <details><summary>  OSPF R1 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>

  <details><summary>  OSPF R2 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>

  <details><summary>  OSPF R3 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>


#### 3.4 Настройка RoS  (SW1-R1 SW3-R2)

  <details><summary>  RoS  SW1 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>
  
  <details><summary>  RoS SW3 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>


## Часть 4. Проверка связи и конфигураций



