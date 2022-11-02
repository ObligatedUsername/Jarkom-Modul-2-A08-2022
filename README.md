# Jarkom-Modul-2-A08-2022

| **No** | **Nama**                   | **NRP**    |
| ------ | -------------------------- | ---------- |
| 1      | Dhani Rizki A. C. T. P.    | 5025201226 |
| 2      | Khariza Azmi Alfajira H.   | 5025201044 |
| 3      | Farros Hilmi Syafei        | 5025201012 |


## **Nomor 1**
**Diminta untuk membuat topology tersebut, beserta konfigurasinya sesuai dengan role masing-masing. WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden.** <br>

Berikut topologi yang kami buat <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1a.png) <br>

Oleh karena itu, masing-masing network configuration dari setiap node adalah sebagai berikut <br>

Ostania <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1b.png) <br>

WISE (DNS Master) <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1d.png) <br>

SSS (Client) <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1e.png) <br>

Garden (Client) <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1f.png) <br>

Berlint (DNS Slave) <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1g.png) <br>

Eden (Web Server) <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1h.png) <br>

Semua Node harus terhubung pada Ostania agar dapat mengakses internet <br>
- Lakukan iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.3.0.0/16 untuk menyambungkan ke internet <br>
- Untuk pengecekan, lakukan ping google.com pada router Ostania <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1j.png) <br>
- Ketikkan echo nameserver 192.168.122.1 > /etc/resolv.conf pada node ubuntu yang lain. berikut salah satu contoh melakukan ping pada client <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.1/1k.png) <br>

## **Nomor 2**
**Membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise** <br>
1. Install bind9 menggunakan command apt-get update dan  apt-get install bind9 -y pada WISE
1. Ubah isi konfigurasi file /etc/bind/named.conf.local menjadi sebagai berikut : <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.2/2a.png)
1. Buat direktori baru di dalam folder bind dengan mkdir /etc/bind/wise
1. Copy file db.local pada /etc/bind ke dalam folder wise yang baru saja dibuat cp /etc/bind/db.local /etc/bind/wise/wise.a08.com
1. Edit file /etc/bind/wise/wise.a08.com <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.2/2b.png)
1. Restart bind9 menggunakan service bind9 restart
1. Arahkan alamat IP pada salah satu client agar menuju ke WISE. disini kami menggunakan node SSS dan mengubah file /etc/resolv.conf menjadi <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.2/2c.png)
1. Untuk pengecekan, lakukan ping wise.a08.com dan ping www.wise.a08.com pada salah satu client. Disini kami menggunakan SSS <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.2/2d.png) <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.2/2e.png)

## **Nomor 3**
**Membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden** <br>
1. Lakukan konfigurasi pada file  /etc/bind/wise/wise.a08.com yang ada pada WISE <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.3/3a.png)
1. Restart bind9
1. Uji coba dengan menggunakan ping eden.wise.a08.com dan ping www.eden.wise.a08.com pada salah satu node client yang IPnya sudah di arahkan ke WISE <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.3/3b.png)

## **Nomor 4**
**Membuat reverse domain untuk domain utama** <br>
1. Lakukan konfigurasi pada file /etc/bind/named.conf.local yang ada pada WISE pada direktori /etc/bind/wise <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.4/4a.png)
1. Buat file /etc/bind/wise/2.3.10.in-addr.arpa pada direktori /etc/bind/wise dan lakukan konfigurasi <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.4/4b.png)
1. Restart bind9
1. Cek dengan melakukan perintah host -t PTR “Alamat IP WISE” pada salah satu node client dan akan muncul seperti ini. Reserve DNS menunjuk ke alamat wise.a08.com. <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.4/4c.png)

## **Nomor 5**
**Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama** <br>
<i>Berlint</i>
1. Lakukan apt-get update dan apt-get install bind9 -y pada Berlint yang akan dijadikan sebagai DNS Slave dari WISE
1. Konfigurasikan fie /etc/bind/named.conf.local yang ada pada Berlint <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.5/5a.png)
1. Restart bind pada Berlint

<i>WISE</i>
1. Konfigurasikan fie /etc/bind/named.conf.local yang ada pada WISE <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.5/5b.png)
1. Restart bind pada WISE

<i>Testing</i>
1. Buka salah satu client, disini kami menggunakan SSS
1. Arahkan nameserver pada file /etc/resolv.conf agar menuju ke WISE dan Berlint <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.5/5c.png)
1. Stop bind pada  WISE dengan perintah service bind9 stop untuk mengecek apakah jika terjadi kerusakan pada WISE akan tetap bisa berfungsi dengan baik karena adanya Berlint sebagai DNS Slave
1. Lakukan ping wise.a08.com pada SSS <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.5/5d.png)
1. Jika berhasil maka DNS Slave berhasil dibuat

## **Nomor 6**
**Buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation** <br>
<i>WISE</i>
1. Edit file /etc/bind/wise/wise.a08.com lalu tambahkan subdomain yang mengarah ke IP Berlint. <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.6/6a.png)

<i>Berlint</i>
1. Konfigurasi file /etc/bind/named.conf.local <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.6/6b.png)
1. Buat file operation.wise.a08.com pada direktori /etc/bind/operation dan lakukan konfigurasi <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.6/6c.png)
1. Restart service bind9

<i>Testing</i>
1. Buka salah satu client, disini kami menggunakan SSS
1. Lakukan ping operation.wise.a08.com dan ping www.operation.wise.a08.com <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.6/6d.png)

## **Nomor 7**
**Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden** <br>
<i>Berlint</i>
1. Konfigurasi file /etc/bind/operation/operation.wise.a08.com <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.7/7a.png)
1. Restart service bind9

<i>Testing</i>
1. Lakukan ping strix.operation.wise.a08.com dan ping www.strix.operation.wise.a08.com <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.7/7b.png)

## **Nomor 8**
**Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com** <br>
<i>WISE</i>
1. Install apache2, php, dan libapache2-mod-php7.0 menggunakan apt-get install
1. Konfigurasi file /etc/apache2/sites-available/000-default.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.8/8a.png)
1. Start service apache2

<i>Testing</i>
1. Install lynx menggunakan apt-get install pada salah satu client, kami akan menggunakan SSS
1. Lakukan lynx wise.a08.com atau www.wise.a08.com <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.8/8b.png)

## **Nomor 9**
**Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home** <br>
<i>WISE</i>
1. Konfigurasi file /etc/apache2/sites-available/000-default.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.9/9a.png)
1. Download file website wise menggunakan transfer.sh, download file zip dari google drive ke komputer, lalu upload ke transfer.sh dan curl melalui node menggunakan link yang diberikan. Jika menggunakan metode ini, lakukan apt-get install ca-certificates sebelum curl dalam node agar dapat akses website dengan protokol HTTPS. Atau juga bisa langsung memindahkan file zip ke folder project
1. Pindahkan file website ke /var/www/wise.a08.com
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx www.wise.a08.com/home, site yang terbuka akan sama dengan jika lynx www.wise.a08.com

## **Nomor 10**
**Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com** <br>
<i>Eden</i>
1. Install apache2, php, dan libapache2-mod-php7.0 menggunakan apt-get install
1. Konfigurasi file /etc/apache2/sites-available <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.10/10a.png)
1. Download file eden.wise menggunakan metode yang sama seperti sebelumnya
1. Pindahkan file eden.wise ke /var/www/eden.wise.a08.com
1. Start service apache2

<i>Testing</i>
1. Lakukan lynx eden.wise.a08.com atau www.eden.wise.a08.com <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.10/10b.png)

## **Nomor 11**
**Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja** <br>
<i>Eden</i>
1. Konfigurasi file /etc/apache2/sites-available/000-default.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.11/11a.png)
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx www.eden.wise.a08.com/public <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.11/11b.png)

## **Nomor 12**
**Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache** <br>
<i>Eden</i>
1. Buat dan konfigurasi file /var/www/eden.wise.a08.com/.htaccess <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.12/12a.png)
1. Konfigurasi file /etc/apache2/sites-available/000-default.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.12/12b.png)
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx www.eden.wise.a08.com/”situs-random”, kami gunakan www.eden.wise.a08.com/private sebagai contoh <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.12/12c.png)

## **Nomor 13**
**Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js** <br>
<i>Eden</i>
1. Konfigurasi file /etc/apache2/sites-available/000-default.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.13/13a.png)
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx www.eden.wise.a08.com/js <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.13/13b.png)

## **Nomor 14**
**Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500** <br>
<i>Eden</i>
1. Buat dan konfigurasi file /etc/apache2/sites-available/001-strix.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.14/14a.png)
1. Konfigurasi file /etc/apache2/ports.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.14/14b.png)
1. Lakukan a2ensite 001-strix.conf
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx www.strix.operation.wise.a08.com:15000 atau :15500 <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.14/14c.png)

## **Nomor 15**
**dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy** <br>
<i>Eden</i>
1. Lakukan htpasswd -c “lokasi file password” Twilight, disini kami meletakan file password di /usr/local/share/apache2/passwords <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.15/15a.png)
1. Konfigurasi /etc/apache2/sites-available/001-strix.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.15/15b.png)
1. Buat dan konfigurasi file /var/www/strix.operation.wise.a08/.htaccess <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.15/15c.png)
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx www.strix.operation.wise.a08.com:15000 atau :15500, kemudian masukan username dan password yang sesuai <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.15/15d.png) <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.15/15e.png)
1. Akan muncul halaman daftar file seperti pada soal sebelumnya

## **Nomor 16**
**dan setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com** <br>
<i>Eden</i>
1. Aktifkan modul rewrite dengan a2enmod rewrite
1. Konfigurasi file /var/www/eden.wise.a08.com/.htaccess <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.16/16a.png)
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx 10.3.3.3
1. lynx akan diredirect menuju www.wise.a08.com dan halaman utama www.wise.a08.com akan muncul

## **Nomor 17**
**Karena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png** <br>
<i>Eden</i>
1. Konfigurasi file /etc/apache2/sites-available/000-default.conf <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.17/17a.png)
1. Restart service apache2

<i>Testing</i>
1. Lakukan lynx www.eden.wise.a08.com/public/images/eden”terserah apa”, di sini kami akan menggunakan eden-test <br>
![alt text](https://github.com/ObligatedUsername/Jarkom-Modul-2-A08-2022/blob/master/assets/No.17/17b.png)
1. Dapat dilihat muncul halaman untuk download file eden.png selama request uri dimulai dengan /public/images/eden

