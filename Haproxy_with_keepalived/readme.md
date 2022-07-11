#  haproxy dan keepalived

## Topologi
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/topologi%20haproxy%20with%20keepalived.png)

## Konfigurasi:
### Load Balancing 1
> Install package keepalived dan haproxy
```bash
apt install keepalived haproxy –y
```
 
> Konfigurasi Keep alived
```bash
nano /etc/keepalived/keepalived.conf
 
vrrp_script chk_haproxy {
        script "/usr/bin/kill -0 haproxy"
        interval 2
        weight 2
        timeout 2
        fall 2
	rise 2
}
 
vrrp_instance VI_1 {
        state MASTER
        interface enp0s3
        virtual_router_id 51
        priority 101
        advert_int 1
        authentication {
        auth_type PASS
        auth_pass 1111
        }
 
 
virtual_ipaddress {
        192.168.10.5 dev enp0s3
}
        track_script {
                chk_haproxy
}
}
 
vrrp_instance VI_2 {
        state MASTER
        interface enp0s8
        virtual_router_id 52
        priority 101
        advert_int 1
        authentication {
        auth_type PASS
        auth_pass 1111
        }
 
virtual_ipaddress {
        10.10.10.2 dev enp0s8
}
track_script {
	chk_haproxy
}
}
```
> Mengaktifkan Keepalived
```bash
systemctl restart keepalived.service
```
 

> Konfigurasi Haproxy
```
nano /etc/haproxy/haproxy.cfg

global
      	log     127.0.0.1 local2 info
        chroot  /var/lib/haproxy
        pidfile /var/run/haproxy.pid
        maxconn 256
        user    haproxy
        group   haproxy
        daemon
defaults
        mode    http
        option  forwardfor
        option  http-server-close
        log     global
        option  httplog
        timeout connect 10s
        timeout client  30s
        timeout server  30s
frontend http-in
        bind *:80
        mode http
        reqadd X-Forwarded-Proto:\ http
        default_backend rcn_servers
        option  forwardfor
        stats   enable
        stats   auth    admin:bujangan
        stats   hide-version
        stats   show-node
        stats refresh 30s
        stats uri /haproxy?stats
backend rcn_servers
        mode http
        balance roundrobin
        server  web-srv01 10.10.10.9:80 check #server backend 1
        server  web-srv02 10.10.10.10:80 check #server backend 2
```
> Mengaktifkan Haproxy
```bash
systemctl restart haproxy
```

### Load Balancing 2
> Install keepalived dan Haproxy
```bash
apt install keepalived haproxy –y
```
 

> Konfigurasi Keepalived
```
nano /etc/keepalived/keepalived.conf

vrrp_script chk_haproxy {
        script "/usr/bin/kill -0 haproxy"
        interval 2
        weight 2
        timeout 2
        fall 2
	rise 2
}
 
vrrp_instance VI_1 {
        state BACKUP
        interface enp0s3
        virtual_router_id 51
        priority 100
        advert_int 1
        authentication {
        auth_type PASS
        auth_pass 1111
        }
 
 
 
virtual_ipaddress {
        192.168.10.5 dev enp0s3
}
        track_script {
                chk_haproxy
}
}
 
vrrp_instance VI_2 {
        state BACKUP
        interface enp0s8
        virtual_router_id 52
        priority 100
        advert_int 1
        authentication {
        auth_type PASS
        auth_pass 1111
        }
 
virtual_ipaddress {
        10.10.10.2 dev enp0s8
}
track_script {
	chk_haproxy
}
}
```
> Mengaktifkan Keepalived
```bash
systemctl restart keepalived.service
```

> Konfigurasi Haproxy
```
nano /etc/haproxy/haproxy.cfg

global
      	log     127.0.0.1 local2 info
        chroot  /var/lib/haproxy
        pidfile /var/run/haproxy.pid
        maxconn 256
        user    haproxy
        group   haproxy
        daemon
defaults
        mode    http
        option  forwardfor
        option  http-server-close
        log     global
        option  httplog
        timeout connect 10s
        timeout client  30s
        timeout server  30s
frontend http-in
        bind *:80
        mode http
        reqadd X-Forwarded-Proto:\ http
        default_backend rcn_servers
        option  forwardfor
        stats   enable
        stats   auth    admin:bujangan
        stats   hide-version
        stats   show-node
        stats refresh 30s
        stats uri /haproxy?stats
backend rcn_servers
        mode http
        balance roundrobin
        server  web-srv01 10.10.10.9:80 check #server backend 1
        server  web-srv02 10.10.10.10:80 check #server backend 2
```
> Mengaktifkan Haproxy
```
systemctl restart haproxy
```
 

## Pengujian
### kita cek ip di load balancer 1  pastikan ada virtual ip nya

```
Ip a
```
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/ipalb1.png)
 

### Setelah itu kita cek apakah haproxy nya sudah berjalan kita cek menggunakan curl ipvirtual atau ip asli di Load balance 1
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/curllb1.png)
 
### Setelah itu kita coba matikan server load balancing 1 dan lakukan command ip a di load balancing 2 maka vip yang kita buat akan pindah ke load balancing 2
 ![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/ipalb2.png)

### Setelah itu kita coba cek curl menggunakan ip virtual kembali di load balancing 2
![gambar](https://github.com/muhammad-soleh/Konfigurasi_Jaringan/blob/main/images/curllb2.png)
 

### Dan  jika muncul maka berhasil 
[Contribution guidelines for this project](docs/CONTRIBUTING.md)
## Manfaat keepalived dan haproxy pada pengujian kali ini :
```
Mengetahui cara kerja sederhana dari high availability dan load balancing yang dimana tidak hanya beban yang seimbang tetapi ketersediaan secara jangka waktu yang panjang juga sangat diperlukan. Dikarenakan kita tidak akan tahu apa yang akan terjadi kedepannya. 
```
