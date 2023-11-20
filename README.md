# Jarkom-Modul3-IT24-2023

## Anggota Kelompok

- Wisnuyasha Faizal / 5027211036
- I Gusti Agung Bagus Maitreya Satyamurti / 5027211046


### Soal Topologi

### Node Configure
![topo](https://github.com/gunggus665/Jarkom-Modul3-IT24-2023/assets/100693456/4987cb4b-dc38-40fb-9f3d-14d2f006ef3e)
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
![nooo1](https://github.com/gunggus665/Jarkom-Modul3-IT24-2023/assets/100693456/941d1938-a55c-4d1a-9a9f-042da9632a09)

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
![no3](https://github.com/gunggus665/Jarkom-Modul3-IT24-2023/assets/100693456/5f2822d8-a42f-4bae-8af8-0d32cb52ef85)

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
![seindhcp](https://github.com/gunggus665/Jarkom-Modul3-IT24-2023/assets/100693456/6cae0134-796c-4bfa-b8c7-b30f866caea3)
![revoltedhcp](https://github.com/gunggus665/Jarkom-Modul3-IT24-2023/assets/100693456/90a7bb25-466b-44ea-b9ac-d90ed18409f7)

## Soal 13
> Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)

### Script config
```sh
echo '[client-server]

!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf
```

Ganti bind-address pada file /etc/mysql/mariadb.conf.d/50-server.cnf jadi 0.0.0.0
```
bind-address            = 0.0.0.0
```

Restart mysql
```
service mysql restart
```

Menggunakan sql mariadb
```sh
mysql

CREATE USER 'kelompokit24'@'%' IDENTIFIED BY 'passwordit24';
CREATE USER 'kelompokit24'@'localhost' IDENTIFIED BY 'passwordit24';
CREATE DATABASE dbkelompokit24;
GRANT ALL PRIVILEGES ON *.* TO 'kelompokit24'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompokit24'@'localhost';
FLUSH PRIVILEGES;
```

### Result
Cek `dbkelompokit24` menggunakan :
```
mariadb --host=192.245.2.2 --port=3306 --user=kelompokit24 --password=passwordit24 dbkelompokit24 -e "SHOW DATABASES;"
```

### Screenshot
![no13](https://github.com/wisnuyasha/Jarkom-Modul-3-IT24-2023/assets/100693456/63dc5c48-be7b-4853-a1ae-c29d4c747465)
![noo13](https://github.com/wisnuyasha/Jarkom-Modul-3-IT24-2023/assets/100693456/f119a8df-fcaa-4625-95d0-23698b2b23ca)

## Soal 14
> Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Compose

### Script Config
```sh
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install lynx mariadb-client git lsb-release ca-certificates apt-transport-https software-properties-common gnupg2 git -y

curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget nginx -y

service php8.0-fpm start
service nginx restart

echo 'nameserver 192.245.1.3' > /etc/resolv.conf

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer

cd /var/www/
git clone https://github.com/martuafernando/laravel-praktikum-jarkom.git
cd /var/www/laravel-praktikum-jarkom
composer update
```
melakukan instalasi package yang diperlukan dan clone.

```bash
echo '
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.245.2.2
DB_PORT=3306
DB_DATABASE=dbkelompokit24
DB_USERNAME=kelompokit24
DB_PASSWORD=passwordit24

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
' > /var/www/laravel-praktikum-jarkom/.env

cd /var/www/laravel-praktikum-jarkom && php artisan migrate:fresh
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed --class=AiringsTableSeeder
cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret

chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
```
melakukan konfigurasi laravel pada masing-masing worker

```bash
echo 'server {
    listen 8002;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}' > /etc/nginx/sites-available/laravel-worker

ln -s /etc/nginx/sites-available/laravel-worker /etc/nginx/sites-enabled/

service nginx restart
```
melakukan konfigurasi nginx pada masingmasing worker (contoh pada port 8002)
```
192.245.4.1:8001; # Frieren 
192.245.4.2:8002; # Flamme
192.245.4.3:8003; # Fern
```
## Result
Lakukan lynx pada salah satu host dengan misal `192.245.4.1` (frieren)
```
lynx 192.245.4.1:8001
```

## Screenshot
![no14](https://github.com/wisnuyasha/Jarkom-Modul-3-IT24-2023/assets/100693456/7433bc5c-3647-4888-828c-57e849bf4d11)
