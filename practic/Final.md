##### Часть 1. Проверка сети






##### Часть 2. Настройка коммутаторов

<details><summary> Код настройки S1 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S1
banner motd #####S1 ENTER PASSWORD##########
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

int g0/0/0
ip add 209.165.200.225 255.255.255.248
no shutdown
exit

int Lo 1
ip add 209.165.200.1 255.255.255.224
no shutdown
exit

ip route 192.168.1.0 255.255.255.0 209.165.200.230

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
banner motd #####S2 ENTER PASSWORD##########
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

int g0/0/0
ip add 209.165.200.225 255.255.255.248
no shutdown
exit

int Lo 1
ip add 209.165.200.1 255.255.255.224
no shutdown
exit

ip route 192.168.1.0 255.255.255.0 209.165.200.230

exit
copy running-config startup-config
</pre>
</details>


<details><summary> Код настройки S3 </summary>
<pre>
enable
conf term
no ip domain-lookup
hostname S3
banner motd #####S3 ENTER PASSWORD##########
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

int g0/0/0
ip add 209.165.200.225 255.255.255.248
no shutdown
exit

int Lo 1
ip add 209.165.200.1 255.255.255.224
no shutdown
exit

ip route 192.168.1.0 255.255.255.0 209.165.200.230

exit
copy running-config startup-config
</pre>
</details>





##### Часть 3. Настройка роутеров 

##### Часть 4. Проверка связи 
