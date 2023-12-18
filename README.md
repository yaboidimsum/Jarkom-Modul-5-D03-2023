# Praktikum Modul 4 Jaringan Komputer

Perkenalkan kami dari kelas `Jaringan Komputer D Kelompok D03`, dengan anggota sebagai berikut:

## Author
| Nama | NRP | Github |
|---------------------------|------------|--------|
|Alfan Lukeyan Rizki | 5025211046 | https://github.com/AlfanLukeyan |
|Dimas Prihady Setyawan| 5025211184 | https://github.com/yaboidimsum |

# Laporan Resmi Praktikum Modul 5
Pada kesempatan kali ini kami akan membahas mengenai soal praktikum modul 5. Soal praktikum modul 5 memuat konsep mengenai
- `VLSM` menggunakan `GNS 3`
- `Firewall` menggunakan `iptables` 

## Daftar Isi
- [Laporan Resmi](#laporan-resmi)
- [Daftar Isi](#daftar-isi)
   - [Topologi VLSM](#topologi-vlsm)
   - [Topologi GNS3](#topologi-gns3)
   - [Prefix IP](#prefix-ip)
   - [Rute](#rute)
- [Persiapan](#persiapan)
   - [Tree](#tree)
   - [Pembagian IP](#pembagian-ip)
   - [Subnetting](#subnetting)
   - [Routing](#routing)
- [Konfigurasi](#konfigurasi)
   - [DNS Server](#dns-server)
   - [DHCP Server](#dhcp-server)
   - [DHCP Relay](#dhcp-relay)
   - [Web Server](#web-server)
   - [Client](#client)

## Topologi VLSM 
![image](img/topologi-vlsm.png)


## Topologi GNS3
![image](img/topologi-gns3.png)

## Prefix IP
Prefix IP yang dimiliki oleh kelompok D03 adalah `10.23.x.x`

## Rute
Berikut adalah Rute yang telah kami buat dari hasil [Topologi VLSM](#topologi-vlsm) sebagai berikut 
![image](img/rute.png)

## Persiapan 

### Tree
Tree yang kami buat didapatkan dari hasil pengelompokkan [Topologi VLSM](#topologi-vlsm) sebagai berikut 

![tree](img/tree.png)

### Pembagian IP
Berikut adalah pembagian IP yang telah kami dapatkan dari hasil [Tree](#tree) tersebut

![pembagian-ip](img/pembagian-ip.png)

### Subnetting 
Berikut merupakan subneeting yang telah kami sesuaikan dengan ``IP`` yang telah didapatkan.

#### Aura
```
auto eth0
iface eth0 inet dhcp

# Aura-Heiter
auto eth1
iface eth1 inet static
	address 10.23.0.1
	netmask 255.255.255.252

# Aura-Frieren
auto eth2
iface eth2 inet static
	address 10.23.0.5
	netmask 255.255.255.252

```

#### Heiter 
```
#Heiter-Aura
auto eth0
iface eth0 inet static
	address 10.23.0.2
	netmask 255.255.255.252
	gateway 10.23.0.1

#Heiter-TurkRegion
auto eth1
iface eth1 inet static
	address 10.23.8.1
	netmask 255.255.248.0

#Heiter-Sein-GrobeForest
auto eth2
iface eth2 inet static
	address 10.23.4.1
	netmask 255.255.252.0

```

#### Frieren 
```
#Frieren-Aura
auto eth0
iface eth0 inet static
	address 10.23.0.6
	netmask 255.255.255.252
	gateway 10.23.0.5

#Frieren-Stark
auto eth1
iface eth1 inet static
	address 10.23.0.9
	netmask 255.255.255.252

#Frieren-Himmel
auto eth2
iface eth2 inet static
	address 10.23.0.13
	netmask 255.255.255.252
```

#### Himmel 

```
#Himmel-Frieren
auto eth0
iface eth0 inet static
	address 10.23.0.14
	netmask 255.255.255.252
	gateway 10.23.0.13

#Himmel-LaubHills
auto eth1
iface eth1 inet static
	address 10.23.2.1
	netmask 255.255.254.0

#Himmel-SchwerMountain-Fern
auto eth2
iface eth2 inet static
	address 10.23.1.1
	netmask 255.255.255.128
```

#### Fern 
```
#Fern-Himmel
auto eth0
iface eth0 inet static
	address 10.23.1.3
	netmask 255.255.255.128
	gateway 10.23.1.1

#Fern-Richter
auto eth1
iface eth1 inet static
	address 10.23.0.17
	netmask 255.255.255.252

#Fern-Revolte
auto eth2
iface eth2 inet static
	address 10.23.0.21
	netmask 255.255.255.252
```

#### Revolte
```
auto eth0
iface eth0 inet static
	address 10.23.0.22
	netmask 255.255.255.252
	gateway 10.23.0.21
```

#### Richter
```
auto eth0
iface eth0 inet static
	address 10.23.0.18
	netmask 255.255.255.252
	gateway 10.23.0.17
```

#### Stark
```
auto eth0
iface eth0 inet static
	address 10.23.0.10
	netmask 255.255.255.252
	gateway 10.23.0.9
```

#### Sein 
```
auto eth0
iface eth0 inet static
	address 10.23.4.2
	netmask 255.255.252.0
	gateway 10.23.4.1
```

#### Client 
```
auto eth0
iface eth0 inet dhcp
```

### Routing 
Setelah melakukan subnetting pada setiap ``node``. Sekarang kami akan beralih pada setup ``routing`` sebagai berikut 

#### Aura 
```
up route add -net 10.23.4.0 netmask 255.255.252.0 gw 10.23.0.2
up route add -net 10.23.8.0 netmask 255.255.248.0 gw 10.23.0.2
up route add -net 10.23.0.8 netmask 255.255.255.252 gw 10.23.0.6
up route add -net 10.23.0.12 netmask 255.255.255.252 gw 10.23.0.6
up route add -net 10.23.2.0 netmask 255.255.254.0 gw 10.23.0.6
up route add -net 10.23.1.0 netmask 255.255.255.128 gw 10.23.0.6
up route add -net 10.23.0.16 netmask 255.255.255.252 gw 10.23.0.6
up route add -net 10.23.0.20 netmask 255.255.255.252 gw 10.23.0.6
```

#### Heiter 
```
up route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.23.0.1
```

#### Frieren 
```
up route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.23.0.5
up route add -net 10.23.2.0 netmask 255.255.254.0 gw 10.23.0.14
up route add -net 10.23.1.0 netmask 255.255.255.128 gw 10.23.0.14
up route add -net 10.23.0.16 netmask 255.255.255.252 gw 10.23.0.14
up route add -net 10.23.0.20 netmask 255.255.255.252 gw 10.23.0.14
```

#### Himmel 
```
up route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.23.0.13
up route add -net 10.23.0.16 netmask 255.255.255.252 gw 10.23.1.3
up route add -net 10.23.0.20 netmask 255.255.255.252 gw 10.23.1.3
```

#### Fern 
```
up route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.23.1.1
```

## Konfigurasi 
Sebelum melakukan konfigurasi terhadap masing-masing ``router`` yang telah ditentukan, disini kita harus menjalankan perintah berikut pada router ``Aura`` 

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.23.0.0/16
```

Setelah itu, jangan lupa untuk tiap node agar di berikan ``nameserver 192.168.122.1`` agar dapat terhubung dengan internet

### DNS Server
Disini yang bertindak sebagai ``DNS Server`` adalah router ``Richter`` dan akan dilakukan konfigurasi sebagai berikut dengan bantuan ``bash`` nantinya 

```sh
# Richter
echo 'nameserver 192.168.122.1' >/etc/resolv.conf

apt-get update
apt-get install bind9 -y
cp ~/named.conf.options /etc/bind/

echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

### DHCP Server
Setelah berhasil melakukan setup untuk ``DNS Server``. Sekarang kami berpindah untuk melakukan beberapa konfigurasi yang dibutuhkan pada ``DHCP Server`` sebagai berikut

```sh
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt install isc-dhcp-server -y
echo '
INTERFACESv4="eth0"
INTERFACESv6=
' > /etc/default/isc-dhcp-server

echo '
#A1
subnet 10.23.4.0 netmask 255.255.252.0 {
    range 10.23.4.2 10.23.7.254;
    option routers 10.23.4.1;
    option broadcast-address 10.23.7.255;
    option domain-name-servers 10.23.0.18;
    default-lease-time 180;
    max-lease-time 5760;
}

#A2
subnet 10.23.8.0 netmask 255.255.248.0 {
    range 10.23.8.2 10.23.15.254;
    option routers 10.23.8.1;
    option broadcast-address 10.23.15.255;
    option domain-name-servers 10.23.0.18;
    default-lease-time 180;
    max-lease-time 5760;
}

#A3
subnet 10.23.0.0 netmask 255.255.255.252 {
}

#A4
subnet 10.23.0.4 netmask 255.255.255.252 {

}

#A5
subnet 10.23.0.8 netmask 255.255.255.252 {
}

#A6
subnet 10.23.0.12 netmask 255.255.255.252 {
}

#A7
subnet 10.23.2.0 netmask 255.255.254.0 {
    range 10.23.2.2 10.23.3.254;
    option routers 10.23.2.1;
    option broadcast-address 10.23.3.255;
    option domain-name-servers 10.23.0.18;
    default-lease-time 180;
    max-lease-time 5760;
}

#A8
subnet 10.23.1.0 netmask 255.255.255.128 {
    range 10.23.1.2 10.23.1.126;
    option routers 10.23.1.1;
    option broadcast-address 10.23.1.127;
    option domain-name-servers 10.23.0.18;
    default-lease-time 180;
    max-lease-time 5760;
}

#A9
subnet 10.23.0.16 netmask 255.255.255.252 {
}

#A10
subnet 10.23.0.20 netmask 255.255.255.252 {
}
' > /etc/dhcp/dhcpd.conf

rm /run/dhcpd.pid
service isc-dhcp-server restart
```

Disini terlihat bahwa ``kami`` juga melakukan setup pada masing-masing ``subnet`` yang nantinya akan digunakan oleh masing-masing client ``dhcp client``.

### DHCP Relay
Untuk ``DHCP Relay`` kita perlu melakukan pertimbangan terlebih dahulu, karena konsep pada ``DHCP Relay`` adalah *DHCP Relay bertindak sebagai perantara antara klien dan server DHCP. Ketika klien mengirim permintaan DHCP, relay menangkap pesan tersebut dan mengirimkannya ke server DHCP melalui unicast (tidak broadcast).*

Sehingga disini kita perlu untuk melakukan ``konfigurasi`` pada router yang berdekatan dengan ``client`` yang akan diberikan ``IP`` oleh DHCP. Sehingga disini kami memberikan ``DHCP Relay`` pada router ``Heiter`` dan ``Himmel``. Karena ``Heiter`` berdekatan dengan client **TurkRegion** dan **GrobeForest**, sedangkan ``Himmel`` berdekatan dengan **LaubHills** dan **SchwerMountain**. Konfigurasinya adalah sebagai berikut

```sh
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

echo '
SERVERS="10.23.0.22"
INTERFACES="eth0 eth1 eth2"
OPTIONS=
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

Lalu jangan lupa untuk melakukan uncomment ``net.ipv4.ip_forward=1`` pada ``/etc/sysctl.conf`` 

### Web Server
Pada ``web server`` kami akan menggunakan ``apache2`` dan akan dikonfigurasikan untuk router ``Sein`` dan ``Stark`` sebagai berikut 

```sh
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt update
apt install netcat -y
apt install apache2 -y
service apache2 start

echo '# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80
Listen 443

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet' > /etc/apache2/ports.conf
```

Lalu pada ``/var/www/html/index.html`` di masing-masing node ``Sein`` ataupun ``Start`` tambahkan berikut:
```
echo '# Sein | Stark
Sein | Stark nih' > /var/www/html/index.html
```

### Client 
Untuk masing-masing client, kita hanya perlu install ``lynx`` karena akan digunakan sebagai testing nantinya

```sh
apt update
apt install netcat -y
apt install lynx -y
```

# Jawaban 

Setelah berhasil melakukan [Konfigurasi](#konfigurasi) seperti diatas, sekarang jangan lupa untuk melakukan ``restart (stop lalu start lagi)`` pada router ``Aura`` karena akan digunakan pada [Soal 1](#soal-1)
 
## Soal 1
> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.

### Script 
Ada 2 opsi untuk mengerjakan ``soal`` ini. Yang pertama dari awal langsung dilakukan setup tanpa ``MASQUERADE`` yang kedua bisa langsung tanpa ``MASQUERADE``. Jika memilih opsi yang pertama, maka anda harus melakukan ``restart`` pada node ``Aura`` memastikan bahwa `iptables` sebelumnya sudah tidak ada.  Jika menggunakan cara yang kedua maka tidak perlu untuk melakukan ``restart`` pada node ``Aura``.

Dengan menggunakan cara yang kedua, dilakukan konfigurasi agar topologi dapat mengakses internet keluar (NAT). Tanpa MASQUERADE, konfigurasi dilakukan dengan memanfaatkan scripting sederhana, yaitu dengan menggunakan ``SNAT --to-source`` yang mengarah pada NID dari router yang berhubungan dengan NAT, yaitu **10.23.0.0/20**. Dengan kata lain, ``IP`` tesebut adalah IP terluar / terjauh yang mencakup seluruh ``IP`` yang kita peroleh sebelumnya.

Namun, sebelumnya perlu definisikan ``interface`` mana yang terkoneksi dengan NAT. Pada kasus ini adalah ``Aura``, interface yang berhubungan adalah ``eth0``. Definisi tersebut dapat dimasukkan ke dalam sebuah variabel. Di sini, digunakan variabel bernama ``IPETH0``.

```sh
# No 1 (Aura)
IPETH0="$(ip -br a | grep eth0 | awk '{print $NF}' | cut -d'/' -f1)"
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source "$IPETH0" -s 10.23.0.0/20
```

### Testing


## Soal 2
> Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.

### Script 


**Penjelasan**


### Testing

**Sukses**

**Gagal** 


## Soal 3
> Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop.

### Script 


**Penjelasan**


### Testing


## Soal 4
> Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.

### Script 

**Penjelasan**

### Testing

**GrobeForest**

**TurkRegion**


## Soal 5
> Selain itu, akses menuju WebServer hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.

### Script 

**Penjelasan**


### Testing
**Sukses**

**Gagal**

## Soal 6
> Lalu, karena ternyata terdapat beberapa waktu di mana network administrator dari WebServer tidak bisa stand by, sehingga perlu ditambahkan rule bahwa akses pada hari Senin - Kamis pada jam 12.00 - 13.00 dilarang (istirahat maksi cuy) dan akses di hari Jumat pada jam 11.00 - 13.00 juga dilarang (maklum, Jumatan rek).

### Script


### Testing

**Sukses**

**Gagal**
 
## Soal 7
> Karena terdapat 2 WebServer, kalian diminta agar setiap client yang mengakses Sein dengan Port 80 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan dan request dari client yang mengakses Stark dengan port 443 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan.

### Script 

### Testing

## Soal 8
> Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses WebServer hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024

### Script 

**Penjelasan**

### Testing

## Soal 9
> Sadar akan adanya potensial saling serang antar kubu politik, maka WebServer harus dapat secara otomatis memblokir  alamat IP yang melakukan scanning port dalam jumlah banyak (maksimal 20 scan port) di dalam selang waktu 10 menit. (clue: test dengan nmap)

### Script 


**Penjelasan**


### Testing


## Soal 10
> Karena kepala suku ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level. 

### Script 

### Testing