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
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.236.0.0/16
```

Setelah itu, restart seluruh node dan memastikan untuk melakukan tes koneksi (PING) dari setiap node untuk memastikan jaringan berfungsi dengan benar.
### Result
![image](https://github.com/user-attachments/assets/794c0c53-ebbc-4c87-8c7b-e4eea0325fe4)
![image](https://github.com/user-attachments/assets/52c19f62-4b30-48ab-8af0-c467d2a44752)
![image](https://github.com/user-attachments/assets/82a47412-5f13-476b-9d17-cb109ab183bc)






