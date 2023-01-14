## Лабраторная работа - Настройка DHCPv6

### Топология

**Схема**

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/shema.png)


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
##### Часть 1. Создание сети и настройка основных параметров устройства
##### Часть 2. Проверка назначения адреса SLAAC от R1
##### Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1
##### Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1
##### Часть 5. Настройка и проверка DHCPv6 Relay на R2

---

## Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.

#### Шаг 1. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/1-1.png)

#### Шаг 2. Настройте базовые параметры каждого коммутатора. (необязательно)

<details><summary> Задание настройки  коммутаторов</summary>
<pre>
a.	Присвойте коммутатору имя устройства.
b.	Отключите поиск DNS
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
f.	Зашифруйте открытые пароли.
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
h.	Отключите все неиспользуемые порты.
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</pre>
</details>



<details><summary> Код S1 </summary>
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
service password-encryption
enable secret class
line vty 0 15
password cisco
login
exit

int range f0/1-4, f0/7-24, g0/1-2
shutdown
exit

exit
copy running-config startup-config

</pre>
</details>



<details><summary> Код S2 </summary>
<pre>

enable
conf term
no ip domain-lookup
hostname S2
banner motd ###### S2 ENTER PASSWORD ######
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

int range f0/1-4, f0/6-17, f0/19-24, g0/1-2
shutdown
exit

exit
copy running-config startup-config

</pre>
</details>

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/1-2-h1.png)

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/1-2-h2.png)


#### Шаг 3. Произведите базовую настройку маршрутизаторов.

<details><summary> Задание настройки маршрутизаторов </summary>
<pre>
a.	Назначьте маршрутизатору имя устройства.
b.	Отключите поиск DNS
c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.
d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.
e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.
f.	Зашифруйте открытые пароли.
g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.
h.	Активация IPv6-маршрутизации
i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.
</pre>
</details>


<details><summary> Код R1 </summary>
<pre>

enable
conf term
no ip domain-lookup
hostname R1
banner motd ###### R1 ENTER PASSWORD ######
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

ipv6 unicast-routing

exit
copy running-config startup-config

</pre>
</details>

<details><summary> Код R2 </summary>
<pre>

enable
conf term
no ip domain-lookup
hostname R2
banner motd ###### R2 ENTER PASSWORD ######
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

ipv6 unicast-routing

exit
copy running-config startup-config

</pre>
</details>


#### Шаг 4. Настройка интерфейсов и маршрутизации для обоих маршрутизаторов.

a.	Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше.

<details><summary> Код </summary>
<pre>

***R1***

int g0/0/1
ipv6 add 2001:db8:acad:1::1 link-local
no shutdown
ex

int g0/0/0
ipv6 add 2001:db8:acad:2::1 link-local
no shutdown
ex

***R2***

int g0/0/1
ipv6 add 2001:db8:acad:3::1 link-local
no shutdown
ex

int g0/0/0
ipv6 add 2001:db8:acad:2::2 link-local
no shutdown
ex
</pre>
</details>


![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/1-4-a.png)

b.	Настройте маршрут по умолчанию на каждом маршрутизаторе, который указывает на IP-адрес G0/0/0 на другом маршрутизаторе.

    код

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/1-4-b.png)


c.	Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/1-4-c.png)

d.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

---

### Часть 2. Проверка назначения адреса SLAAC от R1

В части 2 вы убедитесь, что узел PC-A получает адрес IPv6 с помощью метода SLAAC.

Включите PC-A и убедитесь, что сетевой адаптер настроен для автоматической настройки IPv6.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/1-2-1.png)

Через несколько минут результаты команды ipconfig должны показать, что PC-A присвоил себе адрес из сети 2001:db8:1::/64.


![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/1-2-2.png)


![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/1-2-3.png)

> Откуда взялась часть адреса с идентификатором хоста?
>
>    
---

### Часть 3. Настройка и проверка сервера DHCPv6 на R1

В части 3 выполняется настройка и проверка состояния DHCP-сервера на R1. Цель состоит в том, чтобы предоставить PC-A информацию о DNS-сервере и домене.

#### Шаг 1. Более подробно изучите конфигурацию PC-A.

a.	Выполните команду ipconfig /all на PC-A и посмотрите на результат.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/3-1-a.png)

b.	Обратите внимание, что основной DNS-суффикс отсутствует. Также обратите внимание, что предоставленные адреса DNS-сервера являются адресами «локального сайта anycast», а не одноадресные адреса, как ожидалось.


![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/3-1-b.png)

#### Шаг 2. Настройте R1 для предоставления DHCPv6 без состояния для PC-A.

a.	Создайте пул DHCP IPv6 на R1 с именем R1-STATELESS. В составе этого пула назначьте адрес DNS-сервера как 2001:db8:acad: :1, а имя домена — как stateless.com.

    ipv6 dhcp pool R1-STATELESS
    dns-server 2001:db8:acad::254
    domain-name STATELESS.com

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/3-2-a.png)

b.	Настройте интерфейс G0/0/1 на R1, чтобы предоставить флаг конфигурации OTHER для локальной сети R1 и укажите только что созданный пул DHCP в качестве ресурса DHCP для этого интерфейса.

    interface g0/0/1
    ipv6 nd other-config-flag
    ipv6 dhcp server R1-STATELESS

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/3-2-b.png)

c.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

d.	Перезапустите PC-A.

e.	Проверьте вывод ipconfig /all и обратите внимание на изменения.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/3-2-e.png)

f.	Тестирование подключения с помощью пинга IP-адреса интерфейса G0/1 R2.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/3-2-f.png)

---

### Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1

a.	Создайте пул DHCPv6 на R1 для сети 2001:db8:acad:3:aaa::/80. Это предоставит адреса локальной сети, подключенной к интерфейсу G0/0/1 на R2. В составе пула задайте DNS-сервер 2001:db8:acad: :254 и задайте доменное имя STATEFUL.com.
Откройте окно конфигурации

     ipv6 dhcp pool R2-STATEFUL
     address prefix 2001:db8:acad:3:aaa::/80
     dns-server 2001:db8:acad::254
     domain-name STATEFUL.com

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/4-a.png)

b.	Назначьте только что созданный пул DHCPv6 интерфейсу g0/0/0 на R1.

    interface g0/0/0
    ipv6 dhcp server R2-STATEFUL

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/4-b.png)

---

### Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.

В части 5 необходимо настроить и проверить ретрансляцию DHCPv6 на R2, позволяя PC-B получать адрес IPv6.

#### Шаг 1. Включите PC-B и проверьте адрес SLAAC, который он генерирует.

    ipconfig /all

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/5-1.png)

Обратите внимание на вывод, что используется префикс 2001:db8:acad:3::

#### Шаг 2. Настройте R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1.

a.	Настройте команду ipv6 dhcp relay на интерфейсе R2 G0/0/1, указав адрес назначения интерфейса G0/0/0 на R1. Также настройте команду **managed-config-flag** .

    интерфейс g0/0/1
    ipv6 nd managed-config-flag
    ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
    
![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/5-2-a.png)

b.	Сохраните конфигурацию.

#### Шаг 3. Попытка получить адрес IPv6 из DHCPv6 на PC-B.

a.	Перезапустите PC-B.

b.	Откройте командную строку на PC-B и выполните команду **ipconfig /all** и проверьте выходные данные, чтобы увидеть результаты операции ретрансляции DHCPv6.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/5-3-b.png)

c.	Проверьте подключение с помощью пинга IP-адреса интерфейса R0 G0/0/1.

![Alt-текст](https://github.com/fedotov1evg/OTUS_Network/blob/main/Lab_08/pic6/5-3-c.png)
