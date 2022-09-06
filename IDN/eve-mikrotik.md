# Installasi dan konfigurasi  Mikrotik dengan menggunakan EVE-NG

## Bahan-bahan yang perlu diinstall:
1. VMware atau Virtualbox (https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)
2. EVE-NG OVF (https://www.eve-ng.net/index.php/download/)
3. EVE-NG Client Side (https://www.eve-ng.net/index.php/download/)
4. Mikrotik chr img versi bebas disini saya akan memakai versi 6.46.8

## Langkah-Langkah Installasi dan Konfigurasi awal:
#### Kita extract terlebih dahulu file OVF EVE-NG nya yang sudah kita download
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(1).png)

#### Lalu selanjutnya kita klik kanan pada file yang type nya Open Virtualization format package => open with => VMware Workstation. Lalu kita tunggu saja.
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(2).png)

#### Nanti akan muncul seperti dibawah ini. disini kita ganti nama dari virtual machine nya sesuai keinginan dan klik import
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(3).png)

#### Setelah eve-ng berhasil di import, langkah selanjutnya ialah kita settings dahulu beberapa spesifikasi yang dipakai oleh vm Eve-NG:
###### 1. Klik Edit virtual machine settings.
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(4).png)
###### 2. Pada Tab Memory kita set ke rekomendasi saja 
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(5).png)
###### 3. Lalu kita pundah ke Tab Processor lalu set number of processor nya ke 2
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(6).png)
###### 4. Pada tab Network Adapter disini bisa pilih bridge namun terkadang ada masalah yang muncul jadi saya lebih memilih costum vmnet0 yang dimana mengarah ke jaringan wireless.
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(7).png)
###### 5. Klik oke 

####### ---------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Setelah kita setting maka langkah selanjutnya ialah kita klik saja Power on this virtual machine.
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(8).png)
#### Setelah muncul seperti dibawah ini kita login saja dengan username root dan password eve
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/eve-mik/img%20(9).png)
Nanti nya akan muncul pop up yang dimana ini adalah langkah awal.
Type the Root Password (disini masukkan password bebas yang penting ingat).
Repeat the root Password (disni juga masukkan password yang sama dengan yang tadi)
Type the short hostname for the system: (Disini masukkan hostname bebas)
Type the DNS Domain name for the system: (disini juga bebas dimasukkan apa saja)
Use DHCP/Static (disini pilih dhcp saja)
NTP server (disini kita masukkan id.pool.ntp.org untuk menyinkronisasikan waktu indonesia)
Proxy Server Configuration (disini pilih direct connection saja)
Lalu ditunggu saja sampai reboot dan muncul seperti diawal

Setelah itu maka kita bisa login menggunakan password yang kita sudah buat tadi dan kita cek ip address untuk pnet0 yang terhubung dengan bridge tadi.
Dan kita copy ip tersebut dan buka di web browser client (chrome/firefox).Jika muncul seperti gambar dibawah ini maka sudah selesai instalasi dan konfigurasi awal EVE-NG.

> Konfigurasi mikrotik di EVE-NG
Pertama kita pastikan dahulu sudah terdapat file img dari mikrotik chr versi 6.46.8
Lalu kita extract file img nya yang sudah didownload
Jika sudah maka kita lanjut import ke vm EVE-NG nya menggunakan winscp dengan:
Host Name diisi ip address EVE-NG nya 
Port default saja
User Name diisi dengan root
Password diisi dengan password yang tadi kita sudah buat
lalu klik login, jika muncul pop out klik yes saja
Lalu kita cari folder letak penyimpanan file img mikrotiknya dan juga kita pindah kan file image nya ke /opt/unetlab/addons/qemu/dan kita buat folder dengan mengklik F7 pada keyboard dan masukan nama foldernya. cara memindahkannya dengan drag and drop aja ya
Setelah kita pindahkan file imgnya kita ubah nama file nya dengan klik F2 pada keyboard dan ganti namanya menjadi hda.qcow2
Selanjutnya kita buka lagi eve-ng nya di vmware dan kita masuk ke folder nya dengan command cd /opt/unetlab/addons/qemu/mikrotik-6.46.8/
Lalu kita ketikkan command /opt/unetlab/wrappers/unl_wrapper -a fixpermissions
maka selesai sudah kita mengimport mikrotik ke eve-ng, selanjutnya kita akan mencoba menggunakan mikrotiknya


> Membuat lab, menambahkan mikrotik dan meremote nya menggunakan winbox di client
Kita buka web yang tadi menggunakan ip address, setelah itu kita login menggunakan credentials:
username : admin
password : eve
Lalu kita klik icon document atau Add new lab
Setelah itu kita isi nama dan authornya, lalu kita klik save
Lalu kita klik icon + atau Add an object lalu pilih Node
Lalu kita cari mikrotik maka akan muncul MIkrotik Router OS, klik saja.
Lalu disini kita bisa ganti nama dan pastikan template nya itu Mikrotik RouterOS.dan klik save
Maka akan ada satu buah mikrotik selanjutnya kita klik lagi tanda + atau Add an object dan selanjutnya tambahkan network
Lalu ganti nama nya bebas dan type nya management(cloud). dan klik save
Maka akan ada satu buah bentuk awan 
Selanjutnya drag and drop dari awan(cloud) ke mikrotik dan chose interface pastikan pilih eth1 dan klik save
Selanjutnya kita klik more actions dan klik start all nodes
Lalu klik pada router nya maka akan terbuka tab baru yang berisi console dari mikrotik
Disini kita login dengan user admin dan password kosong
Lalu kita check apakah mikrotik sudah mendapatkan ip dhcp client dengan command ip dhcp-client print
Jika masih searching kemungkinan salah interface seharusny kita set dhcp-client nya di ether. kita ubah menggunakan command ip dhcp-client set numbers=0 interface=ether1 dan kita cek lagi apakah sudah terganti dan sudah mendapatkan ip nya
JIka sudah mendapatkan ip dhcp-client selanjutnya kita check di winbox
jika disini kita refresh tetep tidak muncul kita masukkan saja ip dhcp-client yang tadi dan klik connect
Tada maka sudah berhasil kita remote menggunakan winbox.

