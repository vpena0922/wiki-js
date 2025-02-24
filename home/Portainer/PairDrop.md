# PairDrop

```jsx
version: '3.3'
services:
    crafty:
        image: linuxserver/pairdrop
        container_name: pairdrop
        restart: always
        stdin_open: true # docker run -i
        tty: true        # docker run -t
        networks:
            - proxy
        ports:
            - '3001:3000/tcp'
        environment:
            - PUID=1000
            - PGID=1000
            - TZ=America/New_York
        labels:
            - "traefik.enable=true"
            - "traefik.http.routers.pairdrop.entrypoints=http"
            - "traefik.http.routers.pairdrop.rule=Host(`pairdrop.local.vpena.net`)"
            - "traefik.http.middlewares.pairdrop-https-redirect.redirectscheme.scheme=https"
            - "traefik.http.routers.pairdrop.middlewares=pairdrop-https-redirect"
            - "traefik.http.routers.pairdrop-secure.entrypoints=https"
            - "traefik.http.routers.pairdrop-secure.rule=Host(`pairdrop.local.vpena.net`)"
            - "traefik.http.routers.pairdrop-secure.tls=true"
            - "traefik.http.routers.pairdrop-secure.service=pairdrop"
            - "traefik.http.services.pairdrop.loadbalancer.server.port=3000"
            - "traefik.docker.network=proxy"

networks:
  proxy:
    external: true
```