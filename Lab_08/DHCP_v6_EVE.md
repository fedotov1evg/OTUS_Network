## Лабраторная работа - Настройка DHCPv6 (Выполнена в EVE-NG)

### Топология

**Схема**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6-eve/shema.png)


**Таблица адресации**

| Устройство	| Интерфейс	| IPv6-адрес |
| :---------: | :-------: | :-------: |
| R1	| G0/0/0	| 2001:db8:acad:2::1/64 |
| 	| 	| fe80::1 |
| 	| G0/0/1	| 2001:db8:acad:1::1/64 |
| 	| 	| fe80::1 |
| R2| G0/0/0	| 2001:db8:acad:2::2/64 |
|   | 	| fe80::2 |
| 	| G0/0/1	| 2001:db8:acad:3::1/64 |
| 	| 	| fe80::1 |
| PC-A	| NIC	| DHCP |
| PC-B	| NIC	| DHCP |


## Задачи

**1. Создание сети и настройка основных параметров устройства**

**2. Проверка назначения адреса SLAAC от R1**

**3. Настройка и проверка сервера DHCPv6 без гражданства на R1**

**4. Настройка и проверка состояния DHCPv6 сервера на R1**

**5. Настройка и проверка DHCPv6 Relay на R2**


## Часть 1.Настрйока основных параметров устройств


<details><summary> Код SW1 </summary>
<pre>

enable
conf term
no ip domain-lookup
hostname S1
banner motd #####_S1_ENTER_PASSWORD_#####
line console 0
logging synchronous
password cisco
login
exit
service password-encryption
enable secret class
line vty 0 15
password cisco
login
exit

do copy running-config startup-config

</pre>
</details>



<details><summary> Код SW2 </summary>
<pre>

enable
conf term
no ip domain-lookup
hostname S2
banner motd ######_S2_ENTER_PASSWORD_######
line console 0
logging synchronous
password cisco
login
exit
service password-encryption
enable secret class
line vty 0 15
password cisco
login
exit

do copy running-config startup-config

</pre>
</details>


<details><summary> Код настройки R1 </summary>
<pre>
****Базовая настройка****
enable
conf term
no ip domain-lookup
hostname R1
banner motd ######_R1_ENTER_PASSWORD_######
line console 0
logging synchronous
password cisco
login
exit
service password-encryption
enable secret class
line vty 0 15
password cisco
login
exit

****Настройка портов****
ipv6 unicast-routing

int g
ipv6 add 2001:db8:acad:1::1/64
ipv6 add fe80::1 link-local
no shutdown
ex

int g
ipv6 add 2001:db8:acad:2::1/64
ipv6 add fe80::1 link-local
no shutdown
ex

****Настройка маршрутизации****
ipv6 route 2001:db8:acad:3::1/64 2001:db8:acad:2::2

****Настройка пула DHCP IPv6 для PC-A****
ipv6 dhcp pool R1-STATELESS
dns-server 2001:db8:acad::254
domain-name STATELESS.com

interface g0/0/1
ipv6 nd other-config-flag
ipv6 dhcp server R1-STATELESS

****Настройка пула DHCP IPv6 для R2(PC-B)****
ipv6 dhcp pool R2-STATEFUL
address prefix 2001:db8:acad:3:aaa::/80
dns-server 2001:db8:acad::254
domain-name STATEFUL.com

interface g0/0/0
ipv6 dhcp server R2-STATEFUL
    
</pre>
</details>

<details><summary> Код настройки R2 </summary>
<pre>
****Базовая настройка****
enable
conf term
no ip domain-lookup
hostname R2
banner motd ######_R2_ENTER_PASSWORD_######
line console 0
logging synchronous
password cisco
login
exit
service password-encryption
enable secret class
line vty 0 15
password cisco
login
exit

****Настройка портов****
ipv6 unicast-routing

int 
ipv6 add 2001:db8:acad:3::1/64
ipv6 add fe80::1 link-local
no shutdown
ex
int 
ipv6 add 2001:db8:acad:2::2/64
ipv6 add fe80::2 link-local
no shutdown
ex

****Настройка маршрутизации****
ipv6 route 2001:db8:acad:1::1/64 2001:db8:acad:2::1

****Настройка ретронсляции****
interface g0/0/1
ipv6 nd managed-config-flag
ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0

</pre>
</details>

#### Попытка получить адрес IPv6 из DHCPv6 на PC-A.

<details>
  <summary>Screenshot</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6-eve/1.png">
</details>


#### Попытка получить адрес IPv6 из DHCPv6 на PC-B.

<details>
  <summary>Screenshot</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6-eve/2.png">
</details>

Информация о том что компьютер РС-В теперь получает шз адрес от DHCP сервера. Указаны его имя домена и адрес DNS (2001:db8:acad::254) DHCP сервера (STATEFUL.com)

#### Проверка подключений с помощью пинга


<details>
  <summary>ping PC-B до </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6-eve/p-1.png">
</details>

<details>
  <summary>ping PC-B до </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6-eve/p-2.png">
</details>

<details>
  <summary>ping PC-B до </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6-eve/p-3.png">
</details>

<details>
  <summary>ping PC-B до </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6-eve/p-4.png">
</details>
