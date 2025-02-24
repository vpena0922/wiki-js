# AdGuard Home

```jsx
version: '3.3'
services:
    adguardhome:
        image: adguard/adguardhome
        container_name: adguardhome
        restart: unless-stopped
        stdin_open: true # docker run -i
        tty: true        # docker run -t
        hostname: docker-adguard
        volumes:
            - '/servers/adguard/work:/opt/adguardhome/work'
            - '/servers/adguard/conf:/opt/adguardhome/conf'
        ports:
            - '53:53/tcp'
            - '53:53/udp'
            - '9480:80/tcp'
            - '49443:443/tcp'
            #- '3000:3000/tcp'
            - '853:853/tcp'
            - '784:784/udp'
            - '853:853/udp'
            - '8853:8853/udp'
            - '5443:5443/tcp'
            - '5443:5443/udp'

        security_opt:
            - no-new-privileges:true
        networks:
            - proxy
        labels:
            - "traefik.enable=true"
            - "traefik.http.routers.adguard.entrypoints=http"
            - "traefik.http.routers.adguard.rule=Host(`adguard.local.vpena.net`)"
            - "traefik.http.middlewares.adguard-https-redirect.redirectscheme.scheme=https"
            - "traefik.http.routers.adguard.middlewares=adguard-https-redirect"
            - "traefik.http.routers.adguard-secure.entrypoints=https"
            - "traefik.http.routers.adguard-secure.rule=Host(`adguard.local.vpena.net`)"
            - "traefik.http.routers.adguard-secure.tls=true"
            - "traefik.http.routers.adguard-secure.service=adguard"
            - "traefik.http.services.adguard.loadbalancer.server.port=80"
            - "traefik.docker.network=proxy"

networks:
  proxy:
    external: true
```