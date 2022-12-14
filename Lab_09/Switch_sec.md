## Лабораторная работа - Конфигурация безопасности коммутатора 

**Топология**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/shema.png)

**Таблица адресации**

| Устройство	| interface/vlan	| IP-адрес	| Маска подсети |
| :---------: |:-----------:| :---------------:| :---------------: |
| R1	| G0/0/1	| 192.168.10.1	| 255.255.255.0 |
| | Loopback 0 	| 10.10.1.1	| 255.255.255.0 |
| S1	| VLAN 10	| 192.168.10.201	| 255.255.255.0 |
| S2	| VLAN 10	| 192.168.10.202	| 255.255.255.0 |
| PC A	| NIC	| DHCP	| 255.255.255.0 |
| PC B	| NIC	| DHCP	| 255.255.255.0 |


### Цели

##### Часть 1. Настройка основного сетевого устройства

##### Часть 2. Настройка сетей VLAN

##### Часть 3: Настройки безопасности коммутатора.

---

### Часть 1. Настройка основного сетевого устройства

##### Шаг 1. Создайте сеть

##### Шаг 2. Настройте маршрутизатор R1.

a.	Загрузите следующий конфигурационный скрипт на R1.

<details><summary> Лог загрузки </summary>
<pre>

Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#no ip domain lookup
R1(config)#ip dhcp excluded-address 192.168.10.1 192.168.10.9
R1(config)#ip dhcp excluded-address 192.168.10.201 192.168.10.202
R1(config)#!
R1(config)#ip dhcp pool Students
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# domain-name CCNA2.Lab-11.6.1
R1(dhcp-config)#!
R1(dhcp-config)#interface Loopback0

R1(config-if)# ip address 10.10.1.1 255.255.255.0
R1(config-if)#!
R1(config-if)#interface GigabitEthernet0/0/1
R1(config-if)# description Link to S1
R1(config-if)# ip dhcp relay information trusted
                  ^
% Invalid input detected at '^' marker.
	
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown

R1(config-if)#!
R1(config-if)#line con 0
R1(config-line)# logging synchronous
R1(config-line)# exec-timeout 0 0
R1(config-line)#
%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-line)#

</pre>
</details>

b.	Проверьте текущую конфигурацию на R1, используя следующую команду: ***R1# show ip interface brief***

<details>
  <summary>Screenshot</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/1-2-d.png">
</details>

 

c.	Убедитесь, что IP-адресация и интерфейсы находятся в состоянии up / up (при необходимости устраните неполадки).

<details>
  <summary>Screenshot</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/1-2-c.png">
</details>



##### Шаг 3. Настройка и проверка основных параметров коммутатора

<details><summary> ЗАДАНИЕ </summary>
<pre>
a.	Настройте имя хоста для коммутаторов S1 и S2.
b.	Запретите нежелательный поиск в DNS.
c.	Настройте описания интерфейса для портов, которые используются в S1 и S2.
d.	Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.
</pre>
</details>



<details><summary> Код настройки S1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S1
int f0/6
description user PC-A
exit
int f0/1
description connection for S2 (f0/1)
exit
int f0/5
description connection for R1 (g0/0/1)
exit
int vlan1
</pre>
</details>

<details><summary> Код настройки S1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S2
int f0/18
description user PC-B
exit
int f0/1
description connection for S1 (f0/1)
exit
</pre>
</details>

---

### Часть 2. Настройка сетей VLAN на коммутаторах.

##### Шаг 1. Сконфигруриуйте VLAN 10.

Добавьте VLAN 10 на S1 и S2 и назовите VLAN - Management.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/2-1.png)

##### Шаг 2. Сконфигруриуйте SVI для VLAN 10.

Настройте IP-адрес в соответствии с таблицей адресации для SVI для VLAN 10 на S1 и S2. Включите интерфейсы SVI и предоставьте описание для интерфейса.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/2-2.png)


##### Шаг 3. Настройте VLAN 333 с именем Native на S1 и S2.

##### Шаг 4. Настройте VLAN 999 с именем ParkingLot на S1 и S2.	

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/2-34.png.png)

---

### Часть 3. Настройки безопасности коммутатора.

##### Шаг 1. Релизация магистральных соединений 802.1Q.

a.	Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN.

<details><summary> Лог настройки  </summary>
<pre>
S1(config)#int f0/1
S1(config-if)#Switchport mode trunk

S1(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up

S1(config-if)#Switchport trunk native vlan 333
S1(config-if)#%SPANTREE-2-RECV_PVID_ERR: Received BPDU with inconsistent peer vlan id 1 on FastEthernet0/1 VLAN333.

%SPANTREE-2-BLOCK_PVID_LOCAL: Blocking FastEthernet0/1 on VLAN0333. Inconsistent local vlan.


%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (333), with S2 FastEthernet0/1 (1).

S1(config-if)#exit
S1(config)#
</pre>
</details>



b.	Убедитесь, что режим транкинга успешно настроен на всех коммутаторах. ***команда: show interface trunk***

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-1-b1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-1-b2.png)

c.	Отключить согласование DTP F0/1 на S1 и S2. 

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-1-c1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-1-c2.png)


d.	Проверьте с помощью команды **show interfaces f0/1 switchport | include Negotiation**.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-1-d1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-1-d2.png)


##### Шаг 2. Настройка портов доступа

a.	На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-2-a1.png)

b.	На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-2-b.png)

##### Шаг 3. Безопасность неиспользуемых портов коммутатора

a.	На S1 и S2 переместите неиспользуемые порты из VLAN 1 в VLAN 999 и отключите неиспользуемые порты.

<details><summary> Лог настройки  S1 </summary>
<pre>

S1(config)#int range f0/1-4, f0/7-24, g0/1-2
S1(config-if-range)#Switchport mode access
S1(config-if-range)#Switchport access vlan 999
S1(config-if-range)#shutdown
S1(config-if-range)#exit

</pre>
</details>


b.	Убедитесь, что неиспользуемые порты отключены и связаны с VLAN 999, введя команду  **show.
S1# show interfaces status**

<details>
  <summary>S1</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-3-b1.png">
</details>


<details>
  <summary>S2</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-3-b2.png">
</details>


##### Шаг 4. Документирование и реализация функций безопасности порта.

a.	На S1, введите команду show port-security interface f0/6  для отображения настроек по умолчанию безопасности порта для интерфейса F0/6. Запишите свои ответы ниже.

**Конфигурация безопасности порта по умолчанию**

| Функция	| Настройка по умолчанию |
| :---------: |:-----------:| 
| Защита портов	 | Disabled |
| Максимальное количество записей MAC-адресов	 | 0 |
| Режим проверки на нарушение безопасности | Disabled |
| Aging Time | 0 mins |
| Aging Type | Absolute |
| Secure Static Address Aging | Disabled |
| Sticky MAC Address | 0 |

b.	На S1 включите защиту порта на F0 / 6 со следующими настройками:

<details><summary> настройки защиты порта </summary>
<pre>
o	Максимальное количество записей MAC-адресов: 3
o	Режим безопасности: restrict
o	Aging time: 60 мин.
o	Aging type: неактивный
</pre>
</details>

<details><summary> Лог настройки  порта  F0/6 </summary>
<pre>

S1(config)#int f0/6
S1(config-if)#switch port-se
S1(config-if)#switch port-security 
S1(config-if)#switch port-security maximum 3
S1(config-if)#switchport port-security violation restrict 
S1(config-if)#switchport port-security aging time 60
S1(config-if)#switchport port-security aging type ?
% Unrecognized command

</pre>
</details>


не удалось настроить **switchport port-security aging type** , отсутсвует команда 

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-4-b1.png)


c.	Verify port security on S1 F0/6.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-4-b2.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-4-c.png)

d.	Включите безопасность порта для F0 / 18 на S2. Настройте каждый активный порт доступа таким образом, чтобы он автоматически добавлял адреса МАС, изученные на этом порту, в текущую конфигурацию.




e.	Настройте следующие параметры безопасности порта на S2 F / 18:

<details><summary> настройки защиты порта </summary>
<pre>
o	Максимальное количество записей MAC-адресов: 2
o	Тип безопасности: Protect
o	Aging time: 60 мин.
</pre>
</details>

<details><summary> Лог настройки  порта  F0/18 </summary>
<pre>

S2>en
S2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#int f0/18
S2(config-if)#switch port-security
S2(config-if)#switch port-security maximum 2
S2(config-if)#switchport port-security violation p
S2(config-if)#switchport port-security violation protect 
S2(config-if)#switchport port-security aging time 60
S2(config-if)#

</pre>
</details>

f.	Проверка функции безопасности портов на S2 F0/18.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-4-f1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-4-f2.png)


##### Шаг 5. Реализовать безопасность DHCP snooping.

a.	На S2 включите DHCP snooping и настройте DHCP snooping во VLAN 10.

b.	Настройте магистральные порты на S2 как доверенные порты.

c.	Ограничьте ненадежный порт Fa0/18 на S2 пятью DHCP-пакетами в секунду.

<details>
  <summary>лог настройки шаг 5 а-с</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-5-ac.png">
</details>

     
d.	Проверка DHCP Snooping на S2.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-5-d.png)

e.	В командной строке на PC-B освободите, а затем обновите IP-адрес.

    ipconfig /release
    ipconfig /renew

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-5-e.png)

f.	Проверьте привязку отслеживания DHCP с помощью команды show ip dhcp snooping binding.

    show ip dhcp snooping binding

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-5-f.png)


##### Шаг 6. Реализация PortFast и BPDU Guard	

a.	Настройте PortFast на всех портах доступа, которые используются на обоих коммутаторах.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-6-a1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-6-a2.png)

b.	Включите защиту BPDU на портах доступа VLAN 10 S1 и S2, подключенных к PC-A и PC-B.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-6-b1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-6-b2.png)

c.	Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-6-c1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_09/pic/3-6-c2.png)

##### Шаг 7. Проверьте наличие сквозного ⁪подключения.

Проверьте PING связь между всеми устройствами в таблице IP-адресации. В случае сбоя проверки связи может потребоваться отключить брандмауэр на хостах.

| № | От    | Назначение | Результат |
| :--:| :----:| :---------:| :---------:|
|1| PC-A 192.168.10.10| R1 192.168.10.1   | Успех |
|2| PC-A 192.168.10.10| Loopback 0 10.10.1.1 | Успех |
|3| PC-A 192.168.10.10| S1 192.168.10.201 | Успех |
|4| PC-A 192.168.10.10| S2 192.168.10.202 | Успех |
|5| PC-A 192.168.10.11| PC-B 192.168.10.11   | Успех |
|5| PC-B 192.168.10.11| R1 192.168.10.1   | Успех |
|6| PC-B 192.168.10.11| Loopback 0 10.10.1.1 | Успех |
|7| PC-B 192.168.10.11| S1 192.168.10.201 | Успех |
|8| PC-B 192.168.10.11| S2 192.168.10.202 | Успех |
|9|   R1 | S1 192.168.10.201 | Успех |
|10|  R1 | S2 192.168.10.202 | Успех |
|11|  s1 | S2 192.168.10.202 | Успех |



***Вопросы для повторения***

> 1.	С точки зрения безопасности порта на S2, почему нет значения таймера для оставшегося возраста в минутах, когда было сконфигурировано динамическое обучение - sticky?
>
>     При динамическом обучении sticky mac addres подключенного устройства записывается в постоянную память (NVRAM)

> 2.	Что касается безопасности порта на S2, если вы загружаете скрипт текущей конфигурации на S2, почему порту 18 на PC-B никогда не получит IP-адрес через DHCP?
> 
>     На порту f0/5 включен ограничение по DHCP пакетам

> 3.	Что касается безопасности порта, в чем разница между типом абсолютного устаревания и типом устаревание по неактивности?
>
>     При абсолютном устаревании адреса порт удаляется по истечении указаного времени в настройках, 
>     т.е. есть ограничение по времени работы подключеного устройства к порту. 
>
>     При неактивности только в случае если отключеное состояние порта превысело время указаное при
>     настройке, т.е. нет ограничение работы подключеного устройства к порту по времени 

