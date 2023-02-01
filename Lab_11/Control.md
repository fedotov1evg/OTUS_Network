## Лабораторная работа. Настройка и проверка расширенных списков контроля доступа.

**Топология**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/Shema.png)

**Таблица адресации**

| Устройство	| Интерфейс	| IP-адрес	| Маска подсети 	| Шлюз по умолчанию |
| :---------: |:-----------:| :---------------:| :---------------:|:---------------:|
| R1	| G0/0/1	| —	| —	| — |
| 	| G0/0/1.20	| 10.20.0.1	| 255.255.255.0	| — |
| 	| G0/0/1.30	| 10.30.0.1	| 255.255.255.0	| — |
| 	| G0/0/1.40	| 10.40.0.1	| 255.255.255.0	| — |
| 	| G0/0/1.1000	| —	| —	| — |
|   | Loopback1	| 172.16.1.1 | 255.255.255.0	| — |
| R2	| G0/0/1	| 10.20.0.4	| 255.255.255.0	| — |
| S1	| VLAN 20	| 10.20.0.2	| 255.255.255.0	| 10.20.0.1 |
| S2	| VLAN 20	| 10.20.0.3	| 255.255.255.0	| 10.20.0.1 |
| PC-A	| NIC	| 10.30.0.10	| 255.255.255.0	| 10.30.0.1 |
| PC-B	| NIC	| 10.40.0.10	| 255.255.255.0	| 10.40.0.1 |


**Таблица VLAN**

| VLAN	| Имя	| Назначенный интерфейс |
| :------: |:--------:| :---------------:|
| 20	| Management	| S2: F0/5 |
| 30	| Operations	| S1: F0/6 |
| 40	| Sales	| S2: F0/18 |
| 999	| ParkingLot	| S1: F0/2-4, F0/7-24, G0/1-2 |
| | | S2: F0/2-4, F0/6-17, F0/19-24, G0/1-2 |
| 1000	| Собственная | 	— |


### Задачи

###### Часть 1. Создание сети и настройка основных параметров устройства

###### Часть 2. Настройка сетей VLAN на коммутаторах.

###### Часть 3. ·Настройте транки (магистральные каналы).

###### Часть 4. Настройте маршрутизацию.

###### Часть 5. Настройте удаленный доступ

###### Часть 6. Проверка подключения

###### Часть 7. Настройка и проверка списков контроля доступа (ACL)

---

### Часть 1. Создание сети и настройка основных параметров устройства

##### Шаг 1. Создайте сеть согласно топологии.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/1-1.png)

##### Шаг 2. Произведите базовую настройку маршрутизаторов.



<details><summary>ЗАДАНИЕ 1-2 </summary>
  <pre>
a.	Назначьте маршрутизатору имя устройства.
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
f.	Зашифруйте открытые пароли.
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
  </pre>
  </details>
  
  


<details><summary>Настройка R1</summary>
  <pre>
  
enable
conf term
no ip domain-lookup
hostname R1
banner motd ##R1 ENTER PASSWORD##
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
copy running-config startup-config
  </pre>
  </details>
  
  
  <details><summary>Настройка R2</summary>
  <pre>

enable
conf term
no ip domain-lookup
hostname R2
banner motd ##R2 ENTER PASSWORD##
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
copy running-config startup-config
  </pre>
  </details>


##### Шаг 3. Настройте базовые параметры каждого коммутатора.

<details><summary>ЗАДАНИЕ 1-3</summary>
  <pre>
a.	Присвойте коммутатору имя устройства.
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
f.	Зашифруйте открытые пароли.
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
  </pre>
</details>


  <details><summary>Настройка S1</summary>
  <pre>
enable
conf term
no ip domain-lookup
hostname S1
banner motd ##S1 ENTER PASSWORD##
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
copy running-config startup-config
  </pre>
  </details>



  <details><summary>Настройка S2</summary>
  <pre>
enable
conf term
no ip domain-lookup
hostname S2
banner motd ##S2 ENTER PASSWORD##
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
copy running-config startup-config
  </pre>
  </details>



---

### Часть 2. Настройка сетей VLAN на коммутаторах.

##### Шаг 1. Создайте сети VLAN на коммутаторах

a.	Создайте необходимые VLAN и назовите их на каждом коммутаторе из приведенной выше таблицы.

b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 

c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking Lot, настройте их для статического режима доступа и административно деактивируйте их.

  <details><summary>Настройка S1</summary>
  <pre>
conf term
vlan 20
name Management
ex
vlan 30
name Operations
ex
vlan 40
name Sales
ex
vlan 999
name  Parking_Lot
ex

interface vlan 20
ip add 10.20.0.2 255.255.255.0

interface range fastEthernet 0/2-4, fastEthernet 0/7-24, gigabitEthernet 0/1-2
switchport mode access
switchport access vlan 999
exit
interface vlan 999
shutdown
exit
vlan 1000
name S
ex
  </pre>
  </details>



  <details><summary>Настройка S2</summary>
  <pre>
conf term
vlan 20
name Management
ex
vlan 30
name Operations
ex
vlan 40
name Sales
ex
vlan 999
name  Parking_Lot
ex
vlan 1000
name S
ex

interface vlan 20
ip add 10.20.0.3 255.255.255.0
ex

interface range fastEthernet 0/2-4, fastEthernet 0/6-17, fastEthernet 0/19-24, gigabitEthernet 0/1-2
switchport mode access
switchport access vlan 999
ex
interface vlan 999
shutdown
ex
  </pre>
  </details>
  

##### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.

  <details><summary>Настройка S1</summary>
  <pre>
interface fastEthernet 0/6
switchport mode access
switchport access vlan 30
ex
  </pre>
  </details>


  <details><summary>Настройка S2</summary>
  <pre>
interface fastEthernet 0/5
switchport mode access
switchport access vlan 20
ex
interface fastEthernet 0/18
switchport mode access
switchport access vlan 40
ex
  </pre>
  </details>

b.	Выполните команду show vlan brief, чтобы убедиться, что сети VLAN назначены правильным интерфейсам.

<details>
  <summary>S1 "show vlan brief"</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/2-2-b1.png">
</details>

<details>
  <summary>S2 "show vlan brief"</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/2-2-b2.png">
</details>

---

### Часть 3. ·Настройте транки (магистральные каналы).

##### Шаг 1. Вручную настройте магистральный интерфейс F0/1.

a.	Измените режим порта коммутатора на интерфейсе F0/1, чтобы принудительно создать магистральную связь. Не забудьте сделать это на обоих коммутаторах.

b.	В рамках конфигурации транка установите для native vlan значение 1000 на обоих коммутаторах. При настройке двух интерфейсов для разных собственных VLAN сообщения об ошибках могут отображаться временно.

c.	В качестве другой части конфигурации транка укажите, что VLAN 10, 20, 30 и 1000 разрешены в транке.

  <details><summary>Настройка S1</summary>
  <pre>
interface fastEthernet 0/1
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
exit
  </pre>
  </details>

  <details><summary>Настройка S2</summary>
  <pre>
interface fastEthernet 0/1
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
exit
  </pre>
  </details>

d.	Выполните команду show interfaces trunk для проверки портов магистрали, собственной VLAN и разрешенных VLAN через магистраль.

<details>
  <summary>S1 "show interfaces trunk" </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/3-1-d.png">
</details>



##### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.
b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
c.	Используйте команду show interfaces trunk для проверки настроек транка.

  <details><summary>Настройка S1 F0/5</summary>
  <pre>
interface fastEthernet 0/5
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
exit
copy running-config startup-config
  </pre>
  </details>


<details>
  <summary>S1  F0/5 "show interfaces trunk"</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/3-2-c1.png">
</details>

---

### Часть 4. Настройте маршрутизацию.

##### Шаг 1. Настройка маршрутизации между сетями VLAN на R1.

a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.

b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейс для собственной VLAN не имеет назначенного IP-адреса. Включите описание для каждого подинтерфейса.

  <details><summary>Настройка S1</summary>
  <pre>
interface gigabitEthernet 0/0/1
no shutdown

interface g0/0/1.20
description Management
encapsulation dot1Q 20
ip add 10.20.0.1 255.255.255.0
exit

interface g0/0/1.30
description Operations
encapsulation dot1Q 30
ip add 10.30.0.1 255.255.255.0
exit

interface g0/0/1.40
description Sales
encapsulation dot1Q 40
ip add 10.40.0.1 255.255.255.0
exit

interface g0/0/1.1000
description S
encapsulation dot1Q 1000
exit

  </pre>
  </details>

c.	Настройте интерфейс Loopback 1 на R1 с адресацией из приведенной выше таблицы.

  <details><summary>Настройка Loopback 1</summary>
  <pre>
interface  Loopback1
ip add 172.16.1.1 255.255.255.0
no shutdown
exit
  </pre>
  </details>
  
d.	С помощью команды show ip interface brief проверьте конфигурацию подынтерфейса.

<details>
  <summary>S2  F0/5 "show ip interface brief"</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/4-1-d.png">
</details>


##### Шаг 2. Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1

  <details><summary>Настройка S1</summary>
  <pre>
interface gigabitEthernet 0/0/1
no shutdown
description R2 for S2
ip add 10.20.0.4 255.255.255.0
exit

ip route 0.0.0.0 0.0.0.0 10.20.0.1

  </pre>
  </details>
  

<details>
  <summary>R2  "show ip interface brief"</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/4-2-1.png">
</details>

<details>
  <summary>R2 "show ip route"</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/4-2-2.png">
</details>

---

### Часть 5. Настройте удаленный доступ

##### Шаг 1. Настройте все сетевые устройства для базовой поддержки SSH.

a.	Создайте локального пользователя с именем пользователя SSHadmin и зашифрованным паролем $cisco123!

b.	Используйте ccna-lab.com в качестве доменного имени.

c.	Генерируйте криптоключи с помощью 1024 битного модуля.

d.	Настройте первые пять линий VTY на каждом устройстве, чтобы поддерживать только SSH-соединения и с локальной аутентификацией.

<details><summary>Настройка R1, R2, S1, S2</summary>
<pre>
conf term

ip ssh version 2
username SSHadmin password $cisco123!

ip domain name ccna-lab.com

crypto key generate rsa general-keys modulus 1024


line vty 0 5
login local
transport input ssh
exit
</pre>
</details>
  
  
 <details>
  <summary>S1 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/5-1-1.png">
</details>

 <details>
  <summary>S2 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/5-1-2.png">
</details>

 <details>
  <summary>R1  </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/5-1-3.png">
</details>

 <details>
  <summary>R2  </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/5-1-4.png">
</details>

##### Шаг 2. Включите защищенные веб-службы с проверкой подлинности на R1.

a.	Включите сервер HTTPS на R1.

R1(config)# ip http secure-server 

b.	Настройте R1 для проверки подлинности пользователей, пытающихся подключиться к веб-серверу.

R1(config)# ip http authentication local

 <details>
  <summary>S2  F0/5 "show ip route"</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/5-2-b.png">
</details>

---

### Часть 6. Проверка подключения

##### Шаг 1. Настройте узлы ПК

Адреса ПК можно посмотреть в таблице адресации.

##### Шаг 2. Выполните следующие тесты. Эхозапрос должен пройти успешно.

| № | От	| Протокол	| Назначение | Результат |
| :--:| :---: |:-----:| :---------:| :---------:|
|1| PC-A	| Ping	| 10.40.0.10 | Успех |
|2| PC-A	| Ping	| 10.20.0.1 | Успех |
|3| PC-B	| Ping	| 10.30.0.10 | Успех |
|4| PC-B	| Ping	| 10.20.0.1 | Успех |
|5| PC-B	| Ping	| 172.16.1.1 | Успех |
|6| PC-B	| HTTPS	| 10.20.0.1 | |
|7| PC-B	| HTTPS	| 172.16.1.1 | |
|8| PC-B	| SSH	| 10.20.0.1 | Успех |
|9| PC-B	| SSH	| 172.16.1.1 | Успех |

 <details>
  <summary>1 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-1.png">
</details>
  
 <details>
  <summary>2 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-2.png">
</details>
  
   <details>
  <summary>3 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-3.png">
</details>
  
<details>
  <summary>4 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-4.png">
</details>
  
<details>
  <summary>5 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-5.png">
</details>
  
<details>
  <summary>6 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-6.png">
</details>
  
<details>
  <summary>7 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-7.png">
</details>
  
<details>
  <summary>8 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-8.png">
</details>
  
<details>
  <summary>9 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/6-2-9.png">
</details>

  
  
---
### Часть 7. Настройка и проверка списков контроля доступа (ACL)

**При проверке базового подключения компания требует реализации следующих политик безопасности:**

**Политика1:**

1.1 Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен). 

**Политика 2:**

2.1 Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS).

2.2 Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. 

2.3 Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).

**Политика3:**

3.1 Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. 

**Политика 4:**

4.1 Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам. 

##### Шаг 1. Проанализируйте требования к сети и политике безопасности для планирования реализации ACL.

**Общее**

При создании списков контроля доступа будем опираться на именованные списки с целью понимания какой лист за что отвечает, и упращению введения документирования.

Сеть Sales имеет доступ к сети через коммутатор S2

Cеть Management имеет доступ к сети через коммутатор R2

Cеть Operations имеет доступ к сети через коммутатор S1

*Политика в отношении сети Sales*

1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен)
2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS)
3. Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола.
4. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам.

*Реализация:*

1. Применим расширенный список ACL.
2. 

*Политика в отношении сети Operations*

1. Cеть Operations  не может отправлять ICMP эхо запросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам.

*Реализация:*

1. Применим стандартный список ACL основанные только на исходном IPv4-адрес.
2. Список применем на порт R1 порт на входящий трафик запрещающий ссылку траффика в адрес сети 10.40.0.0 .Также пропишем разрешение на трафик в другие адреса сети

##### Шаг 2. Разработка и применение расширенных списков доступа, которые будут соответствовать требованиям политики безопасности.

  <details><summary>Настройка S1</summary>
  <pre>
ip access-list extended OUT_OPER_1
deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22
deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq www
deny icmp 10.40.0.0 0.0.0.255  10.30.0.0 0.0.0.255 
deny icmp 10.40.0.0 255.255.255.0 10.20.0.0 255.255.255.0
200 permit ip any any

interface g0/0/1.40
ip access-group OUT_OPER_1 in
ex
  </pre>
  </details>
  
 
  
  <details><summary>Настройка S1</summary>
  <pre>

ip access-list standard OUT_OPER_2
deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255
200 permit ip any any
remark Operations no ICMP echo requests to the Sales


R1(config)#interface g0/0/1
R1(config-if)#ip access-
R1(config-if)#ip access-group OPER_2 in
R1(config-if)#
R1(config-if)#
R1(config-if)#ex

  </pre>
  </details>
  



##### Шаг 3. Убедитесь, что политики безопасности применяются развернутыми списками доступа.

| № | От	| Протокол	| Назначение	| Результат | Состояние |
|:--:| :---: |:----:| :----------:| :--------:| :--------:|
|1| PC-A	| Ping	| 10.40.0.10	| Сбой | Выполнено |
|2| PC-A	| Ping	| 10.20.0.1	| Успех | Выполнено |
|3| PC-B	| Ping	| 10.30.0.10	| Сбой |  Выполнено |
|4| PC-B	| Ping	| 10.20.0.1	| Сбой | Выполнено |
|5| PC-B	| Ping	| 172.16.1.1	| Успех | Выполнено |
|6| PC-B	| HTTPS	| 10.20.0.1	| Сбой | |
|7| PC-B	| HTTPS	| 172.16.1.1	| Успех | |
|8| PC-B	| SSH	| 10.20.0.1	| Сбой | Выполнено |
|9| PC-B	| SSH	| 172.16.1.1	| Успех | Выполнено |



 <details>
  <summary>1-2 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/7-3-1.png">
</details>
  
    <details>
  <summary>3-5 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/7-3-3.png">
</details>
  

<details>
  <summary>6 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/7-3-6.png">
</details>
  
<details>
  <summary>7 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/7-3-7.png">
</details>
  
<details>
  <summary>8 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/7-3-8.png">
</details>
  
<details>
  <summary>9 </summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_11/pic/7-3-9.png">
</details>
