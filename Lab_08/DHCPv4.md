## Лабраторная работа - Настройка DHCPv4

### Топология

**Схема**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/Shema-1.png "Схема")

**Таблица адресации**

| Устройство	| Интерфейс	| IP-адрес	| Маска подсети	| Шлюз по умолчанию |
| :---------: | :-------: | :-------: | :-----------: | :---------------: |
| R1	| G0/0/0	| 10.0.0.1	| 255.255.255.252	| — |
| R1	| G0/0/1	| —	| —	| — |
| R1	| G0/0/1.100  | 192.168.1.1| 255.255.255.192 | — |
| R1	| G0/0/1.200	| 192.168.1.65 | 255.255.255.224 |	— |
| R1	| G0/0/1.1000 | —	| —	| — |
| R2	| G0/0	| 10.0.0.2	| 255.255.255.252	| — |
| R2	| G0/0/1	| 192.168.1.97 | 255.255.255.240 | — |
| S1	| VLAN 200	| 192.168.1.66 | 192.168.1.224 | |
| S2	| VLAN 1	| | | |
| PC-A	| NIC	| DHCP	| DHCP	| DHCP |
| PC-B	| NIC	| DHCP	| DHCP	| DHCP |


**Таблица VLAN**
| VLAN  |  Имя | Назначенный интерфей  |
| :---: | :------: | :------------: |
| 1	| Нет	| S2: F0/18 |
| 100	| Клиенты	| S1: F0/6  |
| 200	| Управление	| S1: VLAN 200 |  
| 999	| Parking_Lot	| S1: F0/1-4, F0/7-24, G0/1-2 |
| 1000	| Собственная	| — |



## Задачи

#### Часть 1. Создание сети и настройка основных параметров устройства
#### Часть 2. Настройка и проверка двух серверов DHCPv4 на R1
#### Часть 3. Настройка и проверка DHCP-ретрансляции на R2



------------

### Часть 1. Создание сети и настройка основных параметров устройства

#### Шаг 1. Создание схемы адресации

a.	Одна подсеть «Подсеть A», поддерживающая 58 хостов (клиентская VLAN на R1).
Подсеть A
Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.100 . 

b.	Одна подсеть «Подсеть B», поддерживающая 28 хостов (управляющая VLAN на R1). 
Подсеть B:
Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.200. Запишите второй IP-адрес в таблице адресов для S1 VLAN 200 и введите соответствующий шлюз по умолчанию.

c.	Одна подсеть «Подсеть C», поддерживающая 12 узлов (клиентская сеть на R2).
Подсеть C:
Запишите первый IP-адрес в таблице адресации для R2 G0/0/1.

***Расчет IP адресов***

<details><summary>Расчет IP адресов</summary>
<pre>

| |  |||
| :-------------: | :--------------------: | :--------------------: | :--------------------: |
| IP-адрес узла:| 192.168.1.0 |||
| Исходная маска подсети:  | 255.255.255.0  |||
| Подсеть  | А  | В | С |
| Новая маска подсети  | 255.255.255.192  | 255.255.255.224 | 255.255.255.240 |
| Решение  |
| Количество бит подсети| 26 | 27 | 28 |
| Количество узлов в подсети  | 62 | 30  | 14  |
| Сетевой адрес этой подсети                     | 192.168.1.0 | 192.168.1.64 | 192.168.1.96 |
| IPv4-адрес первого узла в этой подсети         |192.168.1.1  | 192.168.1.65 | 192.168.1.97 |
| IPv4-адрес последнего узла в этой подсети  |    192.168.1.62 | 192.168.1.94 | 192.168.1.110 |
| Широковещательный IPv4-адрес в этой подсети  | 192.168.1.63  | 192.168.1.95 | 192.168.1.111 |

</pre>
</details>




#### Шаг 2.	Создайте сеть согласно топологии

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/Shema.png "Схема")

#### Шаг 3. Произведите базовую настройку маршрутизаторов.

a.	Назначьте маршрутизатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Активация IPv6-маршрутизации

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

<details><summary>Пример комманд настройки R1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R1
banner motd ######################## R1 ENTER PASSWORD ########################
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
clock set 21:00:00 24 nov 2022
conf term
copy running-config startup-config

</pre>
</details>

***Установлено след дата и время :  21:00:00 24 nov 2022***

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-3-r1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-3-r2.png)

#### Шаг 4. Настройка маршрутизации между сетями VLAN на маршрутизаторе R1

a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-4-a.png)

b.	Настройте подинтерфейсы для каждой VLAN в соответствии с требованиями таблицы IP-адресации. Все субинтерфейсы используют инкапсуляцию 802.1Q и назначаются первый полезный адрес из вычисленного пула IP-адресов. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-4-b.png)

<details><summary> Код </summary>
<pre>

R1(config)#interface g0/0/1.100
R1(config-subif)#description VLAN 100 - Clients
R1(config-subif)#encapsulation dot1q 100
R1(config-subif)#ip add 192.168.1.1 255.255.255.192
R1(config-subif)#description VLAN 100 - Clients
R1(config-subif)#exit
R1(config)#interface g0/0/1.200
R1(config-subif)#description VLAN 200 - management
R1(config-subif)#encapsulation dot1q 200
R1(config-subif)#ip add 192.168.1.65 255.255.255.224
R1(config-subif)#exit
R1(config)#interface g0/0/1.1000
R1(config-subif)# exit


</pre>
</details>

c.	Убедитесь, что вспомогательные интерфейсы работают.

#### Шаг 5.	Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов

a.	Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.


![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-5-a.png)


b.	Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-5-b1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-5-b2.png)

c.	Настройте маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом маршрутизаторе.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-5-c1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-5-c2.png)

d.	Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-5-d1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-5-d2.png)

e.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.


#### Шаг 6.	Настройте базовые параметры каждого коммутатора.
a.	Присвойте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

i.	Установите часы на маршрутизаторе на сегодняшнее время и дату.

j.	Скопируйте текущую конфигурацию в файл загрузочной конфигурации.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-6-1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-6-2.png)


#### Шаг 7.	Создайте сети VLAN на коммутаторе S1.

Примечание. S2 настроен только с базовыми настройками. 

a.	Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-a.png)

b.	Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-b.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-b1.png)

c.	Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-c.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-c1.png)

d.	Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.


![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-d1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-d2.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-7-d3.png)


#### Шаг 8.	Назначьте сети VLAN соответствующим интерфейсам коммутатора.

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.
Откройте окно конфигурации

b.	Убедитесь, что VLAN назначены на правильные интерфейсы.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-8-b1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/1-8-b2.png)


> Вопрос:Почему интерфейс F0/5 указан в VLAN 1?
>> При настройке назначения портов на VLAN данный порт не был указан в командах, и он остался в 1 Vlan 


#### Шаг 9.	Вручную настройте интерфейс S1 F0/5 в качестве транка 802.1Q.

a.	Измените режим порта коммутатора, чтобы принудительно создать магистральный канал.

b.	В рамках конфигурации транка  установите для native  VLAN значение 1000.

c.	В качестве другой части конфигурации магистрали укажите, что VLAN 100, 200 и 1000 могут проходить по транку.

d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

<details><summary> **Код** </summary>
<pre>
 
configure terminal
interface f0/5
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 100,200,1000
end
copy running-config startup-config
 
</pre>
</details>


e.	Проверьте состояние транка. **Команда sh int fa0/5 switchport**

<details><summary> Лог </summary>
<pre>
 
S1#sh int fa0/5 switchport 
Name: Fa0/5
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1000 (Inactive)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 100,200,1000
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
 
</pre>
</details>

> Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?
>> IP ПК зависел бы от пула адресов что настроен на сервере
---

### Часть 2.	Настройка и проверка двух серверов DHCPv4 на R1

В части 2 необходимо настроить и проверить сервер DHCPv4 на R1. Сервер DHCPv4 будет обслуживать две подсети, подсеть A и подсеть C.

#### Шаг 1.	Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. Ниже приведен только пул DHCP для подсети A

a.	Исключите первые пять используемых адресов из каждого пула адресов.

b.	Создайте пул DHCP (используйте уникальное имя для каждого пула).
**R1-POOL-A  R2_Client_LAN***


c.	Укажите сеть, поддерживающую этот DHCP-сервер.

d.	В качестве имени домена укажите CCNA-lab.com.

e.	Настройте соответствующий шлюз по умолчанию для каждого пула DHCP.

f.	Настройте время аренды на 2 дня 12 часов и 30 минут.

<details><summary> Код для сети А </summary>
<pre>
   
ip dhcp excluded-address 192.168.1.1
ip dhcp excluded-address 192.168.1.65
ip dhcp excluded-address 192.168.1.97
ip dhcp excluded-address 192.168.1.66
ip dhcp excluded-address 192.168.1.98
ip dhcp excluded-address 192.168.1.254
ip dhcp pool R1-POOL-A
network 192.168.1.0 255.255.255.192
default-router 10.0.0.1
dns-server 10.0.0.1
domain-name CCNA-lab.com
lease 2 12 30 infinite
end

   
</pre>
</details>

<details><summary> Код для сети C </summary>
<pre>
   
ip dhcp excluded-address 192.168.1.1
ip dhcp excluded-address 192.168.1.65
ip dhcp excluded-address 192.168.1.97
ip dhcp excluded-address 192.168.1.66
ip dhcp excluded-address 192.168.1.98
ip dhcp excluded-address 192.168.1.254
ip dhcp pool R2_Client_LAN
network 192.168.1.96 255.255.255.240
default-router 10.0.0.1
dns-server 10.0.0.1
domain-name CCNA-lab.com
lease 2 12 30 infinite
end

   
</pre>
</details>

**Команды lease 2 12 30 infinite нет в Packet Tracer**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-1-f-er.png)


g.	Затем настройте второй пул DHCPv4, используя имя пула R2_Client_LAN и вычислите сеть, маршрутизатор по умолчанию, и используйте то же имя домена и время аренды, что и предыдущий пул DHCP.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-1-g.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-1-g2.png)

#### Шаг 2.	Сохраните конфигурацию.

Сохраните текущую конфигурацию в файл загрузочной конфигурации.
Закройте окно настройки.

#### Шаг 3.	Проверка конфигурации сервера DHCPv4

a.	Чтобы просмотреть сведения о пуле, выполните команду show ip dhcp pool .

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-3-a.png)

<details><summary> лог </summary>
<pre>

Pool R1-POOL-A :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 62
 Leased addresses               : 0
 Excluded addresses             : 6
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.1.1          192.168.1.1      - 192.168.1.62      0    / 6     / 62

Pool R2_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 14
 Leased addresses               : 0
 Excluded addresses             : 6
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.1.97         192.168.1.97     - 192.168.1.110     0    / 6     / 14

</pre>
</details>
 

b.	Выполните команду show ip dhcp bindings для проверки установленных назначений адресов DHCP.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-3-b.png)


c.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.



**Команды "ip dhcp server statistics" нет в Packet Tracer**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-3-c-er.png)




#### Шаг 4.	Попытка получить IP-адрес от DHCP на PC-A

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-4.png)

a.	Из командной строки компьютера PC-A выполните команду ipconfig /all.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-4-a.png)

b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-4-b.png)

c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R0 G0/0/1.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-4-c.png)

### Часть 3.	Настройка и проверка DHCP-ретрансляции на R2

#### Шаг 1.	Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1

a.	Настройте команду ip helper-address на G0/0/1, указав IP-адрес G0/0/0 R1.
Откройте окно конфигурации

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/3-1.png)

b.	Сохраните конфигурацию.

#### Шаг 2.	Попытка получить IP-адрес от DHCP на PC-B

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/3-2.png)

a.	Из командной строки компьютера PC-B выполните команду ipconfig /all.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/3-2-1.png)

b.	После завершения процесса обновления выполните команду ipconfig для просмотра новой информации об IP-адресе.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/3-2-b.png)

c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R1 G0/0/1.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/3-2-c.png)

d.	Выполните show ip dhcp binding для R1 для проверки назначений адресов в DHCP.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/3-2-d.png)

e.	Выполните команду show ip dhcp server statistics для проверки сообщений DHCP.

**Команды "ip dhcp server statistics" нет в Packet Tracer**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic/2-3-c-er.png)


