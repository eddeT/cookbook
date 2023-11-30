# docker-pihole
source: https://github.com/pi-hole/docker-pi-hole

```yaml
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:a
      - "53:53/tcp"
      - "53:53/udp"
      - "8443:80/tcp"
    environment:
      TZ: 'Europe/Stockholm'
      WEBPASSWORD: '<password>'
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    restart: unless-stopped

```