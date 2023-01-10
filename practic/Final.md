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

  <details><summary> LACP SW2 </summary>
  <pre>
   
   
  </pre>
  </details>


  <details><summary> LACP SW3 </summary>
  <pre>
   
   
  </pre>
  </details>



#### 2.3 Настройка PagP  (SW 2 SW1)

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


#### 2.3 Настройка RoS  (SW 1 SW3)

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

  <details><summary>  HSRP R1 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>
  
  <details><summary>  HSRP R2 </summary>
  <pre>
   ТЕКСТ ТЕКСТ ТЕКСТ ТЕКСТ
  </pre>
  </details>


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

## Часть 4. Проверка связи и конфигураций



