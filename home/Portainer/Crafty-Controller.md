# Crafty Controller

```jsx
version: '3'

services:
  crafty:
    container_name: crafty_container
    image: registry.gitlab.com/crafty-controller/crafty-4:latest
    restart: always
    environment:
        - TZ=America/New_York
    ports:
        - "8000:8000" # HTTP
        - "8443:8443" # HTTPS
        - "8123:8123" # DYNMAP
        - "19132:19132/udp" # BEDROCK
        - "25500-25600:25500-25600" # MC SERV PORT RANGE
    volumes:
        - '/servers/crafty/backups:/crafty/backups'
        - '/servers/crafty/logs:/crafty/logs'
        - '/servers/crafty/servers:/crafty/servers'
        - '/servers/crafty/config:/crafty/app/config'
        - '/servers/crafty/import:/crafty/import'
```