## Лабораторная работа - Настройка протоколов CDP, LLDP и NTP

**Топология**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/Shema.png "Схема")


**Таблица адресации**

| Устройство	| Интерфейс	| IP-адрес	| Маска подсети	| Шлюз по умолчанию |
| :---------: |:---------:| :--------:| :-----------: | :---------------: |
| R1	| Loopback1	| 172.16.1.1	| 255.255.255.0	| — |
| R1	| G0/0/1	| 10.22.0.1	| 255.255.255.0	| — |
| S1	| SVI | VLAN 1 	| 10.22.0.2	| 255.255.255.0	| 10.22.0.1 |
| S2	| SVI | VLAN 1	| 10.22.0.3	| 255.255.255.0	| 10.22.0.1 |

---
### Задачи
###### Часть 1. Создание сети и настройка основных параметров устройства
###### Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP
###### Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP
###### Часть 4. Настройка NTP

---

### Часть 1. Создание сети и настройка основных параметров устройства

#### Шаг 1. Создайте сеть согласно топологии.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/1.png)

#### Шаг 2. Настройте базовые параметры для маршрутизатора.

<details><summary> Задание </summary>
<pre>
a.	Назначьте маршрутизатору имя устройства.
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
f.	Зашифруйте открытые пароли.
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
h.	Настройка интерфейсов, перечисленных в таблице выше
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</pre>
</details>

<details><summary> Код настройки R1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R1
banner motd #####R1 ENTER PASSWORD##########
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

interface gigabitEthernet 0/0/1
description R1 for S1
ip add 10.22.0.1 255.255.255.0
no shutdown
exit

interface  Loopback1
ip add 172.16.1.1 255.255.255.0
no shutdown
exit

copy running-config startup-config
</pre>
</details>


#### Шаг 3. Настройте базовые параметры каждого коммутатора.

  <details><summary> ЗАДАНИЕ</summary>
  <pre>
a.	Присвойте коммутатору имя устройства.
b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
f.	Зашифруйте открытые пароли.
g.	Создайте баннер, который предупреждает всех, кто обращается к устройству, видит баннерное сообщение «Только авторизованные пользователи!».  
h.	Отключите неиспользуемые интерфейсы
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
  </pre>
  </details>
  
  <details><summary> Код настройки S1 </summary>
  <pre>
enable
conf term
no ip domain-lookup
hostname S1
banner motd "Only authorized users!"
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

interface range fastEthernet 0/2-4, fastEthernet 0/6-24, gigabitEthernet 0/1-2
shutdown
exit

interface range fastEthernet 0/1
description S1 for S2
exit

interface range fastEthernet 0/5
description S1 for R2
exit

copy running-config startup-config
  </pre>
  </details>


  <details><summary> Код настройки S2 </summary>
  <pre>
enable
conf term
no ip domain-lookup
hostname S2
banner motd "Only authorized users!"
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

interface range fastEthernet 0/2-24, gigabitEthernet 0/1-2
shutdown
exit

interface range fastEthernet 0/1
description S2 for S1
exit

copy running-config startup-config
  </pre>
  </details>


---

### Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP

a.	На R1 используйте соответствующую команду show cdp, чтобы определить, сколько интерфейсов включено CDP, сколько из них включено и сколько отключено.

    show cdp interface

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-aa.png)
 
> **Вопрос:** Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны?
> 
>    используется один  интерфейс GigabitEthernet0/0/1 
 
b.	На R1 используйте соответствующую команду show cdp, чтобы определить версию IOS, используемую на S1.

    show cdp entry  S1

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-b.png)

> Какая версия IOS используется на  S1?
>
>     15.0(2)SE4

  <details><summary> Лог R1 </summary>
  <pre>
Device ID: S1
Entry address(es): 
Platform: cisco 2960, Capabilities: Switch
Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5
Holdtime: 168

Version :
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2013 by Cisco Systems, Inc.
Compiled Wed 26-Jun-13 02:49 by mnguyen

advertisement version: 2
Duplex: full
  </pre>
  </details>
  

c.	На S1 используйте соответствующую команду show cdp, чтобы определить, сколько пакетов CDP было выданных.

    show cdp traffic


**!!Данной команды нет в Cisco packet tracer **


![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-c.png)

  <details><summary> Лог R1 </summary>
  <pre>
S1#show cdp ?
  entry      Information for specific neighbor entry
  interface  CDP interface status and configuration
  neighbors  CDP neighbor entries
  <cr>
  </pre>
  </details>

> Сколько пакетов имеет выход CDP с момента последнего сброса счетчика?
> 
>     Данной команды нет в Cisco packet tracer


d.	Настройте SVI для VLAN 1 на S1 и S2, используя IP-адреса, указанные в таблице адресации выше. Настройте шлюз по умолчанию для каждого коммутатора на основе таблицы адресов.
 
 
  <details><summary> Код настройки S1 </summary>
  <pre>
interface vlan 1
description SVI VLAN 1
ip add 10.22.0.2 255.255.255.0
no shutdown
exit
ip default-gateway 10.22.0.1
  </pre>
  </details>

  <details><summary> Код настройки S2 </summary>
  <pre>
interface vlan 1
description SVI VLAN 1
ip add 10.22.0.3 255.255.255.0
no shutdown
exit
ip default-gateway 10.22.0.1
  </pre>
  </details>
 
 <details>
  <summary>Screenshot</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-d1.png)">
</details>

<details>
  <summary>Screenshot</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-d2png)">
</details>
 
e.	На R1 выполните команду show cdp entry S1 . 

  <details><summary> Лог R1 -S1 </summary>
  <pre>
R1#show cdp entry S1

Device ID: S1
Entry address(es): 
  IP address : 10.22.0.2
Platform: cisco 2960, Capabilities: Switch
Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5
Holdtime: 133

Version :
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2013 by Cisco Systems, Inc.
Compiled Wed 26-Jun-13 02:49 by mnguyen

advertisement version: 2
Duplex: full
  </pre>
  </details>

> Вопрос: Какие дополнительные сведения доступны теперь?
> 
>    Ответ


f.	Отключить CDP глобально на всех устройствах. 

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-f1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-f2.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/2-f3.png)

---

### Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP

a.	Введите соответствующую команду lldp, чтобы включить LLDP на всех устройствах в топологии.

    

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/3-a1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/3-a2.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/3-a3.png)


b.	На S1 выполните соответствующую команду lldp, чтобы предоставить подробную информацию о S2. 

    show lldp entry S2

 
  
 **Данной команды нет СРТ** 
  
 ![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/3-b.png)
  
> Что такое chassis ID  для коммутатора S2?
> 
>    MAC-адрес устройства которое за портом Fa0/1


c.	Соединитесь через консоль на всех устройствах и используйте команды LLDP, необходимые для отображения топологии физической сети только из выходных данных команды show.
  
      show lldp neighbors
  

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/3-c1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/3-c2.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/3-c3.png)

---

### Часть 4. Настройка NTP

<details><summary> Задание </summary>
<pre>
В части 4 необходимо настроить маршрутизатор R1 в качестве сервера NTP, 
а маршрутизатор R2 в качестве клиента NTP маршрутизатора R1. Необходимо 
выполнить синхронизацию времени для Syslog и отладочных функций. Если 
время не синхронизировано, сложно определить, какое сетевое событие стало 
причиной данного сообщения.
</pre>
</details>

#### Шаг 1. Выведите на экран текущее время.

| Дата	| Время	| Часовой пояс	| Источник времени |
| :---: |:-----:| :------------:| :--------------: | 
| Mar 1 1993| *1:13:2.621| UTC | Time source is hardware calendar|

<details>
  <summary>Screenshot</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-1.png">
</details>


#### Шаг 2. Установите время.

С помощью команды clock set установите время на маршрутизаторе R1. Введенное время должно быть в формате UTC. 

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-2.png)

#### Шаг 3. Настройте главный сервер NTP.

Настройте R1 в качестве хозяина NTP с уровнем слоя 4.

    ntp server 172.16.1.1
    ntp master 4
    ntp update-calendar

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-3.png)

#### Шаг 4. Настройте клиент NTP.

a.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время. Запишите текущее время,  в следующей таблице.

    команда

| Дата	| Время	| Часовой пояс	| Источник времени |
| :---: |:-----:| :------------:| :--------------: | 
| Mon Mar 1 1993|1:39:37.247 |UTC |Time source is hardware calendar |
|Mon Mar 1 1993 |1:39:58.716|UTC |Time source is hardware calendar |

<details>
  <summary>S1</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-4-a1.png">
</details>

<details>
  <summary>S2</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-4-a2.png">
</details>

b.	Настройте S1 и S2 в качестве клиентов NTP. Используйте соответствующие команды NTP для получения времени от интерфейса G0/0/1 R1, а также для периодического обновления календаря или аппаратных часов коммутатора.

    ntp server 10.22.0.1

    Команды ntp update-calendar  для 2960 нет СРТ
  
![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-4-b3.png">
  
<details>
  <summary>S1</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-4-b1.png">
</details>

<details>
  <summary>S2</summary>
  <img src="https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-4-b2.png">
</details>


#### Шаг 5. Проверьте настройку NTP.

a.	Используйте соответствующую команду show , чтобы убедиться, что S1 и S2 синхронизированы с R1.
Примечание. Синхронизация метки времени на маршрутизаторе R2 с меткой времени на маршрутизаторе R1 может занять несколько минут.

    *команда*

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-5-a1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-5-a2.png)

b.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время и сравнить ранее записанное время.

    *команда*

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-5-b1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_13/pic/4-5-b2.png)

---

***Вопрос для повторения***

> Для каких интерфейсов в пределах сети не следует использовать протоколы обнаружения сетевых ресурсов?
>     
>     jndt
