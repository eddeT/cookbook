# Docker ntfy
Source: https://docs.ntfy.sh/install/#docker


```yaml
services:
  ntfy:
    image: binwiederhier/ntfy
    container_name: ntfy
    command:
      - serve
    environment:
      NTFY_BASE_URL: https://<hostname>:55000 # does not work with port
      NTFY_CACHE_FILE: /var/lib/ntfy/cache.db
      NTFY_AUTH_FILE: /var/lib/ntfy/auth.db
      NTFY_AUTH_DEFAULT_ACCESS: deny-all
      NTFY_ATTACHMENT_CACHE_DIR: /var/lib/ntfy/attachments
      NTFY_ENABLE_LOGIN: true
      NTFY_BEHIND_PROXY: true
      TZ: Europe/Berlin
    user: 1000:1000
    volumes:
      - ./:/var/lib/ntfy
    ports:
      - 80:80
    restart: unless-stopped
```
