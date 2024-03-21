# Сети в Linux

Настройка сетей в Linux на виртуальных машинах


## Оглавление
[Часть 1. Инструмент ipcalc](#часть-1-инструмент-ipcalc)  
[Часть 2. Статическая маршрутизация между двумя машинами](#часть-2-статическая-маршрутизация-между-двумя-машинами)  
[Часть 3. Утилита iperf3](#часть-3-утилита-iperf3)  
[Часть 4. Сетевой экран](#часть-4-сетевой-экран)  
[Часть 5. Статическая маршрутизация сети](#часть-5-статическая-маршрутизация-сети)  
[Часть 6. Динамическая настройка IP с помощью DHCP](#часть-6-динамическая-настройка-ip-с-помощью-dhcp)  
[Часть 7. NAT](#часть-7-nat)  
[Часть 8. Допополнительно. Знакомство с SSH Tunnels](#часть-8-дополнительно-знакомство-с-ssh-tunnels) 


## Часть 1. Инструмент ipcalc

>ipcalc предоставляет простой метод для вычисления IP-информации хоста. 

### 1.1. Сети и маски

Устанавливаю утилиту `sudo apt-get install ipcalc`

#### Адрес сети 192.167.38.54/13

Вычисляю адрес сети `ipcalc 192.167.38.54/13`

![адрес сети 192.167.38.54/13](misc/part_1/1.png)

- 192.160.0.0/13

#### Перевод маски 255.255.255.0 в префиксную и двоичную запись

Вычисляю маску сети `ipcalc 0.0.0.0/255.255.255.0`

![маска сети 255.255.255.0](misc/part_1/2.png)

- 11111111.11111111.11111111.00000000 - двоичная запись

- 24 - префиксная запись

#### Перевод маски /15 в обычную и двоичную запись

Вычисляю маску сети `ipcalc 0.0.0.0/15`

![маска сети 0.0.0.0/15](misc/part_1/3.png)

- 255.254.0.0 - обычный вид

- 11111111.11111110.00000000.00000000 - двоичная запись

#### Перевод маски 1111111.11111111.11111111.11110000 в обычную и префиксную запись

>ipcalc не работает с двоичной записью

Вычисляю маску сети `ipcalc 0.0.0.0/28`

![маска сети 0.0.0.0/28](misc/part_1/4.png)

- 255.255.255.240 - обычный вид

- 28 - префиксная запись

#### Минимальный и максимальный хост в сети 12.167.38.4/8, 11111111.11111111.00000000.00000000, 255.255.254.0 и /4

`ipcalc 12.167.38.4/8`

![ipcalc 12.167.38.4/8](misc/part_1/5.png)

- HostMin: 12.0.0.1
- HostMax: 12.255.255.254

#### Минимальный и максимальный хост в сети 12.167.38.4/11111111.11111111.00000000.00000000

`ipcalc 12.167.38.4/16`

![ipcalc 12.167.38.4/16](misc/part_1/6.png)

- HostMin: 12.167.0.1
- HostMax: 12.167.255.254

#### Минимальный и максимальный хост в сети 12.167.38.4/255.255.254.0 

`ipcalc 12.167.38.4/255.255.254.0`

![ipcalc 12.167.38.4/255.255.254.0](misc/part_1/7.png)

- HostMin: 12.167.38.1
- HostMax: 12.167.39.254

#### Минимальный и максимальный хост в сети 12.167.38.4/4

`ipcalc 12.167.38.4/4`

![ipcalc 12.167.38.4/4](misc/part_1/8.png)

- HostMin: 0.0.0.1
- HostMax: 15.255.255.254

### 1.2. localhost

`ipcalc 194.34.23.100`

![ipcalc 194.34.23.100](misc/part_1/9.png)

- Нельзя обратиться

`ipcalc 127.0.0.2`

![ipcalc 127.0.0.2](misc/part_1/10.png)

- Можно обратиться

`ipcalc 127.1.0.1`

![ipcalc 127.1.0.1](misc/part_1/11.png)

- Можно обратиться

`ipcalc 128.0.0.1`

![ipcalc 128.0.0.1](misc/part_1/12.png)

- Нельзя обратиться

### 1.3. Диапазоны и сегменты сетей
#### какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: 10.0.0.45, 134.43.0.2, 192.168.4.2, 172.20.250.4, 172.0.2.1, 192.172.0.1, 172.68.0.2, 172.16.255.255, 10.10.10.10, 192.169.168.1

`ipcalc 10.0.0.45`

![ipcalc 10.0.0.45](misc/part_1/13.png)

- Частный IP

`ipcalc 134.43.0.2`

![ipcalc 134.43.0.2](misc/part_1/14.png)

- Публичный IP

`ipcalc 192.168.4.2`

![ipcalc 192.168.4.2](misc/part_1/15.png)

- Частный IP

`ipcalc 172.20.250.4`

![ipcalc 172.20.250.4](misc/part_1/16.png)

- Частный IP

`ipcalc 172.0.2.1`

![ipcalc 172.0.2.1](misc/part_1/17.png)

- Публичный IP

`ipcalc 192.172.0.1`

![ipcalc 192.172.0.1](misc/part_1/18.png)

- Публичный IP

`ipcalc 172.68.0.2`

![ipcalc 172.68.0.2](misc/part_1/19.png)

- Публичный IP

`ipcalc 172.16.255.255`

![ipcalc 172.16.255.255](misc/part_1/20.png)

- Частный IP

`ipcalc 10.10.10.10`

![ipcalc 10.10.10.10](misc/part_1/21.png)

- Частный IP

`ipcalc 192.169.168.1`

![ipcalc 192.169.168.1](misc/part_1/22.png)

- Публичный IP

Частные:
- 10.0.0.45
- 192.168.4.2
- 172.20.250.4
- 172.16.255.255
- 10.10.10.10

Публичные:
- 134.43.0.2
- 172.0.2.1
- 192.172.0.1
- 172.68.0.2
- 192.169.168.1


#### какие из перечисленных IP адресов шлюза возможны у сети 10.10.0.0/18: 10.0.0.1, 10.10.0.2, 10.10.10.10, 10.10.100.1, 10.10.1.255

`ipcalc 10.10.0.0/18`

![ipcalc 10.10.0.0/18](misc/part_1/23.png)

- HostMin: 10.10.0.1
- HostMax: 10.10.63.254

Возможные шлюзы для сети: 10.10.0.2, 10.10.10.10, 10.10.1.255

## Часть 2. Статическая маршрутизация между двумя машинами
#### Поднять две виртуальные машины (далее -- ws1 и ws2)

Клонирую первую машину, добавляю локальную сеть, устанавливаю имя хоста ws2 `sudo hostnamectl set-hostname ws2`

##### С помощью команды ip a посмотреть существующие сетевые интерфейсы

ws1 `ip a`

![ws1 ip a](misc/part_2/1.png)

ws2 `ip a`

![ws2 ip a](misc/part_2/2.png)


##### На обеих машинах задать следующие адреса и маски: ws1 - 192.168.100.10, маска /16, ws2 - 172.24.116.8, маска /12

Для каждой из машин `sudo nano /etc/netplan/00-installer-config.yaml` 

![netplan config ws1](misc/part_2/3.png)
![netplan config ws2](misc/part_2/4.png)

Применяем настройки `sudo netplan apply` 

![netplan config ws1](misc/part_2/5.png)

![netplan config ws1](misc/part_2/6.png)

### 2.1. Добавление статического маршрута вручную

`sudo ip r add 172.24.116.8 dev enp0s3`

![ws1 ip r add](misc/part_2/7.png)

`sudo ip r add 192.168.100.10 dev enp0s3`

![ws2 ip r add](misc/part_2/8.png)

#### Пингую соединение между машинами

`ping -c 4 172.24.116.8`

![ping ws2](misc/part_2/9.png)

`ping -c 4 192.168.100.10`

![ping ws1](misc/part_2/10.png)

### 2.2. Добавление статического маршрута с сохранением

Перезапускаю машины `sudo reboot`

#### Добавить статический маршрут от одной машины до другой с помощью файла etc/netplan/00-installer-config.yaml

Для каждой из машин `sudo nano /etc/netplan/00-installer-config.yaml` 

![netplan config ws1](misc/part_2/11.png)
![netplan config ws2](misc/part_2/12.png)

Применяем настройки `sudo netplan apply` 

#### Пингую соединение между машинами

`ping -c 4 172.24.116.8`

![ping ws2](misc/part_2/13.png)

`ping -c 4 192.168.100.10`

![ping ws1](misc/part_2/14.png)

## Часть 3. Утилита iperf3
#### Перевести и записать в отчёт: 8 Mbps в MB/s, 100 MB/s в Kbps, 1 Gbps в Mbps

- 8 Mbps = 1 MB/s
- 100MB/s = 800000Kbps
- 1Gbps = 1000Mbps

#### Измерить скорость соединения между ws1 и ws2

Слушаю на стороне сервера `iperf3 -s`

![iperf3 -s](misc/part_3/1.png)

На стороне клиента `-с 192.168.100.10`

![iperf3 -с 192.168.100.10](misc/part_3/2.png)

## Часть 4. Сетевой экран

### 4.1. Утилита iptables

Создаю файл /etc/firewall.sh, имитирующий фаервол на обеих машинах `sudo nano /etc/firewall.sh`

![ws1 /etc/firewall.sh](misc/part_4/1.png)

![ws2 /etc/firewall.sh](misc/part_4/2.png)

Запускаю файлы на обеих машинах командами `sudo chmod +x /etc/firewall.sh` и `sudo /etc/firewall.sh`

> Разница заключается в порядке исполнения правил: в первом случае, пакет будет отклонен, во втором - принят.

### 4.2. Утилита nmap

 Пингую со второй машины на первую `ping 192.168.100.10`

Затем вызываю утилиту nmap `sudo nmap 192.168.100.10`

![nmap](misc/part_4/3.png)

## Часть 5. Статическая маршрутизация сети
### 5.1. Настройка адресов машин
#### Настраиваю конфигурацию машин согласно схеме на рисунке

![network](../misc/images/part5_network.png)


ws11 

![ws11 etc/netplan/00-installer-config.yaml](misc/part_5/1.png)

ws22

![ws22 etc/netplan/00-installer-config.yaml](misc/part_5/2.png)

ws21 

![ws21 etc/netplan/00-installer-config.yaml](misc/part_5/3.png)

r1

![r1 etc/netplan/00-installer-config.yaml](misc/part_5/4.png)

r2

![r2 etc/netplan/00-installer-config.yaml](misc/part_5/5.png)

Применяю настройки на каждой из машин `sudo netplan apply`

Проверяю настройки на каждой машине `ip -4 a`

ws11 

![ws11 ip -4 a](misc/part_5/6.png)

ws22 

![ws22 ip -4 a](misc/part_5/7.png)

ws21 

![ws21 ip -4 a](misc/part_5/8.png)

r1 

![r1 ip -4 a](misc/part_5/9.png)

r2

![r2 ip -4 a](misc/part_5/10.png)

Пингую ws22 с ws21 `ping -c 4 10.20.0.20`

![ping ws22 с ws21](misc/part_5/11.png)

Пингую r1 с ws11 `ping -c 4 10.10.0.1`

![ping r1 с ws11](misc/part_5/12.png)

### 5.2. Включение переадресации IP-адресов.

Включаю переадресацию IP, на роутерах `sudo sysctl -w net.ipv4.ip_forward=1`

![переадресации IP r1](misc/part_5/13.png)

![переадресации IP r2](misc/part_5/14.png)

Открываю файл конфигурации на обеих машинах `sudo nano /etc/sysctl.conf`

![/etc/sysctl.conf](misc/part_5/15.png)

### 5.3. Установка маршрута по-умолчанию
#### Настриваю маршрут по-умолчанию (шлюз) для рабочих станций
ws11 

![ws11 etc/netplan/00-installer-config.yaml](misc/part_5/16.png)

ws22

![ws22 etc/netplan/00-installer-config.yaml](misc/part_5/17.png)

ws21 

![ws21 etc/netplan/00-installer-config.yaml](misc/part_5/18.png)

Применяю настройки на каждой из машин `sudo netplan apply`

#### Проверею таблицы маршрутиризации

ws11 `ip r`

![ws11 ip r](misc/part_5/19.png)

ws22 `ip r`

![ws22 ip r](misc/part_5/21.png)

ws21 `ip r`

![ws21 ip r](misc/part_5/20.png)

#### Пингую r2 с ws11

`ping -c 4 10.100.0.12`

![ws21 ip r](misc/part_5/22.png)

Смотрю в r2 `sudo tcpdump -tn -i enp0s8`

![ws21 ip r](misc/part_5/23.png)

### 5.4. Добавление статических маршрутов

Добавляю в роутеры r1 и r2 статические маршруты в файле конфигурации

r1

![r1 etc/netplan/00-installer-config.yaml](misc/part_5/24.png)

r2

![r2 etc/netplan/00-installer-config.yaml](misc/part_5/25.png)

Применяю настройки на обеих роутерах `sudo netplan apply`


r1 `ip r`

![r1 ip r](misc/part_5/26.png)

r2 `ip r`

![r2 ip r](misc/part_5/27.png)

Запускаю команды `ip r list 10.10.0.0/18` и `ip r list 0.0.0.0/0` на ws11

![r2 ip r](misc/part_5/28.png)

>ws11 соединена с сетью 10.10.0.0/18 поэтому, подключение произошло напрямую, но не соединена с 0.0.0.0/0 - сработало дефолное подключение через роутер.

### 5.5. Построение списка маршрутизаторов

 Запускаю команду дампа на r1 `sudo tcpdump -tnv -i enp0s8`

![r1 dump](misc/part_5/29.png)

На ws11 `traceroute 10.20.0.10`

![ws11 traceroute 10.20.0.10](misc/part_5/30.png)

### 5.6. Использование протокола ICMP при маршрутизации

Запускаю на r1 перехват сетевого трафика `tcpdump -n -i enp0s8 icmp`

![r1 dump](misc/part_5/31.png)

Пингую с ws11 несуществующий IP `ping -c 1 10.30.0.111`

![ws11 ping](misc/part_5/32.png)

## Часть 6. Динамическая настройка IP с помощью DHCP

#### Для r2 настраиваю конфигурацию службы DHCP:

Устанавливаю утилиту `sudo apt-get install isc-dhcp-server`

Открываю конфиг и добавляю изменения в r2 `sudo nano  /etc/dhcp/dhcpd.conf`

![dhcpd.conf](misc/part_6/1.png)

В файл resolv.conf прописываю nameserver 8.8.8.8

![resolv.conf](misc/part_6/2.png)

Перезагружаю службу DHCP `systemctl restart isc-dhcp-server` и проверяю ее статус `systemctl status isc-dhcp-server`

![r2 dhcp status](misc/part_6/3.png)

Обновляю настройки в ws21 и ws22

![ws21 /etc/netplan/00-installer-config.yaml](misc/part_6/4.png)

![ws22 /etc/netplan/00-installer-config.yaml](misc/part_6/5.png)

Применяю настройки на обеих машинах `sudo netplan apply`

Перезагружаю ws21 `sudo reboot`

Проверяю, полученный от DHCP адрес

ws21 `ip a`

![ws21 ip a](misc/part_6/6.png)

ws22 `ip a`

![ws22 ip a](misc/part_6/7.png)

Пингую ws22 с ws21  `ping -c 4 10.20.0.5`

![ws22 ip a](misc/part_6/8.png)

Указываю MAC адрес у ws21

В настройках VB

![ws11 mac](misc/part_6/9.png)

В конфигурационном файле

![ws11 yaml mac](misc/part_6/10.png)

#### Для r1 настраиваю аналогично r2, но делаю выдачу адресов с жесткой привязкой к MAC-адресу (ws11)

Устанавливаю утилиту `sudo apt-get install isc-dhcp-server`

Открываю конфиг и добавляю изменения в r1 `sudo nano  /etc/dhcp/dhcpd.conf`

![dhcpd.conf](misc/part_6/11.png)

В файл resolv.conf прописываю nameserver 8.8.8.8

![resolv.conf](misc/part_6/2.png)

Перезагружаю службу DHCP `systemctl restart isc-dhcp-server` и проверяю ее статус `systemctl status isc-dhcp-server`

![r2 dhcp status](misc/part_6/12.png)

Обновляю настройки в ws11

![ws11 /etc/netplan/00-installer-config.yaml](misc/part_6/13.png)

Применяю настройки `sudo netplan apply`

ws11 `ip a`

![ws11 ip a](misc/part_6/14.png)

Пингую ws11 с ws22  `ping -c 4 10.10.0.2`

![ws11 ping](misc/part_6/15.png)

#### Запрашиваю с ws21 обновление ip адреса

ip на ws21 до запроса нового `ip a`

![ws11 ip a](misc/part_6/16.png)

Запрос нового ip `sudo dhclient -v`

> -v - получение нового ip

ws11 `ip a`

![ws11 ip a](misc/part_6/17.png)

## Часть 7. NAT

Устанавливаю apache `sudo apt install apache2`

В файле /etc/apache2/ports.conf на ws22 и r1 меняю строку Listen 80 на Listen 0.0.0.0:80

![ports.conf](misc/part_7/1.png)

Запускаю веб-сервер на ws22 и r1 `service apache2 start` и проверяю `systemctl status apache2`

![start apache 2](misc/part_7/2.png)

![start apache 2](misc/part_7/3.png)

Создаю файл /etc/firewall.sh, имитирующий фаервол на r2 `sudo nano /etc/firewall.sh`

![r2 /etc/firewall.sh](misc/part_7/4.png)

Запускаю файл `sudo chmod +x /etc/firewall.sh` и `sudo /etc/firewall.sh`

Пингую ws22 с r1 

![ping ws22 from r1](misc/part_7/5.png)

Добавляю в фаервол еще одно правило

![r2 /etc/firewall.sh](misc/part_7/6.png)

Пингую ws22 с r1 

![ping ws22 from r1](misc/part_7/7.png)

Добавляю еще правила в фаервол

![r2 /etc/firewall.sh](misc/part_7/8.png)

>- t - указывает на используемую таблицу
>- p - указывает протокол, такие как tcp, udp, udplite и другие, поддерживаемые системой, ознакомиться со списком можно в файле /etc/protocols
>- m - подключает указанный модуль
>- s - указывает адрес источника пакета, в качестве значения можно указать как один IP-адрес, так и диапазон;
>- i - задает входящий сетевой интерфейс
>- o - указывает исходящий сетевой интерфейс
>- --dport - порт получателя пакета
>- DNAT — подменяет адрес получателя в заголовке - IP-пакета, основное применение — предоставление доступа к сервисам снаружи, находящимся внутри сети
>- SNAT — служит для преобразования сетевых адресов, применимо, когда за сервером находятся машины, которым необходимо предоставить доступ в Интернет, при этом от провайдера имеется статический IP-адрес

Запускаю `sudo /etc/firewall.sh`

Проверяю соединение по TCP для SNAT, для этого с ws22 подключаюсь к серверу Apache на r1 `sudo telnet 10.100.0.11 80`

![telnet](misc/part_7/9.png)

## Часть 8. Допополнительно. Знакомство с SSH Tunnels

Устанавливаю apache на ws22 `sudo apt install apache2`

В файле /etc/apache2/ports.conf меняю строку Listen 80 на Listen localhost:80

![ports.conf](misc/part_8/1.png)

Запускаю веб-сервер на ws22  `service apache2 start` и проверяю `systemctl status apache2`

![start apache 2](misc/part_8/2.png)

Запускаю фаервол на r2 `sudo /etc/firewall.sh`

![firewall r2](misc/part_8/3.png)

Local TCP forwarding с ws21 до ws22

![Local TCP](misc/part_8/4.png)

![telenet ](misc/part_8/5.png)

Remote TCP forwarding c ws11 до ws22

![Local TCP](misc/part_8/6.png)

![telenet ](misc/part_8/7.png)

[Оглавление](#оглавление)