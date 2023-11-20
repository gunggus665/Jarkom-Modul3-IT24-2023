# Jarkom-Modul3-IT24-2023

## Anggota Kelompok

- Wisnuyasha Faizal / 5027211036
- I Gusti Agung Bagus Maitreya Satyamurti / 5027211046


### Soal Topologi

### Node Configure

#### Aura (DHCP Relay)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Himmel (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Heiter (DNS Server)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Denken (Database Server)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Eisen (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Frieren (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Flamme (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Fern (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Lawine (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Linie (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Lugner (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 
	netmask 
	gateway 
```
#### Revolte, Richter, Sein, dan Stark (Client)
```
auto eth0
iface eth0 inet dhcp
```

### Router Console Config

Router (Aura) perlu diberikan config iptables, agar semua node yang mengakses ip router bisa mengakses internet
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.245.0.0/16
```
`192.245` adalah ip kelompok IT24

### Soal 1
Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Setelah membuat topolog diatas pada Heiter masukkan script berikut :
```
nameserver="192.168.122.1"

if [ -f /etc/resolv.conf ]; then
    if ! grep -q "nameserver $nameserver" /etc/resolv.conf; then
        echo "nameserver $nameserver" | tee -a /etc/resolv.conf > /dev/null
    fi
fi

apt-get update -y
apt-get install bind9 -y

mkdir /etc/bind/it24

echo 'zone "riegel.canyon.it24.com" {
    type master;
    file "/etc/bind/it24/riegel.canyon.it24.com";
};

zone "granz.channel.it24.com" {
    type master;
    file "/etc/bind/it24/granz.channel.it24.com";
};' > /etc/bind/named.conf.local

echo 'options {
      directory "/var/cache/bind";

      forwarders {
              192.168.122.1;
      };

      // dnssec-validation auto;
      allow-query{any;};
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
}; ' > /etc/bind/named.conf.options


echo '$TTL    604800
@       IN      SOA     riegel.canyon.it24.com. root.riegel.canyon.it24.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.it24.com.
@       IN      A       192.245.4.1
www     IN      CNAME   riegel.canyon.it24.com.' > /etc/bind/it24/riegel.canyon.it24.com

echo '$TTL    604800
@       IN      SOA     granz.channel.it24.com. root.granz.channel.it24.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@           IN      NS      granz.channel.it24.com.
@           IN      A       192.245.3.1
www         IN      CNAME   granz.channel.it24.com.' > /etc/bind/it24/granz.channel.it24.com

service bind9 restart
```

### Result

### Soal 2
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

Masukkan script ini pada Himmel : 
```
echo 'subnet 192.245.1.0 netmask 255.255.255.0 {
}

subnet 192.245.2.0 netmask 255.255.255.0 {
}

subnet 192.245.3.0 netmask 255.255.255.0 {
    range 192.245.3.16 192.245.3.32;
    range 192.245.3.64 192.245.3.80;
    option routers 192.245.3.0;
}' > /etc/dhcp/dhcpd.conf
```

### Result

### Soal 3
Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

Untuk menambah config baru pada Switch4 lakukan command :
```
echo 'subnet 192.245.1.0 netmask 255.255.255.0 {
}

subnet 192.245.2.0 netmask 255.255.255.0 {
}

subnet 192.245.3.0 netmask 255.255.255.0 {
    range 192.245.3.16 192.245.3.32;
    range 192.245.3.64 192.245.3.80;
    option routers 192.245.3.0;
}
subnet 192.245.4.0 netmask 255.255.255.0 {
    range 192.245.4.12 192.245.4.20;
    range 192.245.4.160 192.245.4.168;
    option routers 192.245.4.0;
}' > /etc/dhcp/dhcpd.conf
```

### Result

### Soal 4
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

Tambahkan option broadcast dan domain name pada configurasi :
```
subnet 192.173.3.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.245.3.255;
    option domain-name-servers 192.245.1.3;
    ...
}

subnet 192.173.4.0 netmask 255.255.255.0 {
    option broadcast-address 192.173.4.255;
    option domain-name-servers 192.173.1.3;
} 
```

jangan lupa melakukan restart :

```
service isc-dhcp-server start
```

Pada DHCP Relay kita perlu menjalankan script :
```
echo 'SERVERS="192.245.1.2"

INTERFACES="eth1 eth2 eth3 eth4"

OPTIONS=""' > /etc/default/isc-dhcp-relay

# config pada /etc/sysctl.conf juga untuk enable ip4 forwarding (uncomen syntax forwarding)
echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf

service isc-dhcp-relay start 
```

### Result

### Soal 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

Pada soal 5 kita perlu mengatur config agar dapat mengatur leasing time yang sesuai pada switch3 dan switch 4. Jalankan command berikut pada DHCP Server:
```
echo 'subnet 192.245.1.0 netmask 255.255.255.0 {
}

subnet 192.245.2.0 netmask 255.255.255.0 {
}

subnet 192.245.3.0 netmask 255.255.255.0 {
    range 192.245.3.16 192.245.3.32;
    range 192.245.3.64 192.245.3.80;
    option routers 192.245.3.0;
    option broadcast-address 192.245.3.255;
    option domain-name-servers 192.245.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.245.4.0 netmask 255.255.255.0 {
    range 192.245.4.12 192.245.4.20;
    range 192.245.4.160 192.245.4.168;
    option routers 192.245.4.0;
    option broadcast-address 192.245.4.255;
    option domain-name-servers 192.245.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}
```

### Result





