# Jarkom-Modul-2-IT45-2024

|           Nama               |     NRP    |
|            --                |     --     |
| Dian Anggraeni Putri         | 5027231016 |
| ⁠Abhirama Triadyatma Hermawan | 5027231061 |

## Topology
![image](https://github.com/user-attachments/assets/163045b3-3076-4907-87c0-0d0daec2d9b0)

## Configuration

* Nusantara
 ```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 192.239.1.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 192.239.2.1
    netmask 255.255.255.0

auto eth3
iface eth3 inet static
    address 192.239.3.1
    netmask 255.255.255.0
```
* Majapahit
 ```
auto eth0
iface eth0 inet static
    address 192.239.1.2
    netmask 255.255.255.0
    gateway 192.239.1.1
```
* Mulawarman
 ```
auto eth0
iface eth0 inet static
    address 192.239.1.3
    netmask 255.255.255.0
    gateway 192.239.1.1
```
* Grahambell
 ```
auto eth0
iface eth0 inet static
    address 192.239.1.4
    netmask 255.255.255.0
    gateway 192.239.1.1
```
* Samaratungga
```
auto eth0
iface eth0 inet static
    address 192.239.1.5
    netmask 255.255.255.0
    gateway 192.239.1.1
```
* Solok
```
auto eth0
iface eth0 inet static
    address 192.239.2.2
    netmask 255.255.255.0
    gateway 192.239.2.1
```
* Srikandi
```
auto eth0
iface eth0 inet static
    address 192.239.2.3
    netmask 255.255.255.0
    gateway 192.239.2.1
```
* Kotalingga
```
auto eth0
iface eth0 inet static
    address 192.239.2.4
    netmask 255.255.255.0
    gateway 192.239.2.1
```
* Sriwijaya
```
auto eth0
iface eth0 inet static
    address 192.239.2.5
    netmask 255.255.255.0
    gateway 192.239.2.1
```
* Tanjungkulai
```
auto eth0
iface eth0 inet static
    address 192.239.2.6
    netmask 255.255.255.0
    gateway 192.239.2.1
```
* Bedahulu
```
auto eth0
iface eth0 inet static
    address 192.239.2.7
    netmask 255.255.255.0
    gateway 192.239.2.1
```

# Soal:
Sebuah kerajaan besar di Indonesia sedang mengalami pertempuran dengan penjajah. Kerajaan tersebut adalah Sriwijaya. Karena merasa terdesak Sriwijaya meminta bantuan pada Majapahit untuk mempertahankan wilayahnya. Pertempuran besar tersebut berada di Nusantara. 
## 1
> Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave.

Langkah pertama, cek nameserver yang digunakan oleh node Nusantara dengan command berikut:
```
cat /etc/resolv.conf
```
Hasilnya akan menunjukkan bahwa nameserver yang digunakan adalah `192.168.122.1`. Selanjutnya, pada node lain (selain Nusantara), edit file `/root/.bashrc` menggunakan command berikut:

```
nano /root/.bashrc
```
Lalu tambahkan baris ini di bagian paling bawah:
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
```

Untuk node Sriwijaya, jalankan command berikut untuk menginstal `bind9` karena Sriwijaya menjadi DNS Master:
```
apt-get install bind9 -y
```

Sedangkan, pada node Nusantara, gunakan command berikut untuk melakukan NAT:
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.239.0.0/16
```

Setelah itu, restart seluruh node dan memastikan untuk melakukan tes koneksi (PING) dari setiap node untuk memastikan jaringan berfungsi dengan benar.
### Result
![image](https://github.com/user-attachments/assets/794c0c53-ebbc-4c87-8c7b-e4eea0325fe4)
![image](https://github.com/user-attachments/assets/52c19f62-4b30-48ab-8af0-c467d2a44752)
![image](https://github.com/user-attachments/assets/82a47412-5f13-476b-9d17-cb109ab183bc)

## 2
> Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

Simpan nameserver milik Nusantara dan Sriwijaya menggunakan command berikut:
```
nano /etc/resolv.conf
```
Isinya menjadi seperti ini
```
nameserver 192.168.122.1
nameserver 192.239.2.5
```

Jalankan command berikut untuk menambahkan script kedalam /root/.bashrc
```
nano /root/.bashrc
```

Kemudian, masukkan skrip berikut ke dalam file tersebut:
```
#!/bin/bash

# Konfigurasi zona DNS di named.conf.local
echo 'zone "sudarsana.it45.com" {
    type master;
    file "/etc/bind/jarkom/sudarsana.it45.com";
};' > /etc/bind/named.conf.local

# Membuat direktori jika belum ada
mkdir -p /etc/bind/jarkom

# Menyalin template file db.local ke file zona baru
cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it45.com

# Menulis konfigurasi zona DNS
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     root.sudarsana.it45.com. admin.sudarsana.it45.com. (
                        2024100301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it45.com.
@       IN      A       192.239.2.2     ; IP Solok
www     IN      CNAME   sudarsana.it45.com.' > /etc/bind/jarkom/sudarsana.it45.com

# Restart service BIND
sudo service bind9 restart
```

Setelah itu, jika diperlukan restart layanan BIND dengan command:
```
service bind9 restart
```

Apabila BIND belum terinstall, lakukan instalasi dengan command:
```
apt install bind9 bind9utils bind9-doc -y
```

Terakhir, tes PING ke domain tersebut dari node Sriwijaya dengan command:
```
ping sudarsana.it45.com
```

### Result
![image](https://github.com/user-attachments/assets/bd5a21a0-947c-4417-962c-8cc85be18550)

## 3
> Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

Jalankan command berikut untuk menambahkan script kedalam /root/.bashrc
```
nano /root/.bashrc
```

Kemudian, masukkan skrip berikut ke dalam file tersebut:

```
#!/bin/bash

# Menambahkan zona ke konfigurasi BIND
echo 'zone "pasopati.it45.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it45.com";
};' > /etc/bind/named.conf.local

# Membuat direktori jika belum ada
mkdir -p /etc/bind/jarkom

# Template file DNS
cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it45.com

# Mengatur file zona
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it45.com. pasopati.it45.com. (
                        2024100301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it45.com.
@       IN      A       192.239.2.4     ; IP Kotalingga
www     IN      CNAME   pasopati.it45.com.' > /etc/bind/jarkom/pasopati.it45.com

# Memeriksa konfigurasi BIND
named-checkconf

# Restart layanan BIND9
service bind9 restart
```

Terakhir, coba lakukan tes PING ke domain tersebut dari node Sriwijaya dengan command:
```
ping pasopati.it45.com
```

### Result
![image](https://github.com/user-attachments/assets/151cb6af-d73c-47da-9e6c-38859fca2df8)

## 4
> Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

Jalankan command berikut untuk menambahkan script kedalam /root/.bashrc
```
nano /root/.bashrc
```

Kemudian, masukkan skrip berikut ke dalam file tersebut:

```
#!/bin/bash

echo 'zone "rujapala.it45.com" {
	type master;
	file "/etc/bind/jarkom/rujapala.it45.com";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it45.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it45.com. rujapala.it45.com. (
                        2024100301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it45.com.
@       IN      A       192.239.2.6     ; IP Tanjungkulai
www     IN      CNAME   rujapala.it45.com.' > /etc/bind/jarkom/rujapala.it45.com

service bind9 restart

```

Terakhir, coba lakukan tes PING ke domain tersebut dari node Sriwijaya dengan command:
```
ping rujapala.it45.com
```

### Result
![image](https://github.com/user-attachments/assets/b69ff532-afcf-4ffc-a4fc-135a8979da61)

## 5
> Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.

Kita akan melakukan pengujian konektivitas dengan mengirimkan PING ke server `sudarsana.it45.com`, `pasopati.it45.com`, dan `rujapala.it45.com` dari klien Mulawarman, GrahamBell, Samaratungga, dan Srikandi. Kita dapat memverifikasi apakah koneksi ke server-server tersebut berfungsi dengan baik dengan mengimplementasikan command-command berikut: (agar hasilnya tidak menampilkan sangat banyak, maka bisa diambil sampel dengan memakai -c 2)

```
ping -c 2 sudarsana.it45.com
```

```
ping -c 2 pasopati.it45.com
```

```
ping -c 2 rujapala.it45.com
```

### Result (Mulawarman)
![image](https://github.com/user-attachments/assets/9b726553-ca87-4f2c-9468-86cf2d97793f)

### Result (GrahamBell)
![image](https://github.com/user-attachments/assets/94d4aacd-1e2f-4623-91b3-0a2c06c3c63c)

### Result (Samaratungga)
![image](https://github.com/user-attachments/assets/191757ee-0602-4324-b6f3-5ef3679e19d8)

### Result (Srikandi)
![image](https://github.com/user-attachments/assets/fb6d7b11-1db6-4773-8970-9642e3a8b732)

## 6
> Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

```
apt-get update
apt-get install bind9 dnsutils -y

echo 'zone "2.239.192.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.239.192.in-addr.arpa";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/2.239.192.in-addr.arpa

echo '
;
; BIND reverse data file for 192.239.2.12
;
$TTL    604800
@       IN      SOA     pasopati.it23.com. pasopati.it23.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
2.239.192.in-addr.arpa   IN      NS      pasopati.it45.com.
12                      IN      PTR     pasopati.it45.com.' > /etc/bind/jarkom/2.239.192.in-addr.arpa

service bind9 restart

dig -x 192.239.2.12
```

## 7
> Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

## 8
> Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

Jalankan command berikut untuk menambahkan script kedalam /root/.bashrc
```
nano /root/.bashrc
```
Kemudian, masukkan skrip berikut ke dalam file tersebut:
```
#!/bin/bash

# Konfigurasi zona DNS di named.conf.local
echo 'zone "sudarsana.it45.com" {
    type master;
    file "/etc/bind/jarkom/sudarsana.it45.com";
};' > /etc/bind/named.conf.local

# Membuat direktori jika belum ada
mkdir -p /etc/bind/jarkom

# Menyalin template file db.local ke file zona baru
cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it45.com

# Menulis konfigurasi zona DNS
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     root.sudarsana.it45.com. admin.sudarsana.it45.com. (
                        2024100301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it45.com.
@       IN      A       192.239.2.2     ; IP Solok
www     IN      CNAME   sudarsana.it45.com.
cakra       IN      A       192.239.2.7     ; IP Bedahulu
www     IN      CNAME   sudarsana.it45.com.' > /etc/bind/jarkom/sudarsana.it45.com

# Restart service BIND
sudo service bind9 restart
```

### Result
![image](https://github.com/user-attachments/assets/3bae65b1-9b99-4f11-b775-2b7ec7b58a45)

## 9
> Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga.

## 10
> Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.

## 11
> Setelah pertempuran mereda, warga IT dapat kembali mengakses jaringan luar dan menikmati meme brainrot terbaru, tetapi hanya warga Majapahit saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga IT yang berada diluar Majapahit dapat mengakses jaringan luar melalui DNS Server Majapahit.

## 12
> Karena pusat ingin sebuah laman web yang ingin digunakan untuk memantau kondisi kota lainnya maka deploy laman web ini (cek resource yg lb) pada Kotalingga menggunakan apache.

## 13
> Karena Sriwijaya dan Majapahit memenangkan pertempuran ini dan memiliki banyak uang dari hasil penjarahan (sebanyak 35 juta, belum dipotong pajak) maka pusat meminta kita memasang load balancer untuk membagikan uangnya pada web nya, dengan Kotalingga, Bedahulu, Tanjungkulai sebagai worker dan Solok sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancer nya.

## 14
> Selama melakukan penjarahan mereka melihat bagaimana web server luar negeri, hal ini membuat mereka iri, dengki, sirik dan ingin flexing sehingga meminta agar web server dan load balancer nya diubah menjadi nginx.

## 15
> Markas pusat meminta laporan hasil benchmark dengan menggunakan apache benchmark dari load balancer dengan 2 web server yang berbeda tersebut dan meminta secara detail dengan ketentuan:
> - Nama Algoritma Load Balancer
> - Report hasil testing apache benchmark 
> - Grafik request per second untuk masing masing algoritma. 
> - Analisis
> - Meme terbaik kalian (terserah ( ͡° ͜ʖ ͡°)) 🤓

## 16
> Karena dirasa kurang aman dari brainrot karena masih memakai IP, markas ingin akses ke Solok memakai solok.xxxx.com dengan alias www.solok.xxxx.com (sesuai web server terbaik hasil analisis kalian).

## 17
> Agar aman, buatlah konfigurasi agar solok.xxx.com hanya dapat diakses melalui port sebesar π x 10^4 = (phi nya desimal) dan 2000 + 2000 log 10 (10) +700 - π = ?.

## 18
> Apa bila ada yang mencoba mengakses IP solok akan secara otomatis dialihkan ke www.solok.xxxx.com.

## 19
> Karena probset sudah kehabisan ide masuk ke salah satu worker buatkan akses direktori listing yang mengarah ke resource worker2.

## 20
> Worker tersebut harus dapat di akses dengan sekiantterimakasih.xxxx.com dengan alias www.sekiantterimakasih.xxxx.com.










