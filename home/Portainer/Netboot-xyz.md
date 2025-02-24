# Netboot.xyz

```jsx
version: "3"
services:
  netbootxyz:
    image: lscr.io/linuxserver/netbootxyz:latest
    container_name: netbootxyz
    environment:
      - PUID=1000 #current user
      - PGID=1000 #current group
      - TZ=America/New_York
      # - MENU_VERSION=1.9.9 #optional, sets menus version, unset uses latest
      - PORT_RANGE=30000:30010 #optional
      - SUBFOLDER=/ #optional
    volumes:
      - /servers/netboot_xyz/config:/config
      - /servers/netboot_xyz/assets:/assets #optional
    networks:
      - proxy
    ports:
      - 3000:3000
      - 69:69/udp
      - 8080:80 #optional
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.netbootxyz.entrypoints=http"
      - "traefik.http.routers.netbootxyz.rule=Host(`netbootxyz.local.vpena.net`)"
      - "traefik.http.middlewares.netbootxyz-https-redirect.redirectscheme.scheme=https"
      - "traefik.http.routers.netbootxyz.middlewares=netbootxyz-https-redirect"
      - "traefik.http.routers.netbootxyz-secure.entrypoints=https"
      - "traefik.http.routers.netbootxyz-secure.rule=Host(`netbootxyz.local.vpena.net`)"
      - "traefik.http.routers.netbootxyz-secure.tls=true"
      - "traefik.http.routers.netbootxyz-secure.service=netbootxyz"
      - "traefik.http.services.netbootxyz.loadbalancer.server.port=3000"
      - "traefik.docker.network=proxy"

networks:
  proxy:
    external: true
```