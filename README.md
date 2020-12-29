# JARKOM_Modul5_Lapres_T07

Lapres Praktikum Jarkom Modul 5<br />
<br />
![kelompok](https://img.shields.io/badge/Kelompok-T07-00a69a)<br />
<br />
Anggota:<br />
- ![fikri](https://img.shields.io/badge/Fikri%20Haykal-05311840000006-blueviolet)<br />
- ![syarif](https://img.shields.io/badge/Fancista%20Syarif%20H.-05311840000027-blueviolet)<br />

## Subnetting (VLSM)
  Menghitung IP yang dibutuhkan serta memberi label `AX` untuk setiap netmask
  
  ![img](https://github.com/fikrihaykal/JARKOM_Modul5_Lapres_T07/blob/main/img/Topologi%20VLSM.jpg?raw=true)<br /><br />
  
  <table>
    <tr>
      <th>Label</th>
      <th>Jumlah IP</th>
      <th>Netmask</th>
    </tr>
    <tr>
      <td>A1</td>
      <td>201</td>
      <td>/24</td>
    </tr>
    <tr>
      <td>A2</td>
      <td>2</td>
      <td>/30</td>
    </tr>
    <tr>
      <td>A3</td>
      <td>2</td>
      <td>/30</td>
    </tr>
    <tr>
      <td>A4</td>
      <td>211</td>
      <td>/24</td>
    </tr>
    <tr>
      <td>A5</td>
      <td>3</td>
      <td>/29</td>
    </tr>
  </table>
  
  Menghitung pembagian IP berdasarkan NID dan Netmask
  ![img](https://github.com/fikrihaykal/JARKOM_Modul5_Lapres_T07/blob/main/img/VLSM.png?raw=true)<br /><br />

## Topologi
  Membuat file <b>topologi.sh</b> sesuai rancangan topologi
```
# Switch
uml_switch -unix switch0 > /dev/null < /dev/null &
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.74.69 eth1=daemon,,,switch0 eth2=daemon,,,switch3 mem=96M &
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch0 eth1=daemon,,,switch2 eth2=daemon,,,switch4 mem=96M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch3 eth1=daemon,,,switch1 eth2=daemon,,,switch5 mem=96M &

# DNS + Web Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch1 mem=128M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,switch1 mem=128M &

# Client
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch4 mem=96M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch5 mem=96M &
```

## Setting IP
  Mengatur IP tiap UML dengan perintah `nano /etc/network/interfaces`.
  
### Surabaya
```
auto eth0
iface eth0 inet static
address 10.151.74.70
netmask 255.255.255.252
gateway 10.151.74.69

auto eth1
iface eth1 inet static
address 192.168.0.1
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.0.5
netmask 255.255.255.252
```

### Batu
```
auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.252
gateway 192.168.0.1

auto eth1
iface eth1 inet static
address 10.151.83.137
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.2.1
netmask 255.255.255.0
```

### Kediri
```
auto eth0
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

auto eth1
iface eth1 inet static
address 192.168.0.9
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.3.1
netmask 255.255.255.0
```

### Malang
```
auto eth0
iface eth0 inet static
address 10.151.83.138
netmask 255.255.255.248
gateway 10.151.83.137
```

### Mojokerto
```
auto eth0
iface eth0 inet static
address 10.151.83.139
netmask 255.255.255.248
gateway 10.151.83.137
```

### Sidoarjo
```
auto eth0
iface eth0 inet dhcp
```

### Gresik
```
auto eth0
iface eth0 inet dhcp
```

### Probolinggo
```
auto eth0
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.248
gateway 192.168.0.9
```

### Madiun
```
auto eth0
iface eth0 inet static
address 192.168.0.11
netmask 255.255.255.248
gateway 192.168.0.9
```

## Routing
  Mengatur Routing tiap router dengan membuat file <b>routing.sh</b> berisi:
  
### Surabaya
```
route add -net 10.151.83.136 netmask 255.255.255.248 gw 192.168.0.2
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.0.2
route add -net 192.168.3.0 netmask 255.255.255.0 gw 192.168.0.6
route add -net 192.168.0.8 netmask 255.255.255.248 gw 192.168.0.6
```

### Batu
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.1
```

### Kediri
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.168.0.5
```

## Setting DHCP Server dan Relay
### Mojokerto
<ol>
  <li>Menginstall <b>DHCP Server</b> dengan perintah `apt-get install isc-dhcp-server`</li>
  <li>Mengatur <b>INTERFACES</b> menjadi `eth0`</li>
  <li>Deklarasi subnet B1<br />
    ```
    subnet 10.151.83.136 netmask 255.255.255.248 {
    }
    ```
  </li>
  <li>Deklarasi subnet A1<br />
    ```
    subnet 192.168.2.0 netmask 255.255.255.0 {
      range 192.168.2.2 192.168.2.201;
      option routers 192.168.2.1;
      option broadcast-address 192.168.2.255;
      option domain-name-servers 10.151.83.139, 202.46.129.2, 10.151.36.7;
      default-lease-time 600;
      max-lease-time 7200;
    }
    ```
  </li>
  <li>Deklarasi subnet A4<br />
    ```
    subnet 192.168.3.0 netmask 255.255.255.0 {
      range 192.168.3.2 192.168.2.211;
      option routers 192.168.3.1;
      option broadcast-address 192.168.3.255;
      option domain-name-servers 10.151.83.139, 202.46.129.2, 10.151.36.7;
      default-lease-time 600;
      max-lease-time 7200;
    }
    ```
  </li>
</ol>

### Batu
<ol>
  <li>Menginstall <b>DHCP Relay</b> dengan perintah `apt-get install isc-dhcp-relay`</li>
  <li>Mengatur <b>SERVERS</b> menjadi `10.151.83.138`</li>
  <li>Mengatur <b>INTERFACES</b> menjadi `eth0 eth1 eth2`</li>
</ol>

### Surabaya
<ol>
  <li>Menginstall <b>DHCP Relay</b> dengan perintah `apt-get install isc-dhcp-relay`</li>
  <li>Mengatur <b>SERVERS</b> menjadi `10.151.83.138`</li>
  <li>Mengatur <b>INTERFACES</b> menjadi `eth1 eth2`</li>
</ol>

### Kediri
<ol>
  <li>Menginstall <b>DHCP Relay</b> dengan perintah `apt-get install isc-dhcp-relay`</li>
  <li>Mengatur <b>SERVERS</b> menjadi `10.151.83.138`</li>
  <li>Mengatur <b>INTERFACES</b> menjadi `eth0 eth2`</li>
</ol>


## SOAL
### 1. Konfigurasi Akses Keluar
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE.

<b><i>Penyelesaian :</i></b><br />
Membuat soal1.sh di SURABAYA
```
iptables -t nat -A POSTROUTING -s 192.168.0.0/22 -o eth0 -j SNAT --to-source 10.151.74.70
```

### 2. Drop Akses SSH dari Luar Topologi pada SURABAYA
Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.

<b><i>Penyelesaian :</i></b><br />
Membuat soal2.sh di SURABAYA
```
iptables -A FORWARD -d 10.151.83.136/29 -i eth0 -p tcp -m tcp --dport 22 -j DROP
```

### 3. Membatasi Maksimal 3 Koneksi ICMP Secara Bersamaan
Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.

<b><i>Penyelesaian :</i></b><br />
Membuat soal3.sh di MALANG dan MOJOKERTO
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

### 4. Membatasi Waktu Akses di Subnet SIDOARJO
Kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET
SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut:
1. Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat.

<b><i>Penyelesaian :</i></b><br />
Membuat soal4.sh di MALANG
```
iptables -A INPUT -s 192.168.2.0/24 -m time --weekdays Sat,Sun -j REJECT
iptables -A INPUT -s 192.168.2.0/24 -m time --timestart 00:00 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
iptables -A INPUT -s 192.168.2.0/24 -m time --timestart 17:01 --timestop 23:59 --weekdays Mon,Tue,Wed,Thu,Fri -j REJECT
```

### 5. Membatasi Waktu Akses di Subnet GRESIK
Kemudian kalian diminta untuk membatasi akses ke MALANG yang berasal dari SUBNET
SIDOARJO dan SUBNET GRESIK dengan peraturan sebagai berikut:
2. Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. Selain itu paket akan di REJECT.

<b><i>Penyelesaian :</i></b><br />
Membuat soal5.sh di MALANG
```
iptables -A INPUT -s 192.168.3.0/24 -m time --timestart 07:01 --timestop 16:59 -j REJECT
```

### 6. Mengatur Distribusi Client yang Mengakses DNS Server
Karena kita memiliki 2 buah WEB Server, Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.

<b><i>Penyelesaian :</i></b><br />
Membuat soal6.sh di SURABAYA
```
iptables -A PREROUTING -t nat -p tcp -d 10.151.83.138 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.0.11:80
iptables -A PREROUTING -t nat -p tcp -d 10.151.83.138 -j DNAT --to-destination 192.168.0.10:80
```

### 7.
Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.

<b><i>Penyelesaian :</i></b><br />
Membuat soal7.sh di SURABAYA
```
iptables -N LOGGING 
iptables -A FORWARD -d 10.151.83.136/29 -i eth0 -p tcp -m tcp --dport 22 -j LOGGING 
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```

Membuat soal7.sh di MALANG dan MOJOKERTO
```
iptables -N LOGGING 
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j LOGGING
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```

