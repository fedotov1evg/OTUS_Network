## Лабораторная работа. Настройка протокола OSPFv2 для одной области

**Топология**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/Shema.png "Схема")


**Таблица адресации**	

| Устройство	| Интерфейс	| IP-адрес	| Маска подсети |
| :---------: |:-----------:| :---------------:| :---------------:|
| R1	| G0/0/1	| 10.53.0.1	| 255.255.255.0 |
| R1	| Loopback1	| 172.16.1.1	| 255.255.255.0 |
| R2	| G0/0/1	| 10.53.0.2	| 255.255.255.0 |
| R2	| Loopback1	| 192.168.1.1	| 255.255.255.0 |

---

## Цели

###### Часть 1. Создание сети и настройка основных параметров устройства

###### Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области

###### Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области

---

### Часть 1. Создание сети и настройка основных параметров устройства

#### Шаг 1. Создайте сеть согласно топологии.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/1-1.png)

#### Шаг 2. Произведите базовую настройку маршрутизаторов.

<details><summary> Задание настройки маршрутизаторов </summary>
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

<details><summary> Код настройки R1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R1
banner motd ### R1 ENTER PASSWORD ####
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

<details><summary> Код настройки R2 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname R2
banner motd ### R2 ENTER PASSWORD ####
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


#### Шаг 3. Настройте базовые параметры каждого коммутатора.

<details><summary> Задание настройки коммутатора </summary>
<pre>

a.	Назначьте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

</pre>
</details>

<details><summary> Код настройки S1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S1
banner motd ### S1 ENTER PASSWORD ####
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


<details><summary> Код настройки S2 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S2
banner motd ### S2 ENTER PASSWORD ####
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

### Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области

#### Шаг 1. Настройте адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.

a.	Настройте адреса интерфейсов на каждом маршрутизаторе, как показано в таблице адресации выше.

<details><summary> лог настрйоки R1 </summary>
<pre>
R1(config)#int g0/0/1
R1(config-if)#ip add 10.53.0.1 255.255.255.0
R1(config-if)#no shutdown
R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
R1(config-if)#ex
R1(config)#int Loopback 1
R1(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up
R1(config-if)#ip add 172.16.1.1 255.255.255.0
R1(config-if)#no shutdown
R1(config-if)#ex
R1(config)#

</pre>
</details>

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-a11.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-a12.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-a21.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-a22.png)


b.	Перейдите в режим конфигурации маршрутизатора OSPF, используя идентификатор процесса 56.

    router ospf 56

c.	Настройте статический идентификатор маршрутизатора для каждого маршрутизатора (1.1.1.1 для R1, 2.2.2.2 для R2).

**S1**

    router-id 1.1.1.1

**s2**

    router-id 2.2.2.2

d.	Настройте инструкцию сети для сети между R1 и R2, поместив ее в область 0.

> S1
network 172.16.1.1 0.0.0.255 area 0
network 10.53.0.1 0.0.0.255 area 0

> s2
network 172.168.1.1 0.0.0.255 area 0
network 10.53.0.2 0.0.0.255 area 0

e.	Только на R2 добавьте конфигурацию, необходимую для объявления сети Loopback 1 в область OSPF 0.

    conf term
    interface loopback 1
    ip ospf 56 area 0
    exit
    
    router ospf 56
    passive-interface loopback 1
    exit
    
    
f.	Убедитесь, что OSPFv2 работает между маршрутизаторами. Выполните команду, чтобы убедиться, что R1 и R2 сформировали смежность.

**Команда: show ip ospf neighbor**

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-f1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-f2.png)

> Вопрос:Какой маршрутизатор является DR?
>
>     R1


> Какой маршрутизатор является BDR?
>
>     R2


> Каковы критерии отбора?
>
>     выбираются по значению Router ID. Маршрутизатор с наивысшим Router ID становится DR, 
>     а маршрутизатор со вторым наивысшим Router ID — BDR


g.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание, что поведение OSPF по умолчанию заключается в объявлении интерфейса обратной связи в качестве маршрута узла с использованием 32-битной маски.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-g.png)

h.	Запустите Ping до  адреса интерфейса R2 Loopback 1 из R1. Выполнение команды ping должно быть успешным.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/2-1-h.png)

---

### Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области

#### Шаг 1. Реализация различных оптимизаций на каждом маршрутизаторе.

a.	На R1 настройте приоритет OSPF интерфейса G0/0/1 на 50, чтобы убедиться, что R1 является назначенным маршрутизатором.

    interface g0/0/1
    ip ospf priority 50
    end
    clear ip ospf process

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-a.png)

b.	Настройте таймеры OSPF на G0/0/1 каждого маршрутизатора для таймера приветствия, составляющего 30 секунд.

     interface g0/0/1
     ip ospf hello-interval 30
     ip ospf dead-interval 120
     end
     
![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-b1.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-b2.png)

c.	На R1 настройте статический маршрут по умолчанию, который использует интерфейс Loopback 1 в качестве интерфейса выхода. Затем распространите маршрут по умолчанию в OSPF. Обратите внимание на сообщение консоли после установки маршрута по умолчанию.

    ip route 0.0.0.0 0.0.0.0 Loopback 1
    
    router ospf 56
    default-information originate
    end

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-c.png)

d.	добавьте конфигурацию, необходимую для OSPF для обработки R2 Loopback 1 как сети точка-точка. Это приводит к тому, что OSPF объявляет Loopback 1 использует маску подсети интерфейса.

   interface loopback 1
   ip ospf network point-to-point 

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-d.png)

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-d1.png)

e.	Только на R2 добавьте конфигурацию, необходимую для предотвращения отправки объявлений OSPF в сеть Loopback 1.

     router ospf 56
     passive-interface loopback 1
     end

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-e.png)

f.	Измените базовую пропускную способность для маршрутизаторов. После этой настройки перезапустите OSPF с помощью команды clear ip ospf process . Обратите внимание на сообщение консоли после установки новой опорной полосы пропускания.

    router ospf 56
    auto-cost reference-bandwidth 10000


![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-f1.png)


![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-f2.png)


![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-f3.png)


![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-1-f4.png)



#### Шаг 2. Убедитесь, что оптимизация OSPFv2 реализовалась.

a.	Выполните команду show ip ospf interface g0/0/1 на R1 и убедитесь, что приоритет интерфейса установлен равным 50, а временные интервалы — Hello 30, Dead 120, а тип сети по умолчанию — Broadcast

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-2-a.png)

b.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание на разницу в метрике между этим выходным и предыдущим выходным. Также обратите внимание, что маска теперь составляет 24 бита, в отличие от 32 битов, ранее объявленных.



![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-2-b.png)

c.	Введите команду show ip route ospf на маршрутизаторе R2. Единственная информация о маршруте OSPF должна быть распространяемый по умолчанию маршрут R1.


![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-2-c.png)

d.	Запустите Ping до адреса интерфейса R1 Loopback 1 из R2. Выполнение команды ping должно быть успешным.

![alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_10/pic/3-2-d.png)


---

> Вопрос:Почему стоимость OSPF для маршрута по умолчанию отличается от стоимости OSPF в R1 для сети 192.168.1.0/24?
>
> Основной метрикой, используемой в OSPF для выбора лучшего маршрута, является стоимость: в таблицу маршрутизации будет добавлен маршрут с меньшим значением стоимости.  Значение стоимости указывается в качестве параметра каждого интерфейса, ассоциированного с OSPF и может принимать значения от 1 до 65535. Парметр "O" что маршрут построен OSPF и его стоймость рассчитана в зависимости от параметра интерфейса. O+E2 внешние маршруты 2-го типа (External), то есть они были введены в процесс OSPF извне и его стоймость равна 1, по умолчанию, но можем сами назначит ей необходимую стоймость. 
Для маршрута 2 типа стоимость  при передаче по сети не увеличивается.


