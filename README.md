# unbound-docker

This docker image contains unbound and can be used as the upstream dns resolver in adguardhome.

```yml
services:
  adguardhome:
    container_name: adguardhome
    image: adguard/adguardhome
    restart: always
    network_mode: host
    environment:
      - "TZ=Europe/Berlin"
    volumes:
      - "/opt/adguardhome/data:/opt/adguardhome/work/data"
      - "/opt/adguardhome/conf:/opt/adguardhome/conf"
      - "/etc/ssl/fullchain-dns.pem:/etc/ssl/fullchain-dns.pem:ro"
      - "/etc/ssl/privkey-dns.pem:/etc/ssl/privkey-dns.pem:ro"
  unbound:
    container_name: adguardhome-unbound
    image: zoeyvid/unbound-docker
    restart: always
    network_mode: bridge
    ports:
      - "127.0.0.1:153:53/tcp"
      - "127.0.0.1:153:53/udp"
    environment:
      - "TZ=Europe/Berlin"
```

Then use only `127.0.0.1:153` as your upstream server in AdGuard Home!
