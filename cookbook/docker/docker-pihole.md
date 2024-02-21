# docker-pihole
source: https://github.com/pi-hole/docker-pi-hole

```yaml
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "8443:80/tcp"
    dns:
      - 127.0.0.1
    environment:
      TZ: 'Europe/Stockholm'
      WEBPASSWORD: '<password>'
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    restart: unless-stopped

```