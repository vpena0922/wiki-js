# Shlink

```yaml
version: "3"

services:
  shlink:
    image: shlinkio/shlink:stable
    restart: always
    container_name: shlink
    environment:
      - TZ="America/New_York"
      - DEFAULT_DOMAIN=link.vpena.net #no http/https. no trailing slash
      - IS_HTTPS_ENABLED=true
      - GEOLITE_LICENSE_KEY=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 #we'll need to get this key from maxmind.com
      - DB_DRIVER=maria
      - DB_USER=shlink
      - DB_NAME=shlink
      - DB_PASSWORD=Vict0r2209 #change this
      - DB_HOST=database
    depends_on:
      - database
    ports:
      - 8180:8080

  database:
    image: mariadb:latest
    restart: always
    container_name: shlink-db
    environment:
      - MARIADB_ROOT_PASSWORD=d0n0tu$3+h1s #change this
      - MARIADB_DATABASE=shlink
      - MARIADB_USER=shlink
      - MARIADB_PASSWORD=Vict0r2209 #change this
    volumes:
      - /servers/shlink:/var/lib/mysql

  shlink-web-client:
    image: shlinkio/shlink-web-client
    restart: always
    container_name: shlink-web
    # volumes:
    #   - /servers/shlink/servers.json:/usr/share/nginx/html/servers.json #this file will be generated automatically
    depends_on:
      - shlink
    ports:
      - 8181:8080
```