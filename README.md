# Jarkom_Modul3_Lapres_T15

Nama Anggota: 
  - Widyantari Febiyanti 05311840000030
  - Nabella Desyawulansari 05311840000039
  
## Soal7
Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. (7) User autentikasi milik Anri memiliki format:
User : userta_yyy
Password : inipassw0rdta_yyy
Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01

## Jawaban7
1. Install squid `apt-get install squid`
2. Install apache `apt-get install apache2-utils`
3. backup file konfigurasi default squid `mv /etc/squid/squid.conf /etc/squid/squid.conf.bak`
4. buat user `htpasswd -c /etc/squid/passwd userta_t15`
5. input password `inipassw0rdta_t15`
6. buat konfig `nano /etc/squid/squid.conf` dan input :
    `http_port 8080 // http_port 8080 : Port yang digunakan untuk mengakses proxy, dalam kasus ini adalah 8080.
   ` visible_hostname mojokerto // visible_hostname mojokerto : Nama proxy yang akan terlihat oleh user


