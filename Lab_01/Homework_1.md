# Network Engineer. Basic. Домашнее задание по лекции №2.

## Лабораторная работа. Базовая настройка коммутатора

**Схема**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/Shema.png)


**Исходные данные**
таблица адресации

| Устройство  | Интерфейс   | IP-адрес/префикс|
| :--------- |:------------:| :-----:|
| S1      | VLAN 1 | 192.168.1.2/24 |
| PC-A    | NIC  |  192.168.1.10/24 |

### Задачи

[Часть 1. Проверка конфигурации коммутатора по умолчанию](https://github.com/fedotov1evg/OTUS_Network/edit/main/Lab_01/Homework_1.md "Часть 1. Проверка конфигурации коммутатора по умолчанию")
[Часть 2. Создание сети и настройка основных параметров устройства](https://github.com/fedotov1evg/OTUS_Network/edit/main/Lab_01/Homework_1.md "Часть 2. Создание сети и настройка основных параметров устройства")
[Часть 3. Проверка сетевых подключений](https://github.com/fedotov1evg/OTUS_Network/edit/main/Lab_01/Homework_1.md "Часть 3. Проверка сетевых подключений")

#### Часть 1. Проверка конфигурации коммутатора по умолчанию
#### Часть 2. Создание сети и настройка основных параметров устройства
#### Часть 3. Проверка сетевых подключений

---

### Часть 1. Создание сети и проверка настроек коммутатора по умолчанию

#### Шаг 1. Создайте сеть согласно топологии.

*a.	Подсоедините консольный кабель, как показано в топологии. На данном этапе не подключайте кабель Ethernet компьютера PC-A.*

*b.	Установите консольное подключение к коммутатору с компьютера PC-A с помощью Tera Term или другой программы эмуляции терминала.*

> Почему нужно использовать консольное подключение для первоначальной настройки коммутатора? 
>> В коммутаторе отсутсвуют настройки протоколов для удаленого подключения. 

> Почему нельзя подключиться к коммутатору через Telnet или SSH?
>> Нет настроек данных протоколов в комутаторе что не позволяет подключиться к нему.


#### Шаг 2. Проверьте настройки коммутатора по умолчанию.

b.	Изучите текущий файл running configuration.
> Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?
>>  24

> Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?
>> 2

> Каков диапазон значений, отображаемых в vty-линиях?
>> 0 4  
>> 5 15

c.	Изучите файл загрузочной конфигурации (startup configuration), который содержится в энергонезависимом ОЗУ (NVRAM).

Появилось сообщение ***startup-config is not present***

> Почему появляется это сообщение?
>> Данного файла конфигурации нет в коммутаторе. Это файл конфигурации с пользовательскими настройками, коммутатор не настроен и не сохранены настройки.

d.	Изучите характеристики SVI для VLAN 1.


> Назначен ли IP-адрес сети VLAN 1?
>> Нет

> Какой MAC-адрес имеет SVI?
>> 0060.5c4a.e706

>Данный интерфейс включен?
>> Нет. Отключен


e.	Изучите IP-свойства интерфейса SVI сети VLAN 1.
> Какие выходные данные вы видите?
>> Ip Адреса нет

f.	Подсоедините кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучите IP-свойства интерфейса SVI сети VLAN 1. Дождитесь согласования параметров скорости и дуплекса между коммутатором и ПК.

> Какие выходные данные вы видите?
>> Full-duplex, 100Mb/s


g.	Изучите сведения о версии ОС Cisco IOS на коммутаторе.


> Под управлением какой версии ОС Cisco IOS работает коммутатор?
>> Version 15.0(2)SE4

> Как называется файл образа системы?
>> 2960-lanbasek9-mz.150-2.SE4.bin

> Какой базовый MAC-адрес назначен коммутатору?
>>00:60:3E:DE:7B:9D

h.	Изучите свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.
Switch# show interface f0/6 

<details><summary>Лог</summary>
<pre>
    Switch>show interface f0/6
FastEthernet0/6 is up, line protocol is up (connected)
  Hardware is Lance, address is 0060.5c4a.e706 (bia 0060.5c4a.e706)
 BW 100000 Kbit, DLY 1000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s
  input flow-control is off, output flow-control is off
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:08, output 00:00:05, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue :0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     956 packets input, 193351 bytes, 0 no buffer
     Received 956 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     2357 packets output, 263570 bytes, 0 underruns
     0 output errors, 0 collisions, 10 interface resets
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier
     0 output buffer failures, 0 output buffers swapped out

</pre>
</details>

> Интерфейс включен или выключен?
>> Включен


> Что нужно сделать, чтобы включить интерфейс?
>> ввести команду **noshutdown**


> Какой MAC-адрес у интерфейса?
>> 0060.5c4a.e706


> Какие настройки скорости и дуплекса заданы в интерфейсе?
>> Full-duplex, 100Mb/s


i.	Изучите параметры сети VLAN по умолчанию на коммутаторе.

<details><summary>Лог Vlan (brief) </summary>
<pre>
Switch>show  vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active   
</pre>
</details>


<details><summary>Лог Vlan 1</summary>
<pre>
Switch>show interfaces vlan 1
Vlan1 is administratively down, line protocol is down
  Hardware is CPU Interface, address is 0004.9ab3.922e (bia 0004.9ab3.922e)
  MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 21:40:21, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1682 packets input, 530955 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicast)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     563859 packets output, 0 bytes, 0 underruns
     0 output errors, 23 interface resets
     0 output buffer failures, 0 output buffers swapped out
</pre>
</details>

> Какое имя присвоено сети VLAN 1 по умолчанию?
>> default (по умолчанию нет имени)

> Какие порты расположены в сети VLAN 1?
>> Все

> Активна ли сеть VLAN 1?
>> да

> К какому типу сетей VLAN принадлежит VLAN по умолчанию?
> >  VLAN1


j.	Изучите флеш-память.
Выполните одну из следующих команд, чтобы изучить содержимое флеш-каталога.

Switch# show flash 

Switch# dir flash: 


<details><summary>Лог </summary>
<pre>
Switch#dir flash:
Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin

64016384 bytes total (59345929 bytes free)
Switch#show flash
Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin

64016384 bytes total (59345929 bytes free)
</pre>
</details>

В конце имени файла указано расширение, например .bin. Каталоги не имеют расширения файла.

> Какое имя присвоено образу Cisco IOS?
>> 2960-lanbasek9-mz.150-2.SE4
---

### Часть 2. Настройка базовых параметров сетевых устройств

#### Шаг 1. Настройте базовые параметры коммутатора.

a.	В режиме глобальной конфигурации скопируйте следующие базовые параметры конфигурации и вставьте их в файл на коммутаторе S1. 

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/2-1-a.png)


b.	Назначьте IP-адрес интерфейсу SVI на коммутаторе. Благодаря этому вы получите возможность удаленного управления коммутатором.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/2-1-b.png)


c.	Доступ через порт консоли также следует ограничить  с помощью пароля. Используйте cisco в качестве пароля для входа в консоль в этом задании. Конфигурация по умолчанию разрешает все консольные подключения без пароля. Чтобы консольные сообщения не прерывали выполнение команд, используйте параметр logging synchronous.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/2-2-c.png)


d.	Настройте каналы виртуального соединения для удаленного управления (vty), чтобы коммутатор разрешил доступ через Telnet. Если не настроить пароль VTY, будет невозможно подключиться к коммутатору по протоколу Telnet.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/2-2-d.png)

> Для чего нужна команда login?
>> С помощью команды login указываем циске при подключении запрос пароля 

#### Шаг 2. Настройте IP-адрес на компьютере PC-A.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/2-2.png)

---

### Часть 3. Проверка сетевых подключений	

#### Шаг 1. Отобразите конфигурацию коммутатора.

a. Параметры, которые настроили, выделены желтым. 

<details><summary> Конфигурация коммутатора </summary>
<pre>
    S1#show run
Building configuration...

Current configuration : 1302 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 shutdown
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 ip address 192.168.1.2 255.255.255.0
!
banner motd ^C
Unauthorized acces is strictly prohibited. ^C
!
!
!
line con 0
 logging synchronous
 login
!
line vty 0 4
 password 7 0822455D0A16
 login
line vty 5 15
 password 7 0822455D0A16
 login
!
!
!
!
end
    
</pre>
</details>


b.	Проверьте параметры VLAN 1.

<details><summary>Лог</summary>
<pre>
    S1#show interface vlan 1
Vlan1 is up, line protocol is up
  Hardware is CPU Interface, address is 0060.3ede.7b9d (bia 0060.3ede.7b9d)
  Internet address is 192.168.1.2/24
  MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 21:40:21, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1682 packets input, 530955 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicast)
     0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     563859 packets output, 0 bytes, 0 underruns
     0 output errors, 23 interface resets
     0 output buffer failures, 0 output buffers swapped out
</pre>
</details>


> Какова полоса пропускания этого интерфейса?
>>MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,
     reliability 255/255, txload 1/255, rxload 1/255

> В каком состоянии находится VLAN 1?
>> Vlan1 is up, 

> В каком состоянии находится канальный протокол?
>> работает - line protocol is up


#### Шаг 2. Протестируйте сквозное соединение, отправив эхо-запрос.

a.	В командной строке компьютера PC-A с помощью утилиты ping проверьте связь сначала с адресом S-1.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/3-2-b.png)

b.	Из командной строки компьютера PC-A отправьте эхо-запрос на административный адрес интерфейса SVI коммутатора S1.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/3-2-a.png)


#### Шаг 3. Проверьте удаленное управление коммутатором S1.

a.	Откройте Tera Term или другую программу эмуляции терминала с возможностью Telnet. 

b.	Выберите сервер Telnet и укажите адрес управления SVI для подключения к S1.  Пароль: cisco.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/3-3-1.png)

c.	После ввода пароля cisco вы окажетесь в командной строке пользовательского режима. Для перехода в исполнительский режим EXEC введите команду enable и используйте секретный пароль class.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/3-3-2.png)

d.	Сохраните конфигурацию.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_01/PIC/3-3-3.png)

e.	Чтобы завершить сеанс Telnet, введите exit.


***Вопросы для повторения***

> 1.	Зачем необходимо настраивать пароль VTY для коммутатора?
>> Для защиты от несанционированного доступа

> 2.	Что нужно сделать, чтобы пароли не отправлялись в незашифрованном виде?
>> Чтобы зашифровать пароли, используется команда глобальной конфигурации ***service password-encryption.***





















