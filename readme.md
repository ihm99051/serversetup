# Useful commands
Here are the commands neccessary for setup

# Install SSR
```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

# Install nginx + extras
```
apt update
apt install nginx
apt install vim
apt install curl
apt-get update && sudo apt-get install speedtest-cli
```

# Install Certbot
```
# deb http://deb.debian.org/debian stretch-backports main
# Add to your sources.list (or add a new file with the ".list" extension to /etc/apt/sources.list.d/) You can instead use https when the apt-transport-https package is installed.

apt-get update
apt-get install certbot python-certbot-nginx -t stretch-backports
certbot --nginx
```

Edit conf
```
vim /etc/nginx/sites-available/default
```
Add following lines
```
	location /ray {
        	proxy_redirect off;
        	proxy_pass http://127.0.0.1:64079; #此IP地址和端口需要和v2ray服务器保持一致，
        	proxy_http_version 1.1;
        	proxy_set_header Upgrade $http_upgrade;
        	proxy_set_header Connection "upgrade";
        	proxy_set_header Host $http_host;
	}
```

# Install BBR
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
lsmod | grep bbr
```

# Install V2Ray
```
bash <(curl -L -s https://install.direct/go.sh)
```

# Config V2ray
```
wget --no-check-certificate -O /etc/v2ray/config.json https://raw.githubusercontent.com/ihm99051/serversetup/master/v2ray/config.json
```

# Config Network Interface
```
ls /sys/class/net/
vi /etc/network/interfaces
service networking restart
```

# Sppedtest
```
# STC (Hong Kong, China) [4.86 km]
speedtest-cli --server 1536
# China Mobile,Guangdong (Guangzhou, China) [130.32 km]
speedtest-cli --server 6611
# ChinaTelecom-GZ (Guangzhou, CN) [130.32 km]
speedtest-cli --server 17251
# SmarTone (Hong Kong, China) [4.86 km]
speedtest-cli --server 19036
```

# Restart Services
```
systemctl restart nginx
service v2ray restart
service v2ray status
```

# Install SS-libev
Follow [Shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev) to install
```
wget --no-check-certificate -O /etc/shadowsocks-libev/config.json https://raw.githubusercontent.com/ihm99051/serversetup/master/shadowsocks-libev/config.json
```
Restart service
```
systemctl restart shadowsocks-libev
```
