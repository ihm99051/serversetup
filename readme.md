# Useful commands
Here are the commands neccessary for setup

# Install SSR
```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/ihm99051/serversetup/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

# Install SS-libev
Follow [Shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev) to install
```
sudo sh -c 'printf "deb http://deb.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/stretch-backports.list'
sudo apt update
sudo apt -t stretch-backports install shadowsocks-libev simple-obfs
```

using config below
```
wget --no-check-certificate -O /etc/shadowsocks-libev/config.json https://raw.githubusercontent.com/ihm99051/serversetup/master/shadowsocks-libev/config.json
```

# Install nginx + extras
```
apt update
apt install nginx
apt install vim
apt install curl
apt install apt-transport-https
apt-get update && apt-get install speedtest-cli
echo "deb https://deb.debian.org/debian/ unstable main" | sudo tee /etc/apt/sources.list.d/unstable.list
echo "deb http://deb.debian.org/debian stretch-backports main"  | sudo tee /etc/apt/sources.list.d/stretch-backports.list
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
		if ($http_upgrade != "websocket") {
			return 403;
		}
		proxy_redirect off;
  		proxy_pass http://127.0.0.1:14079;
  		proxy_http_version 1.1;
  		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		proxy_set_header Host $http_host;
	}

	location /ss {
		if ($http_upgrade != "websocket") {
			return 403;
		}
		proxy_redirect off;
		proxy_pass http://127.0.0.1:14077;
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

# Speedtest
```
# STC (Hong Kong, China) [4.86 km]
speedtest-cli --server 1536
```

# Restart Services
```
systemctl restart shadowsocks-libev
systemctl restart nginx
service v2ray restart
service v2ray status
```

# Install DNSCrypt-Proxy
```
apt update
apt install -t unstable dnscrypt-proxy
```
Configure dnscrypt-proxy.toml
```
wget --no-check-certificate -O /etc/dnscrypt-proxy/dnscrypt-proxy.toml https://raw.githubusercontent.com/ihm99051/serversetup/master/dnscrypt-proxy/dnscrypt-proxy.toml
```
Restart service
```
cd /etc/dnscrypt-proxy
dnscrypt-proxy -service install
dnscrypt-proxy -service start
service dnscrypt-proxy restart
```
Install for [macOS](https://github.com/jedisct1/dnscrypt-proxy/wiki/Installation-macOS)
