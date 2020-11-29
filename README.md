# Jarkom_Modul3_Lapres_T15

Nama Anggota: 
  - Widyantari Febiyanti 05311840000030
  - Nabella Desyawulansari 05311840000039

## Membuat Topologi Jaringan
1. Membuat ```topologi.sh``` sesuai ketentuan

![no1](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/topologi.jpg)

2. Mengatur settings **_sysctl_** dengan perintah ```nano /etc/sysctl.conf```. Kemudian hilangkan tanda **(#)** pada bagian ```net.ipv4.ip_forward=1```. 
Gunakan perintah ```sysctl -p``` untuk mengaktifkan perubahan.

![no2](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/surabaya%20ip.png)

3. Mengatur IP untuk setiap UML yang ada dengan perintah ```nano /etc/network/interfaces```

**Surabaya:** 

![no3](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20surabaya.png)

**Malang, Tuban, Mojokerto:**

![no4](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20malang%20tuban%20mojokerto.png)

**Madiun, Banyuwangi, Gresik, Sidoarjo:**

![no5](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20client.png)

4. Restart network dengan perintah ```service networking restart``` disetiap UML. 
5. Cek dengan perintah ```ipconfig```
6. Lalu jalankan perintah ```iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16``` pada router **SURABAYA** agar internet terhubung kejaringan luar. 
7. Install dhcp-server pada **TUBAN** dengan perintah ```apt-get install isc-dhcp-server```
8. Lakukan konfigurasi seperti dibawah: 

![no6](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20dhcp%20server.png)

9. Install dhcp-server pada **SURABAYA** dengan perintah ```apt-get install isc-dhcp-relay```
10. Lakukan konfigurasi seperti dibawah: 

![no7](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20dhcp%20relay.png)

*Keterangan: **10.151.77.172** adalah IP **TUBAN** yang menunjukkan dimana dhcp-servernya berada.*

## Soal 1
Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.

### Jawaban
1. Ketikkan perintah ```nano /etc/network/interfaces``` untuk setiap client (**GRESIK, SIDOARJO, BANYUWANGI, MADIUN**)
2. Lalu tambahkan script seperti dibawah: 

![no8](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20client.png)

## Soal 3,5,6 
- Client pada **Subnet 1** mendapatkan range IP dari **192.168.0.10** sampai **192.168.0.100** dan **192.168.0.110** sampai **192.168.0.200**.
- Client mendapatkan DNS **Malang** dan DNS **202.46.129.2** dari DHCP.
- Client di **Subnet 1** mendapatkan peminjaman alamat IP selama 5 menit.

### Jawaban
1. Pada UML dhcp-server atau **TUBAN** buka file ```nano /etc/dhcp/dhcpd.conf```
2. Ketikkan script seperti dibawah: 

```
subnet 192.168.0.0 netmask 255.255.255.0 { // berisi IP DMZ dan Netmask.
    range 192.168.0.10 192.168.0.100; // berisi range IP yang diinginkan.
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.77.170, 202.46.129.2; //berisi DNS Malang dan DNS DHCP
    default-lease-time 300; //berisi waktu yaitu 5 menit (dalam detik)
    max-lease-time 7200;
```
![no9](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20tuban%201.png)

3. Restart dhcp-server dengan perintah ```service isc-dhcp-server restart```
4. Tes pada client yang berada di subnet 1 yaitu: **GRESIK** dan **SIDOARJO**
5. Ketikkan perintah ```ifconfig``` untuk mengetahui IP pada UML tersebut.
6. Ketikkan perintah ```cat /etc/resolv.conf```

Hasil **GRESIK**

![no10](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/hasil%20gresik.png)

Hasil **SIDOARJO**

![no11](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/hasil%20sidoarjo.png)

## Soal 4, 5, 6
- Client pada **Subnet 3** mendapatkan range IP dari **192.168.1.50** sampai **192.168.1.70**.
- Client mendapatkan DNS **Malang** dan DNS **202.46.129.2** dari DHCP.
- Client di **Subnet 1** mendapatkan peminjaman alamat IP selama 10 menit.

### Jawaban 
1. Pada UML dhcp-server atau **TUBAN** buka file ```nano /etc/dhcp/dhcpd.conf```
2. Ketikkan script seperti dibawah: 

```
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.70; // berisi range IP yang diinginkan.
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 10.151.77.170, 202.46.129.2; //berisi DNS Malang dan DNS DHCP.
    default-lease-time 600; //berisi waktu yaitu 10 menit (600 dalam detik)
    max-lease-time 7200;
```
![no12](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/konfigurasi%20tuban%202.png)

3. Restart dhcp-server dengan perintah ```service isc-dhcp-server restart```
4. Tes pada client yang berada di subnet 3 yaitu: **BANYUWANGI** dan **MADIUN**
5. Ketikkan perintah ```ifconfig``` untuk mengetahui IP pada UML tersebut.
6. Ketikkan perintah ```cat /etc/resolv.conf```

Hasil **BANYUWANGI**

![no13](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/hasil%20banyuwangi.png)

Hasil **MADIUN**

![no14](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/hasil%20madiun.png)



## Soal 7
Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. (7) User autentikasi milik Anri memiliki format:
User : userta_yyy
Password : inipassw0rdta_yyy
Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01

### Jawaban 
1. Install squid `apt-get install squid`
2. Install apache `apt-get install apache2-utils`
3. backup file konfigurasi default squid `mv /etc/squid/squid.conf /etc/squid/squid.conf.bak`
4. buat user `htpasswd -c /etc/squid/passwd userta_t15`
5. input password `inipassw0rdta_t15`
6. buat konfig `nano /etc/squid/squid.conf` dan input #LOGIN USERPASS:

        http_port 8080
        visible_hostname mojokerto
        auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd 
        auth_param basic children 5 
        auth_param basic realm Proxy
        auth_param basic credentialsttl 2 hours berlaku
        auth_param basic casesensitive on 
        acl USERS proxy_auth REQUIRED
        http_access allow USERS 
 
 ![no7](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/squid%20config.png)
 
  ![no7](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/7%20test.png)
  
   ![no7](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/7%20testt.png)
 
 7. service squid restart
 
 
 ## Soal 8 dan 9
 (8) setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja. Maka diluar jam tersebut, Anri tidak dapat mengakses jaringan internet dengan proxy tersebut. Jadwal bimbingan dengan Bu Meguri adalah 
 (9) setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). Agar Anri bisa fokus mengerjakan TA
 
 ### Jawaban 
1. buat dan edit file acl `nano /etc/squid/acl.conf`
input dengan aturan acl 

2. edit file konfigurasi `nano /etc/squid/squid.conf`
menambahkann #TIMEACCESS

![no89](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/squid%20config.png)

## Soal 10 
(10) setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA

### Jawaban 
1. Edit file `nano /etc/squid/squid.conf`
2. tambahkan konfigurasi  acl #REDIRECT

![no10](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/squid%20config.png)

![no10](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/10%20test.png)


## Soal 11
Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, (11) Bu Meguri meminta Anri untuk mengubah error page default squid

### Jawaban 
1. download file `wget 10.151.36.202/ERR_ACCESS_DENIED`
2. copy ke folder error `cp-r nama file /usr/share/squid/errors/English`
3. command semua line pada `nano /etc/squid/squid.conf` tambahin `http_access deny all`
4. exit lalu restart squid `service squid restart`

![no11](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/11%20test.png)

## Soal 12
(12) Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080. 
Keterangan : yyy adalah nama kelompok masing-masing. Contoh: janganlupa-ta.c01.pw

### Jawaban 
1. di uml malang , install bind9 `apt-get install bind9 -y`
2. `nano /etc/bind/named.conf.local` input seperti berikut :

![no12](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/12%20named.conf.local.png)

3. Buat folder jarkom di dalam `nano /etc/bind mkdir /etc/bind/jarkom`
4. `cp /etc/bind/db.local /etc/bind/jarkom/janganlupa-ta.t15.pw`
5. `nano /etc/bind/jarkom/janganlupa-ta.t15.pw`, kemudian ubah settingan mengarah ke ip mojo

![no12](https://github.com/widyantarif/Jarkom_Modul3_Lapres_T15/blob/main/gambar/janganlupa-ta.t15.pw.png)

6. Restart bind9 dengan perintah `service bind9 restart`

