# Домашняя работа. Настройка IPv6-адресов на сетевых устройствах

## Задачи
#### Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
#### Часть 2. Ручная настройка IPv6-адресов
#### Часть 3. Проверка сквозного соединения



## Часть 1. Создание и настройка сети
Топология сети

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/Shema.png "Схема")

Таблица адресации настроенной сети 

Устройство	Интерфейс	IPv6-адрес	Длина префикса	Шлюз по умолчанию

| Устройство | 	Интерфейс |	IPv6-адрес |	Длина префикса |	Шлюз по умолчанию |
| :--------: |:--------:  |:---------:| :--------------:| :-----------------:|
| R1	       | G0/0/0    | 2001:db8:acad:a ::1  |	64	|  —  |
| R1	       | G0/0/1	   | 2001:db8:acad:1::1  |	64	|  —  |
| S1	       |  VLAN 1	 | 2001:db8:acad:1::b	 |  64	|  —  |
| PC-A     	|  NIC	     | 2001:db8:acad:1::3	 |  64	| fe80::1 |
| PC-B     	|  NIC	     | 2001:db8:acad:a : :3	 |  64	| fe80::1 |

#### Шаг 1. Настройте маршрутизатор

<details><summary>Пример комманд настройки R1 </summary>
<pre>

enable
conf term
no ip domain-lookup
hostname R1
banner motd ##### R1 ENTER PASSWORD #####
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

#### Шаг 2. Настройте коммутатор.

<details><summary>Пример комманд настройки S1 </summary>
<pre>

enable
conf term
no ip domain-lookup
hostname S1
banner motd ##### S1 ENTER PASSWORD #####
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

***

## Часть 2. Ручная настройка IPv6-адресов

#### Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1
a.	Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1.

b.	Введите команду show ipv6 interface brief, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-1-b.png)

c.	Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вручную введите локальные адреса канала на каждом интерфейсе Ethernet на R1.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-1-c.png)

d.	Используйте выбранную команду, чтобы убедиться, что локальный адрес связи изменен на fe80::1.  

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-1-d1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-1-d2.png)

> *Какие группы многоадресной рассылки назначены интерфейсу G0/0?*
> > Joined group address(es):    FF02::1    FF02::2    FF02::1:FF00:1


#### Шаг 2. Активируйте IPv6-маршрутизацию на R1.

a В командной строке на PC-B введите команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-2-a.png)

> *Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B?*
> > Да назначен адрес 


b.	Активируйте IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing.

c.	Теперь, когда R1 входит в группу многоадресной рассылки всех маршрутизаторов, еще раз введите команду ipconfig на PC-B. Проверьте данные IPv6-адреса.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-2-c.png)


> *Почему PC-B получил глобальный префикс маршрутизации и идентификатор подсети, которые вы настроили на R1?*
>> РС получает свой IPv6-адреса и префикс от маршрутизатора т.к. в нем включена IPv6 адресация



#### Шаг 3. Назначьте IPv6-адреса интерфейсу управления (SVI) на S1.

a.	Назначьте адрес IPv6 для S1. Также назначьте этому интерфейсу локальный адрес канала.

b.	Проверьте правильность назначения IPv6-адресов интерфейсу управления с помощью команды show ipv6 interface vlan1.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-3-b.png)


#### Шаг 4. Назначьте компьютерам статические IPv6-адреса

a.	Откройте окно Свойства Ethernet для каждого ПК и назначьте адресацию IPv6.

b.	Убедитесь, что оба компьютера имеют правильную информацию адреса IPv6. Каждый компьютер должен иметь два глобальных адреса IPv6: один статический и один SLACC

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-4-a.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/2-4-a2.png)

***

#### Часть 3. Проверка сквозного соединения

**С PC-A отправьте эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/3-3-1.png)

**Отправьте эхо-запрос на интерфейс управления S1 с PC-A**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/3-3-2.png)


**Введите команду tracert на PC-A, чтобы проверить наличие сквозного подключения к PC-B**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/3-3-3.png)

**С PC-B отправьте эхо-запрос на PC-A**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/3-3-4.png)

**С PC-B отправьте эхо-запрос на локальный адрес канала G0/0 на R1**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_04/pic/3-3-5.png)


***


##### Вопросы для повторения

> *1.	Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот же локальный адрес канала — FE80::1?*
>> Т.к. каждый интерфейс маршрутизатора относиться к отдельной сети



> *2.	Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64?*
>> идентификатор подсети **2001:db8:acad::aaaa:**










