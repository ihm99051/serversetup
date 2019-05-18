1) Install cloudflared

```
curl https://bin.equinox.io/c/VdrWdbjqyF/cloudflared-stable-darwin-amd64.tgz | tar xzC /usr/local/bin
```

or installed via Homebrew

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install cloudflare/cloudflare/cloudflared
```

2) Create /usr/local/etc/cloudflared/config.yaml, with the following content

```
proxy-dns: true
proxy-dns-upstream:
  - https://1.1.1.1/dns-query
  - https://1.0.0.1/dns-query
```

3) Activate cloudflared as a service

```
sudo cloudflared service install
```

4) Test
```
dig +short @127.0.0.1 github.com AA
```
5) If OK, change DNS on your mac to 127.0.0.1 (System Preferences->Network->Advanced->DNS)
