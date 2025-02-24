# Homepage

```jsx
version: "3.3"
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    networks:
      - proxy
    ports:
      - 3002:3000
    volumes:
      - /servers/homepage/config:/app/config # Make sure your local config directory exists
      - /var/run/docker.sock:/var/run/docker.sock # (optional) For docker integrations
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.homepage.entrypoints=http"
      - "traefik.http.routers.homepage.rule=Host(`homepage.local.vpena.net`)"
      - "traefik.http.middlewares.homepage-https-redirect.redirectscheme.scheme=https"
      - "traefik.http.routers.homepage.middlewares=homepage-https-redirect"
      - "traefik.http.routers.homepage-secure.entrypoints=https"
      - "traefik.http.routers.homepage-secure.rule=Host(`homepage.local.vpena.net`)"
      - "traefik.http.routers.homepage-secure.tls=true"
      - "traefik.http.routers.homepage-secure.service=homepage"
      - "traefik.http.services.homepage.loadbalancer.server.port=3000"
      - "traefik.docker.network=proxy"

networks:
  proxy:
    external: true
```