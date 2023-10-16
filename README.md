# Praktikum Modul 2 Jaringan Komputer

## Anggota Kelompok A08

| No.  | Nama Anggota       | NRP          |
|------|--------------------|--------------|
| 1    |Mohammad Zhafran Dzaky           | 5025211142   |
| 2    | Katarina Inezita Prambudi         | 5025211148   |

## Penjelasan Soal
### Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian [sebagai berikut](https://docs.google.com/spreadsheets/d/1OqwQblR_mXurPI4gEGqUe7v0LSr1yJViGVEzpMEm2e8/edit?usp=sharing). Folder topologi dapat diakses pada [drive berikut](https://drive.google.com/drive/folders/1Ij9J1HdIW4yyPEoDqU1kAwTn_iIxg3gk)

![image](https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/e722348c-5742-40b3-808f-4f2858742ffc)

1. Atur konfigurasi untuk masing masing node 

* Router Pandudewanata
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.3.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.3.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.3.3.1
	netmask 255.255.255.0
```

* Yudhistira DNS Master
```
auto eth0
iface eth0 inet static
	address 10.3.2.2
	netmask 255.255.255.0
	gateway 10.3.2.1
```

* Sadewa Client
```
auto eth0
iface eth0 inet static
	address 10.3.1.2
	netmask 255.255.255.0
	gateway 10.3.1.1
```

* Nakula Client
```
auto eth0
iface eth0 inet static
	address 10.3.1.3
	netmask 255.255.255.0
	gateway 10.3.1.1
```

* Werkudara DNS Slave
```
auto eth0
iface eth0 inet static
	address 10.3.3.2
	netmask 255.255.255.0
	gateway 10.3.3.1
```

* Arjuna Load Balancer
```
auto eth0
iface eth0 inet static
	address 10.3.3.3
	netmask 255.255.255.0
	gateway 10.3.3.1
```

* Abimanyu Web Server
```
auto eth0
iface eth0 inet static
	address 10.3.3.4
	netmask 255.255.255.0
	gateway 10.3.3.1
```

* Prabakusuma Web Server
```
auto eth0
iface eth0 inet static
	address 10.3.3.5
	netmask 255.255.255.0
	gateway 10.3.3.1
```

* Wisanggeni Web Server
```
auto eth0
iface eth0 inet static
	address 10.3.3.6
	netmask 255.255.255.0
	gateway 10.3.3.1
```

2. Ketikkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.3.0.0/16` pada router Pandudewanata. Setelah itu ketikkan command `cat /etc/resolv.conf`

3. Masukkan command `echo nameserver 192.168.122.1 > /etc/resolv.conf`pada semua node yang ada dan coba `ping google.com` untuk memastikan bahwa semua node sudah tersambung.
   
![image](https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/f1a87bdb-db81-44db-bf33-43cf0ccb735f)

## Soal 2
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

Untuk menjadikan arjuna sebagai website utama, kita perlu mengetikkan command di bawah ini pada Yudhistira DNS Master dan menyimpannya dalam sebuah file bernama `no2.sh`

* Lakukan instalasi bind terlebih dahulu
```
apt-get update
apt-get install bind9 -y
```

* Untuk membuat sebuah domain, masukkan konfigurasi dibawah ini
```
echo 'zone "arjuna.a08.com" {
	type master;
	file "/etc/bind/jarkom/arjuna.a08.com";
};' > /etc/bind/named.conf.local
```

* Buat sebuah folder
```
mkdir /etc/bind/jarkom
```

* Copy dile `db.local` ke dalam file yang baru saja dibuat dengan syntax seperti di bawah ini
```
cp /etc/bind/db.local /etc/bind/jarkom/arjuna.a08.com
```

* Buka file arjuna.a08.com dan edit seperti gambar di bawah ini
```
nano /etc/bind/jarkom/arjuna.a08.com
```

<img width="325" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/4c38ca91-06db-433c-ba57-e33fea4e439a">

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```

* Lakukan pengetesan pada node client lain (Nakula atau Sadewa). Disini kita menggunakan Nakula sebagai percobaan.

```
echo 'nameserver 10.3.2.2' > /etc/resolv.conf

ping arjuna.a08.com -c 5
```
<img width="331" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/fc2abafc-37b4-41be-9e44-bc0bf1a9bd8c">

## Soal 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

Untuk menjadikan abimanyu sebagai website utama, kita perlu mengetikkan command di bawah ini pada Yudhistira DNS Master dan menyimpannya dalam sebuah file bernama `no3.sh`

* Untuk membuat sebuah domain, masukkan konfigurasi dibawah ini
```
echo 'zone "abimanyu.a08.com" {
	type master;
	file "/etc/bind/jarkom/abimanyu.a08.com";
};' > /etc/bind/named.conf.local
```

* Buat sebuah folder
```
mkdir /etc/bind/jarkom
```

* Copy dile `db.local` ke dalam file yang baru saja dibuat dengan syntax seperti di bawah ini
```
cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.a08.com
```

* Buka file abimanyu.a08.com dan edit seperti gambar di bawah ini
```
nano /etc/bind/jarkom/abimanyu.a08.com
```
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.a08.com. root.abimanyu.a08.com. (
		     2023101001         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.a08.com.
@       IN      A       10.3.2.2      
www     IN      CNAME   abimanyu.a08.com. 
@       IN      AAAA    ::1' > /etc/bind/jarkom/abimanyu.a08.com
```

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```

* Lakukan pengetesan pada node client lain (Nakula atau Sadewa). Disini kita menggunakan Nakula sebagai percobaan.

```
echo 'nameserver 10.3.2.2' > /etc/resolv.conf

ping abimanyu.a08.com -c 5
```
<img width="331" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/fc2abafc-37b4-41be-9e44-bc0bf1a9bd8c">

## Soal 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

## Soal 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

## Soal 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

## Soal 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

## Soal 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

## Soal 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

## Soal 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

## Soal 11
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

## Soal 12
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

## Soal 13
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

## Soal 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

## Soal 15
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden

## Soal 16
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js

## Soal 17
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

## Soal 18
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

## Soal 19
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

## Soal 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.
