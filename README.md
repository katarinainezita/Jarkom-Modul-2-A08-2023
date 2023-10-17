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

<img width="319" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/d1039ce3-7707-42e8-a14a-26104c25f7f3">

## Soal 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

Untuk membuat subdomain `parikesit`, cukup menambahkan konfigurasi pada Yudhistira dan disimpan dalam file `no4.sh` seperti gambar dibawah ini

<img width="345" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/00831c22-aaed-4243-95f3-b7b27617c0cf">

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```
* Lakukan pengetesan pada node client lain (Nakula atau Sadewa). Disini kita menggunakan Nakula sebagai percobaan.

```
echo 'nameserver 10.3.2.2' > /etc/resolv.conf

ping parikesit.abimanyu.a08.com -c 5
```

<img width="328" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/d34f4941-2ac8-40dc-9409-e14b1fd55e23">


## Soal 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

Untuk melakukan reverse domain, perlu mengedit file `/etc/bind/named.conf.local` pada Yudhistira dan menyimpannya pada file `no5.sh`

* Masukkan konfigurasi seperti dibawah ini

```
echo 'zone "2.3.10.in-addr.arpa" {
	type master;
	file "/etc/bind/jarkom/2.3.10.in-addr.arpa";
};' > /etc/bind/named.conf.local
```

* Copykan file db.local
```
cp /etc/bind/db.local /etc/bind/jarkom/2.3.10.in-addr.arpa
```

* Edit file `/etc/bind/jarkom/2.3.10.in-addr.arpa` seperti dibawah ini
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
2.3.10.in-addr.arpa.	IN	NS      abimanyu.a08.com.
2 			IN 	PTR 	abimanyu.a08.com. ' > /etc/bind/jarkom/2.3.10.in-addr.arpa
```
* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```

* Lakukan pengetesan pada node client lain (Nakula atau Sadewa). Disini kita menggunakan Nakula sebagai percobaan.

```
echo 'nameserver 10.3.2.2
nameserver 192.168.122.1
' > /etc/resolv.conf

apt-get update
apt-get install dnsutils
```

* Kemudian ketikkan
```
host -t PTR 10.3.2.2
```

<img width="254" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/e2d37c6e-8744-4597-ae4e-40674ec13765">

## Soal 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

Untuk membuat Werkudara sebagai DNS Slave, perlu melakukan konfigurasi pada file `/etc/bind/named.conf.local` yang nantinya akan di simpan pada file `no6.sh` pada yudhistira

* Masukkan syntax seperti gambar dibawah ini

<img width="264" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/47c446ff-8058-4c3b-abe2-dbbc973601ea">

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```

Pada Werkudara DNS Slave, ketikkan beberapa command dibawah dan simpan didalam file `no6.sh`

* Lakukan konfigurasi seperti dibawah ini
```
apt-get update
apt-get install bind9 -y
```

* Tambahkan syntax seperti dibawah ini pada file `/etc/bind/named.conf.local`

<img width="238" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/8334084c-8119-4a48-9b5f-e81e9ba9c3b7">

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```

* Lakukan testing pada Yudhistira
```
service bind9 stop
```

<img width="535" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/a42c4144-3641-441f-b018-0e90472be8f9">

* Lakukan pengetesan pada node client lain (Nakula atau Sadewa). Disini kita menggunakan Nakula sebagai percobaan.

```
echo 'nameserver 10.3.2.2
nameserver 10.3.3.2' > /etc/resolv.conf

ping abimanyu.a08.com -c 5
```

<img width="319" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/4fec73c3-3ce0-4e8c-b80d-705c2909373e">

## Soal 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

* Pada Yudhistira DNS Master ubah konfigurasi mengikuti syntax dibawah ini dan simpan file dalam `no7.sh`

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
@                       IN      NS      abimanyu.a08.com.
@                       IN      A       10.3.3.4
www                     IN      CNAME   abimanyu.a08.com.
parikesit               IN      A       10.3.3.4 
ns1                     IN      A       10.3.3.2
baratayuda              IN      NS      ns1
@       IN      AAAA    ::1' > /etc/bind/jarkom/abimanyu.a08.com
```

* Buka file `/etc/bind/named.conf.options` dan comment `dnssec-validation auto` serta tambahkan `allow-query{any;};` atau bisa langsung menuliskan syntax dibawah ini pada file `no7.sh`

```
echo 'options {
	directory "/var/cache/bind";

	// If there is a firewall between you and nameservers you want
	// to talk to, you may need to fix the firewall to allow the multiple
	// ports to talk. See http://www.kb.cert.org/vuls/id/800113

	// If your ISP provided one or more IP addresses for stable
	// nameservers, you probably want to use them as forwarders.
	// Uncomment the following block, and insert the addresses replacing
	// the all-0s placeholder.

    	// forwarders {
    	//      0.0.0.0;
    	// };

	//===================================================================$
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys. See https://www.isc.org/bind-keys
	//===================================================================$
    	//dnssec-validation auto;
	allow-query{any;};

    	auth-nxdomain no;    # conform to RFC1035
    	listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```

* Kemudian edit file  `/etc/bind/named.conf.local` seperti syntax dibawah ini :
```
echo 'zone "arjuna.a08.com" {
	type master;
	notify yes;
	also-notify { 10.3.3.2; };
	allow-transfer { 10.3.3.2; };
	file "/etc/bind/jarkom/arjuna.a08.com";
};

echo 'zone "2.3.10.in-addr.arpa" {
	type master;
	file "/etc/bind/jarkom/2.3.10.in-addr.arpa";
};

echo 'zone "abimanyu.a08.com" {
	type master;
	file "/etc/bind/jarkom/abimanyu.a08.com";
	allow-transfer { 10.3.3.2; };
};' > /etc/bind/named.conf.local
```

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```

* Pada Werkudara DNS Slave, buka file `/etc/bind/named.conf.options` dan comment `dnssec-validation auto` serta tambahkan `allow-query{any;};` atau bisa langsung menuliskan syntax dibawah ini pada file `no7.sh`

```
echo 'options {
	directory "/var/cache/bind";

	// If there is a firewall between you and nameservers you want
	// to talk to, you may need to fix the firewall to allow the multiple
	// ports to talk. See http://www.kb.cert.org/vuls/id/800113

	// If your ISP provided one or more IP addresses for stable
	// nameservers, you probably want to use them as forwarders.
	// Uncomment the following block, and insert the addresses replacing
	// the all-0s placeholder.

    	// forwarders {
    	//      0.0.0.0;
    	// };

	//===================================================================$
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys. See https://www.isc.org/bind-keys
	//===================================================================$
    	//dnssec-validation auto;
	allow-query{any;};

    	auth-nxdomain no;    # conform to RFC1035
    	listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```

* Kemudian edit file `/etc/bind/named.conf.local` seperti syntax dibawah ini :
```
echo 'zone "baratayuda.abimanyu.a08.com" {
	type master;
	file "/etc/bind/baratayuda/baratayuda.abimanyu.a08.com";
};' > /etc/bind/named.conf.local
```

* Buat direktori
```
mkdir /etc/bind/baratayuda
```

* Copy db.local ke direktori tersebut
```
cp /etc/bind/db.local /etc/bind/baratayuda/baratayuda.abimanyu.a08.com
```

* Edit file `baratayuda.abimanyu.a08.com` seperti di bawah ini
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@               IN      SOA     baratayuda.abimanyu.a08.com. root.baratayuda.abimanyu.a08.com. (
                2023101001         ; Serial
		    604800         ; Refresh
		    86400          ; Retry
		    2419200        ; Expire
		    604800 )       ; Negative Cache TTL
;
@               IN      NS      baratayuda.abimanyu.a08.com.
@               IN      A       10.3.2.2
www             IN      CNAME   baratayuda.abimanyu.a08.com.
rjp		IN	A	10.3.3.4
www.rjp		IN	CNAME	rjp.baratayuda.abimanyu.a08.com
@               IN      AAAA    ::1' > /etc/bind/baratayuda/baratayuda.abimanyu.a08.com
```

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```

* Lakukan pengetesan pada node client lain (Nakula atau Sadewa). Disini kita menggunakan Nakula sebagai percobaan.

```
echo 'nameserver 10.3.2.2
nameserver 10.3.3.2' > /etc/resolv.conf

ping abimanyu.a08.com -c 3
ping baratayuda.abimanyu.a08.com -c 3
ping www.baratayuda.abimanyu.a08.com -c 3
```

<img width="396" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/109232320/922e397c-1451-4059-af3f-c26732ffccaf">

## Soal 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

* Pada Werkudara DNS Slave, buka file `/etc/bind/named.conf.options` dan comment `dnssec-validation auto` serta tambahkan `allow-query{any;};` atau bisa langsung menuliskan syntax dibawah ini pada file `no7.sh`

```
echo 'options {
	directory "/var/cache/bind";

	// If there is a firewall between you and nameservers you want
	// to talk to, you may need to fix the firewall to allow the multiple
	// ports to talk. See http://www.kb.cert.org/vuls/id/800113

	// If your ISP provided one or more IP addresses for stable
	// nameservers, you probably want to use them as forwarders.
	// Uncomment the following block, and insert the addresses replacing
	// the all-0s placeholder.

    	// forwarders {
    	//      0.0.0.0;
    	// };

	//===================================================================$
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys. See https://www.isc.org/bind-keys
	//===================================================================$
    	//dnssec-validation auto;
	allow-query{any;};

    	auth-nxdomain no;    # conform to RFC1035
    	listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```

* Kemudian edit file `/etc/bind/named.conf.local` seperti syntax dibawah ini :
```
echo 'zone "baratayuda.abimanyu.a08.com" {
	type master;
	file "/etc/bind/baratayuda/baratayuda.abimanyu.a08.com";
};' > /etc/bind/named.conf.local
```

* Buat direktori
```
mkdir /etc/bind/baratayuda
```

* Copy db.local ke direktori tersebut
```
cp /etc/bind/db.local /etc/bind/baratayuda/baratayuda.abimanyu.a08.com
```

* Edit file `baratayuda.abimanyu.a08.com` seperti di bawah ini
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@               IN      SOA     baratayuda.abimanyu.a08.com. root.baratayuda.abimanyu.a08.com. (
                2023101001         ; Serial
		    604800         ; Refresh
		    86400          ; Retry
		    2419200        ; Expire
		    604800 )       ; Negative Cache TTL
;
@               IN      NS      baratayuda.abimanyu.a08.com.
@               IN      A       10.3.2.2
www             IN      CNAME   baratayuda.abimanyu.a08.com.
rjp		IN	A	10.3.3.4
www.rjp		IN	CNAME	rjp.baratayuda.abimanyu.a08.com
@               IN      AAAA    ::1' > /etc/bind/baratayuda/baratayuda.abimanyu.a08.com
```

* Restart bind9 sesuai syntax di bawah
```
service bind9 restart
```
<img width="718" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/64f0154d-2b6e-4514-8c64-1b68bc9e57e6">


## Soal 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

* Pada Arjuna Load Balancer, tulisan syntax ini pada file `arjuna_nginx.sh`
```
apt-get update
apt-get install bind9 nginx
```

```
echo '
zone "jarkom.site" {
	type master;	
	file "/etc/bind/jarkom/jarkom.site";
};

zone "2.168.192.in-addr.arpa" {
	type master;
	file "/etc/bind/jarkom/2.168.192.in-addr.arpa";
};'
```

* Pada Arjuna, buat file bernama `no9.sh` dan ketikkan syntax dibawah ini

```
apt-get update
apt-get install bind9 nginx
touch /etc/nginx/sites-available/lb-jarkom

echo '
upstream myweb {
	server 10.3.3.5:8001;
	server 10.3.3.4:8002;
	server 10.3.3.6:8006;
}

server {
	lister 80;
	server_name arjuna.a08.com www.arjuna.a08.com;

	location / {
	proxy_pass http://myweb;
	}
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
```

* Pada Abimanyu Web Server, ketikkan syntax dibawah ini dan simpan dalam file bernama `no9.sh`

```
apt-get update && apt-get install nginx php php-fpm -y
mkdir /var/www/jarkom
touch /var/www/jarkom/index.php

echo '<?php
$hostname = gethostname();
$date = date("Y-m-d H:i:s");
$php_version = phpversion();
$username = get_current_user();

echo "Hello World! <br>";
echo "Saya adalah: $username<br>";
echo "Saat ini berada di : $hostname<br>";
?>' > /var/www/jarkom/index.php

touch /etc/nginx/sites-available/jarkom

echo 'server {
	listen 8002;

	root /var/www/jarkom;

	index index.php index.html index.htm;
	sever_name _;

	location / {
			try_files $uri $uri/ /index.php?$query_string;
	}

	# pass PHP scripts to FastCGI server
	location ~ \.php$ {
	include snippets/fastcgi-php.conf;
	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
	}

location ~ /\.ht {
			deny all;
	}

	error_log /var/log/nginx/jarkom_error.log
	access_log /var/log/nginx/jarkom_access.log
}' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service php7.0-fpm start
service php7.0-fpm restart
service nginx restart
nginx -t
```

* Pada Prabukusuma Web Server, ketikkan syntax dibawah ini dan simpan dalam file bernama `no9.sh`

```
apt-get update && apt-get install nginx php php-fpm -y
mkdir /var/www/jarkom
touch /var/www/jarkom/index.php

echo '<?php
$hostname = gethostname();
$date = date("Y-m-d H:i:s");
$php_version = phpversion();
$username = get_current_user();

echo "Hello World! <br>";
echo "Saya adalah: $username<br>";
echo "Saat ini berada di: $hostname<br>";
echo "Versi PHP yang saya gunakan: $php_version<br>";
echo "Tanggal saat ini: $date<br>";
?>' > /var/www/jarkom/index.php

touch /etc/nginx/sites-available/jarkom

echo 'server {
	listen 8001;

	root /var/www/jarkom;

	index index.php index.html index.htm;
	sever_name _;

	location / {
			try_files $uri $uri/ /index.php?$query_string;
	}

	# pass PHP scripts to FastCGI server
	location ~ \.php$ {
	include snippets/fastcgi-php.conf;
	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
	}

location ~ /\.ht {
			deny all;
	}

	error_log /var/log/nginx/jarkom_error.log
	access_log /var/log/nginx/jarkom_access.log
}' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service php7.0-fpm start
service php7.0-fpm restart
service nginx restart
nginx -t
```

* Pada Wisanggeni Web Server, ketikkan syntax dibawah ini dan simpan dalam file bernama `no9.sh`

```
apt-get update && apt-get install nginx php php-fpm -y
mkdir /var/www/jarkom
touch /var/www/jarkom/index.php

echo '<?php
$hostname = gethostname();
$date = date("Y-m-d H:i:s");
$php_version = phpversion();
$username = get_current_user();

echo "Hello World! <br>";
echo "Saya adalah: $username<br>";
echo "Saat ini berada di: $hostname<br>";
echo "Versi PHP yang saya gunakan: $php_version<br>";
echo "Tanggal saat ini: $date<br>";
?>' > /var/www/jarkom/index.php

touch /etc/nginx/sites-available/jarkom

echo 'server {
	listen 8003;

	root /var/www/jarkom;

	index index.php index.html index.htm;
	sever_name _;

	location / {
			try_files $uri $uri/ /index.php?$query_string;
	}

	# pass PHP scripts to FastCGI server
	location ~ \.php$ {
	include snippets/fastcgi-php.conf;
	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
	}

location ~ /\.ht {
			deny all;
	}

	error_log /var/log/nginx/jarkom_error.log
	access_log /var/log/nginx/jarkom_access.log
}' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service php7.0-fpm start
service php7.0-fpm restart
service nginx restart
nginx -t
```

- lalu tes pada salah satu client (sadewa) untuk akses ke arjuna.a08.com
<img width="718" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/c68c2d33-82b5-4ec2-9810-4be6018ad896">


## Soal 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

- Untuk penyelesaian nomor 10 kurang lebih sama dengan nomor 9, karena pada dasarnya, nginx memiliki default algoritma Round Robin, sehingga yang perlu dilakukan hanyalah mengubah port untuk masing - masing server dan mengurutkannya di load balancer.
- Ini adalah update untuk arjuna :
```
#!/bin/bash

echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install bind9 nginx
touch /etc/nginx/sites-available/lb-jarkom

echo '
upstream myweb {
        server 10.3.3.5:8001;
        server 10.3.3.4:8002;
        server 10.3.3.6:8003;
}

server {
        listen 80;
        server_name arjuna.a08.com www.arjuna.a08.com;

        location / {
        proxy_pass http://myweb;
        }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart

nginx -t
```

- ini update untuk abimanyu :
```
echo 'server {

        listen 8002;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
}' > /etc/nginx/sites-available/jarkom
```

- ini update untuk prabakusuma:
```
echo 'server {

        listen 8001;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
}' > /etc/nginx/sites-available/jarkom
```

- ini update untuk wisanggeni :
```
echo 'server {

        listen 8003;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
}' > /etc/nginx/sites-available/jarkom
```

- lalu untuk tesnya, kita dapat mengaksesnya pada salah satu client, sadewa, dengan lynx arjuna.a08.com dan click ctrl + r untuk mengganti portnya:

<img width="694" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/6559ba80-d248-4dd7-a4e9-d2d9cd97aae4">

<img width="706" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/6b5ca4d4-176c-4be3-84f3-0a2672f59f1c">

<img width="706" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/48e2e016-cde1-494e-ba26-ae9232476a5e">

## Soal 11
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

* Pada Abimanyu Web Server, ketikkan syntax dibawah ini dan simpan dalam file bernama `no11.sh`

```
apt-get install apache2
apt-get install php
apt-get install libapache2-mod-php7.0
apt-get install wget
apt-get install unzip

mkdir /var/www/abimanyu.a08

touch /etc/apache2/sites-available/abimanyu.a08.com.conf

echo '<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/abimanyu.a08
	ServerName abimanyu.a08.com
	ServerAlias www.abimanyu.a08.com

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/abimanyu.a08.com.conf

a2enmod rewrite abimanyu.a08.com.conf
a2ensite abimanyu.a08.com.conf
service apache2 restart

wget 'https://drive.usercontent.google.com/download?id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc&export=download&authuser=0&confirm=t&uuid=ba803a01-09da-443a-aca9-f9af1f05407a&at=APZUnTW3k1liYo8m6AXA1QNDCNX1:1696931215102'
unzip -j /var/www/abimanyu.a08/abimanyu.zip -d /var/www/abimanyu.a08

rm -rf /var/www/abimanyu.a08/abimanyu.zip 
```

- kemudian tes di client apakah abimanyu.a08.com atau www.abimanyu.a08.com sudah dapat diakses atau belum menggunakan lynx abimanyu.a08.com dan lynx www.abimanyu.a08.com. Didapatkan bahwa dua - duanya sudah menunjukkan bahwa deployment sudah berhasil

<img width="709" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/0818d6d6-b1de-42b7-961a-433627f9d6d8">

<img width="704" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/0f11e7cb-8b38-4537-b49e-63649b7676f8">


## Soal 12
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

- untuk menyelesaikan masalah ini, kita dapat menggunakan directory alias yang sudah dijelaskan di modul. perintah ini dapat kita tambahkan di file konfigurasi abimanyu.a08

```
<Directory /var/www/abimanyu.a08/>
		Options +Indexes
</Directory>

Alias "/home" "/var/www/abimanyu.a08/index.php/home"
```

- kemudian tes di client apakah abimanyu.a08.com/home sudah dapat diakses atau belum.

<img width="706" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/3efb3866-99df-4c01-9962-169c64ddbdb3">

<img width="709" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/865d78d8-cb29-4a5d-81a5-4209b9c15ae8">

## Soal 13
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

* Pada Abimanyu Web Server, ketikkan syntax dibawah ini dan simpan dalam file bernama `no13.sh`

```
mkdir /var/www/parikesit.abimanyu.a08

touch /etc/apache2/sites-available/parikesit.abimanyu.a08.com.conf

echo '<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/parikesit.abimanyu.a08
	ServerName parikesit.abimanyu.a08
	ServerAlias www.parikesit.abimanyu.a08

	<Directory /var/www/parikesit.abimanyu.a08/>
		Options +Indexes
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/parikesit.abimanyu.a08.com.conf

a2enmod rewrite parikesit.abimanyu.a08.com.conf
a2ensite parikesit.abimanyu.a08.com.conf
service apache2 restart

unzip -j /var/www/parikesit.abimanyu.a08/abimanyu.zip -d /var/www/parikesit.abimanyu.a08

rm -rf /var/www/parikesit.abimanyu.a08/parikesit_abimanyu.zip 
```

- kemudian tes di client apakah parikesit.abimanyu.a08.com dapat diakses atau belum
  
<img width="705" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/9e89e106-9805-4001-8ffe-c5cf13f0e726">

<img width="707" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/3e5ca7dc-ab47-4776-813e-d4c3c5b82d9e">

## Soal 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

* Pada Abimanyu Web Server, ketikkan syntax dibawah ini dan simpan dalam file bernama `no14.sh`

```
echo '<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/parikesit.abimanyu.a08
	ServerName parikesit.abimanyu.a08.com
	ServerAlias www.parikesit.abimanyu.a08.com

	<Directory /var/www/parikesit.abimanyu.a08/public/>
		Options +Indexes
	</Directory>

	<Directory /var/www/parikesit.abimanyu.a08/secret/>
		Options -Indexes
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/parikesit.abimanyu.a08.com.conf

a2enmod rewrite parikesit.abimanyu.a08.com.conf
a2ensite parikesit.abimanyu.a08.com.conf
service apache2 restart

rm -rf /var/www/parikesit.abimanyu.a08
mkdir var/www/parikesit.abimanyu.a08
wget 'https://drive.usercontent.google.com/download?id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS&export=download&authuser=0&confirm=t&uuid=7440274a-d695-44db-8cd9-70df5bbf7c96&at=APZUnTWw6S5Rd4s_a6CHfvUKTqQG:1696947964978'
unzip -j /var/www/parikesit.abimanyu.a08/parikesit_abimanyu.zip -d /var/www/parikesit.abimanyu.a08
rm -rf /var/www/parikesit.abimanyu.a08/parikesit_abimanyu.zip

mv /var/www/parikesit.abimanyu.a08/parikesit.abimanyu.yyy.com/public
mv /var/www/parikesit.abimanyu.a08/parikesit.abimanyu.yyy.com/error
mkdir /var/www/parikesit.abimanyu.a08/secret
cp /var/www/parikesit.abimanyu.a08/error/403.html
rm -rf /var/www/parikesit.abimanyu.a08/parikesit.abimanyu.yyy.com

```
- dapat dilihat bahwa untuk directory public, command yang dituliskan adalah Option +Indexes yang mana directory tersebut dapat diakses secara bebas, namun untuk directory secret, command yang dituliskan adalah Option -Indexes yang mana directory tersebut tidak dapat diakses oleh pengguna. Namun karena di dalam starter content parikesit yang didapatkan dari resource gdrive tidak memiliki folder secret, maka kita buat terlebih dahulu foldernya dan isi dengan file sebarang.

- kemudian kita dapat tes untuk akses directory public dan directory secret sebagai berikut:

- directory public :
  
<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/673e7b71-a0d3-4086-b68d-727fc3982448">

- directory secret :

<img width="867" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/d671e314-ccd0-48b3-8f21-12b015c77361">

## Soal 15
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden

- Untuk menyelesaikan permasalahan ini adalah kita dapat menambahkan command berikut di file config parikesit.

```
ErrorDocument 403 /error/403.html
ErrorDocument 404 /error/404.html
```

sehingga nantinya apabila ada error 403 (forbidden directory) maka akan ditampilkan file 403.html, namun apabila ada error 404 (directory not found) maka akan ditampilkan file 404.html

- contoh error 403 :

<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/1d442c51-6fd4-4301-ab03-829b1fc209b5">

- contoh error 404 :

<img width="862" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/3df2aa8c-5a1f-4886-a31a-2d36b10aae8d">

## Soal 16
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js

Untuk menjawab soal nomor 16, bisa melakukan perintah di bawah ini di file config prikesit

```
Alias "/js" "/var/www/parikesit.abimanyu.a08/public/js"
```

- kemudian tes di client apakah parikesit.abimanyu.a08.com/js dapat diakses dan mengarah langsung ke /public/js

<img width="857" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/e08bd9ee-54a4-4c9e-bd9e-ea5476e022fa">

<img width="861" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/b7c48072-d68e-4e99-ae36-e09be4c7a92b">


## Soal 17
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

- untuk menjawab soal ini, kita dapat membuat terlebih dahulu domain rjp.baratayuda.abimanyu.a08.com lalu atur port nya untuk listen ke 14000 dan 14400 di file confignya seperti berikut:

```
mkdir /var/www/rjp.baratayuda.abimanyu.a08

touch /etc/apache2/sites-available/rjp.baratayuda.abimanyu.a08.com.conf

echo '<VirtualHost *:14000 *:14400>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.a08
        ServerName rjp.baratayuda.abimanyu.a08.com
        ServerAlias www.rjp.baratayuda.abimanyu.a08.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/rjp.baratayuda.abimanyu.a08.com.conf
```

- kemudian jangan lupa untuk setting konfigurasi port milik apache di /etc/apache2/ports.conf untuk juga listen ke port 14000 dan 14400 seperti berikut:

```
echo '# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80
Listen 14000
Listen 14400

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet' > /etc/apache2/ports.conf
```

- jangan lupa untuk download resource untuk rjp.baratayuda.abimanyu di gdrive dengan syntax yang sama seperti sebelumnya. kemudian enable site dan restart service apache nya seperti berikut:

```
wget 'https://drive.usercontent.google.com/download?id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6&export=download&authuser=0&confirm=t&uuid=c039943d-843b-47b9-8910-4b46edf58596&at=APZUnTWSNiEJdlQx1mlaCEI-F6-B:169695756$unzip /var/www/rjp.baratayuda.abimanyu.a08/rjp_baratayuda_abimanyu.zip -d /var/www/rjp.baratayuda.abimanyu.a08

rm -rf /var/www/rjp.baratayuda.abimanyu.a08/rjp_baratayuda_abimanyu.zip

a2ensite rjp.baratayuda.abimanyu.a08.com.conf
service apache2 restart
```

- yang terakhir tes di client apakah bisa diakses atau tidak untuk domain rjp.baratayuda.abimanyu.a08.com

<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/4e253632-918f-44e6-98a9-1f0719eacfb7">

namun karena confignya juga sudah kami setting untuk bisa menyelesaikan nomor 19 (redirect ke www.abimanyu.a08.com) maka yang tampil adalah tampilan index www.abimanyu.a08.com

## Soal 18
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

- untuk menyelesaikan soal nomor 18, kita dapat menambahkan command berikut ini untuk memberikan authentifikasi pada saat mengkases rjp.abimnanyu.a08.com di file config rjp.baratayuda.abimanyu.a08

```
<Directory /var/www/rjp.baratayuda.abimanyu.a08>
		AuthType Basic
		AuthName "Restricted Content"
		AuthUserFile /etc/apache2/.htpasswd
		Require valid-user 
</Directory>
```

- lalu tambahkan juga line ini untuk membuat sebuah username dan password yang harus dimasukkan saat akan mengakses rjp.baratayuda

```
htpasswd -c -b /etc/apache2/.htpasswd Wayang baratayudaa08
```

Sehingga untuk menjawab soal 17 dan 18 kita bisa menuliskan perintah dibawah ini dan simpan dalam file bernama `no17.sh`

```
mkdir /var/www/rjp.baratayuda.abimanyu.a08

touch /etc/apache2/sites-available/rjp.baratayuda.abimanyu.a08.com.conf

echo '<VirtualHost *:14000 *:14400>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/rjp.baratayuda.abimanyu.a08
	ServerName rjp.baratayuda.abimanyu.a08.com
	ServerAlias www.rjp.baratayuda.abimanyu.a08.com

	<Directory /var/www/rjp.baratayuda.abimanyu.a08>
		AuthType Basic
		AuthName "Restricted Content"
		AuthUserFile /etc/apache2/.htpasswd
		Require valid-user 
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/rjp.baratayuda.abimanyu.a08.com.conf

htpasswd -c -b /etc/apache2/.htpasswd Wayang baratayudaa08

a2enmod rewrite rjp.baratayuda.abimanyu.a08.com.conf
a2ensite rjp.baratayuda.abimanyu.a08.com.conf
service apache2 restart

echo '# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80
Listen 14000
Listen 14400

<IfModule ssl_module>
	Listen 443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet' > /etc/apache2/ports.conf

wget 'https://drive.usercontent.google.com/download?id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6&export=download&authuser=0&confirm=t&uuid=c039943d-843b-47b9-8910-4b46edf58596&at=APZUnTWSNiEJdlQx1mlaCEI-F6-B:1696957565777'
unzip -j /var/www/rjp.baratayuda.abimanyu.a08/rjp_baratayuda_abimanyu.zip -d /var/www/rjp.baratayuda.abimanyu.a08

rm -rf /var/www/rjp.baratayuda.abimanyu.a08/rjp_baratayuda_abimanyu.zip

```

- lalu tes di client apakah authentifikasi yang kita terapkan sudah masuk atau belum

<img width="866" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/e4db30f3-13da-4604-8208-6c04fcc6f9c7">

<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/7aa1fddd-8006-4b6d-b35e-16f20396af1e">

<img width="868" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/2f3deac3-79fc-4639-883c-37beacc5f99f">

## Soal 19
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

* Pada Abimanyu Web Server, ketikkan syntax dibawah ini dan simpan dalam file bernama `no19.sh`

```
echo '<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	Redirect / http://www.abimanyu.a08.com/
</VirtualHost>' > /etc/apache2/sites-available/000-default.conf

service apache2 restart
```

- lalu tes di client apakah akan ke redirect apabila kita mengakses ip abimanyu

<img width="704" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/781ad20c-69f8-45f6-a25e-fee185c8b7c4">

<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/e1a47ee5-58cc-447d-becf-96564351068f">

## Soal 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

- untuk menyelesaikan soal ini, kita hanya perlu me-rewrite modul dari parikesit untuk me-redirect user ke abimanyu.png apabila user request gambar dengan kata abimanyu menggunakan module rewrite di file config parikesit seperti berikut:

```
<Directory /var/www/parikesit.abimanyu.a08>
		Options +FollowSymLinks -Multiviews
		AllowOverride All
</Directory>

RewriteEngine On
RewriteCond %{REQUEST_URI} abimanyu
RewriteCond %{REQUEST_URI} !^/public/images/abimanyu\.png
RewriteRule ^(.+)\.(png|jpg|jpeg|webp|gif)$ /abimanyu.png [L,R=301]
```

- kemudian jangan lupa untuk menambahkan command berikut untuk dapat mengaplikasikan hasil rewrite modul yang telah kita tambahkan :

```
a2enmod rewrite parikesit.abimanyu.a08.com.conf
```

- lalu tes di client apakah modul rewrite kita sudah berjalan atau belum

<img width="866" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-2-A08-2023/assets/105977864/14feb060-88df-4a13-ae79-c034c09eb0e1">