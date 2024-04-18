## Часть 1. Инструмент ipcalc ##
  ### 1.1.1 Сети и маски ###
  Адрес сети 192.167.38.54/13 <br>
  ![Alt-текст](pics/ipcalcaddr.png) <br>
  ### 1.2.1 Перевод масок ###
  Перевод маски 255.255.255.0 в префиксную и двоичную запись <br>
  ![Alt-текст](pics/maska.png) <br>
  /15 в обычную и двоичную <br>
  ![Alt-текст](pics/maska1.png) <br>
  11111111.11111111.11111111.11110000 в обычную и префиксную <br>
  ![Alt-текст](pics/maska2.png) <br>
  ### 1.3.1 Максимальный и минимальный хосты ###
  Минимальный и максимальный хост в сети 12.167.38.4 при маскe /8 <br>
  ![Alt-текст](pics/maxminh.png) <br>
  При маскe 11111111.11111111.00000000.00000000 <br>
  ![Alt-текст](pics/maxminh1.png) <br>
  При маске 255.255.254.0 <br>
  ![Alt-текст](pics/maxminh2.png) <br>
  При маске /4 <br>
  ![Alt-текст](pics/maxminh3.png) <br>
  ### 1.2.1 localhost ###
  ```Зарезервированные адреса для localhost относятся к диапазону 127.0.0.0 с маской подсети /8``` <br>
  Исходя из этого:<br>
  194.34.23.100 - нельзя обратиться, адрес общедоступный <br>
  127.0.0.2 - можно обратиться, входит в диапазон <br>
  127.1.0.1 - можно обратиться, входит в диапазон<br>
  128.0.0.1 - нельзя обратиться, не входит в диапазон<br>
  ### 1.3.1 Диапазоны и сегменты сетей ###
  ```Диапазоны частных адресов:``` <br>
  ```10.0.0.0 - 10.255.255.255``` <br>
  ```172.16.0.0 - 172.31.255.255```<br>
  ```192.168.0.0 - 192.168.255.255```<br>
  Исходя из этого: <br>
  ```Частные:``` <br>
  - 172.20.250.4 <br>
  - 10.0.0.45 <br>
  - 192.168.4.2 <br>
  - 172.16.255.255 <br>
  - 10.10.10.10 <br>

  ```Публичные:```<br>
  - 134.43.0.2 
  - 172.0.2.1
  - 192.172.0.1
  - 172.68.0.2
  - 192.169.168.1
  ### 1.3.2 Возможные адреса шлюза IP адресов сети  10.10.0.0/18
  ```Диапазон сети 10.10.0.0/18: 10.10.0.1 - 10.10.63.254```<br>
  Возможны: 10.10.0.2, 10.10.10.10, 10.10.1.255 <br>
  Невозможны: 10.0.0.1, 10.10.100.1 <br>

## Часть 2. Статическая маршрутизация между двумя машинами ##
  ![Alt-текст](pics/ws1ipa.png) <br>
  ![Alt-текст](pics/ws2ipa.png) <br>
  Меняем конфиг: <br>
  ![Alt-текст](pics/ws1netplan.png) <br>
  ![Alt-текст](pics/ws2netplan.png) <br>
  Далее нужно изменить тип подключения на "Сетевой мост" и ребутнуть виртуалки. <br>
  ```sudo netplan apply``` <br>
  ![Alt-текст](pics/ws1netplanapply.png) <br>
  ![Alt-текст](pics/ws2netplanapply.png) <br>
  ### 2.1 Добавление статического маршрута вручную ###
  ![Alt-текст](pics/ws1ipradd.png) <br>
  ![Alt-текст](pics/ws2ipradd.png) <br>
  ![Alt-текст](pics/ws1ping.png) <br>
  ![Alt-текст](pics/ws2ping.png) <br>
  ### 2.2 Добавление статического маршрута с сохранением ###
  Меняем конфиг: <br>
  ![Alt-текст](pics/ws1configch.png) <br>
  ![Alt-текст](pics/ws2configch.png) <br>
  Пингуем: <br>
  ![Alt-текст](pics/ws1ping2.png) <br>
  ![Alt-текст](pics/ws2ping2.png) <br>

## Часть 3. Утилита iperf3 ##
  ### 3.1 Скорость соединения ###
   ```8Mbps = 8 / 8 = 1Mbs```<br>
   ```10Mbs = 8 * 1024 * 100 = 819200 Kbps```<br>
   ```1Gbps = 1 * 1024 = 1024Mbps```<br>
  ### 3.2. Утилита iperf3 ###
  Перевести виртуалки опять в NAT, проверить ping -c 2 8.8.8.8 и дальше  <br>
  ```sudo apt update```<br>
  ```sudo apt install iperf```<br>
  Ребутнуть виртуалки <br>
  На первой машине не работало после перевода в NAT, пришлось менять конфиг DNS:<br>
  ```sudo nano /etc/resolv.conf```<br>
  Изменить значение namserver на 8.8.8.8<br>
  ![Alt-текст](pics/ws1iperf.png) <br>
  ![Alt-текст](pics/ws2iperf.png) <br>

## Часть 4. Сетевой экран ##
  ### 4.1 Утилита iptables ###
  ```/etc/firewall.sh ws1``` <br>
  ![Alt-текст](pics/ws1iptables.png) <br>
  ```/etc/firewall.sh ws1``` <br>
  ![Alt-текст](pics/ws2iptables.png) <br>
  Запуск файлов: <br>
  ![Alt-текст](pics/ws1drop.png) <br>
  ![Alt-текст](pics/ws2accept.png) <br>
  Разница заключается в том, что на первой виртуалке у нас запрещающее правило стоит раньше разрешающего, на второй наоборот и следовательно первая виртуалка не будет пинговаться. <br>
  ![Alt-текст](pics/ws1ping3.png) <br>
  ![Alt-текст](pics/ws2ping3.png) <br>
  ![Alt-текст](pics/hostisup.png) <br>

## Часть 5. Статическая маршрутизация сети ##
  ### Часть 5.1 Настройка адресов машин ###
  ```ws11config```<br>
  ![Alt-текст](pics/ws11config.png) <br>
  ```ws21config``` <br>
  ![Alt-текст](pics/ws21config.png) <br>
  ```ws22config``` <br>
  ![Alt-текст](pics/ws22config.png) <br>
  ```r1config``` <br>
  ![Alt-текст](pics/r1config.png) <br>
  ```r2config``` <br>
  ![Alt-текст](pics/r2config.png) <br>
  ip -4 a для каждой машины: <br>
  ```ws11``` <br>
  ![Alt-текст](pics/ip-4aws11.png) <br>
  ```ws21``` <br>
  ![Alt-текст](pics/ip-4aws21.png) <br>
  ```ws22``` <br>
  ![Alt-текст](pics/ip-4aws22.png) <br>
  ```r1``` <br>
  ![Alt-текст](pics/ip-4ar1.png) <br>
  ```r2``` <br>
  ![Alt-текст](pics/ip-4ar2.png) <br>
  Пингуем r1 с ws11: <br>
  ![Alt-текст](pics/r1pingws11.png) <br>
  Пингуем ws22 с ws21: <br>
  ![Alt-текст](pics/ws22pingws21.png) <br>
  ### 5.2 Включение переадресации IP-адресов ###
  ![Alt-текст](pics/r1komanda.png) <br>
  ![Alt-текст](pics/r2komanda.png) <br>
  ![Alt-текст](pics/r1sysctl.png) <br>
  ![Alt-текст](pics/r2sysctl.png) <br>
  ### 5.3 Установка маршрута по умолчанию ###
  ```ws11``` <br>
  ![Alt-текст](pics/ws11routes.png) <br>
  ```ws21``` <br>
  ![Alt-текст](pics/ws21routes.png) <br>
  ```ws22``` <br>
  ![Alt-текст](pics/ws22routes.png) <br>
  ```ws11 ip r``` <br>
  ![Alt-текст](pics/ws11ipr.png) <br>
  ```ws21 ip r``` <br>
  ![Alt-текст](pics/ws21ipr.png) <br>
  ```ws22 ip r```<br>
  ![Alt-текст](pics/ws22ipr.png) <br>
  ```tcpdump``` <br>
  ![Alt-текст](pics/tcpdump.png) <br>
  ping r2 <br>
  ![Alt-текст](pics/ws11pingr2.png) <br>
  ### 5.4 Добавление статических маршрутов ###
  ![Alt-текст](pics/r1netplanroutes.png) <br>
  ![Alt-текст](pics/r2netplanroutes.png) <br>
  ![Alt-текст](pics/r1ipr.png) <br>
  ![Alt-текст](pics/r2ipr.png) <br>
  ![Alt-текст](pics/ipr.png) <br>
  Маршруты различаются, потому что при маршрутизации в первом случае маршрут задан явным образом в конфигурационном файле. <br>
  ### 5.5. Построение списка маршрутизаторов ###
  ![Alt-текст](pics/traceroute.png) <br>
  ![Alt-текст](pics/tcmdumpik.png) <br>
  Traceroute предоставляет информацию о пути пакета данных из одной точки сети на конкретный IP-сервер. Когда данные передаются между двумя точками, они должны проходить через маршрутизаторы.
  Traceroute сопоставляет каждый переход, предоставляет подробную информацию и время приёма-передачи (RTT), а также, по возможности, сообщает имя устройства и IP-адрес. <br>
  ### 5.6. Использование протокола ICMP при маршрутизации ###
  ![Alt-текст](pics/tcpdump56.png) <br>
  ![Alt-текст](pics/pingnowhere.png) <br>

## Часть 6. Динамическая настройка IP с помощью DHCP ##
  dhcp conf r2 <br>
  ![Alt-текст](pics/dhcpconf.png) <br>
  resolv.conf r2
  ![Alt-текст](pics/resolvconf.png) <br>
  restart dhcp <br>
  ![Alt-текст](pics/restartdhcp.png) <br>
  ip a ws22 <br>
  ![Alt-текст](pics/ipa6.png) <br>
  ws22 ping ws21 <br>
  ![Alt-текст](pics/ws22pingws216.png) <br>
  netplan macaddress add <br>
  ![Alt-текст](pics/macaddress.png) <br>
  dhcp.conf r1 <br>
  ![Alt-текст](pics/dhcpconfr1.png) <br>
  ```sudo restart isc-dhcp-server```
  ![Alt-текст](pics/ws11ipaposle.png) <br>
  На ws 21: до обновления <br>
  ![Alt-текст](pics/ipado.png) <br>
  ```sudo dhclient enp0s8``` <br>
  На ws 21: после обновления <br>
  ![Alt-текст](pics/ipaposle.png) <br>

## Часть 7. NAT ##
  ```sudo apt install apache2``` <br>
  r1 /etc/apache2/ports.conf <br>
  ![Alt-текст](pics/r1apache.png) <br>
  ws22 /etc/apache2/ports.conf <br>
  ![Alt-текст](pics/ws22apache.png) <br>
  r1 start apache <br>
  ![Alt-текст](pics/r1startapache.png) <br>
  ws22 start apache <br>
  ![Alt-текст](pics/ws22startapache.png) <br>
  меняем firewall на r2: <br>
  ![Alt-текст](pics/firewallr2.png) <br>
  ping ws22 с r1: <br>
  ![Alt-текст](pics/pingnogo.png) <br>
  меняем фаервол: <br>
  ![Alt-текст](pics/firewallr2icmp.png) <br>
  пинга идет <br>
  ![Alt-текст](pics/pingaidet.png) <br>
  добавил SNAT и DNAT <br>
  ![Alt-текст](pics/snatdnat.png) <br>
  telnet c ws22: <br>
  ![Alt-текст](pics/telnetws22.png) <br>
  telnet c r1: <br>
  ![Alt-текст](pics/telnetr1.png) <br>

## Часть 8. Знакомство с SSH tunnels ##
  запуск фаервола <br>
  ![Alt-текст](pics/firewallzapusk.png) <br>
  ![Alt-текст](pics/apachi.png) <br>
  запуск апачи: <br>
  ```sudo service apache2 start``` <br>
  local tcp: <br>
  ![Alt-текст](pics/sshl.png) <br>
  ![Alt-текст](pics/telnetsshl.png) <br>
  remote tcp: <br>
  ![Alt-текст](pics/sshr.png) <br>
  ![Alt-текст](pics/telnetsshr.png) <br>