# HomeBridge

```jsx
version: '3'
services:
  homebridge:
    image: homebridge/homebridge
    container_name: homebridge
    restart: always
    stdin_open: true # docker run -i
    tty: true        # docker run -t
    network_mode: host
    environment:
      - HOMEBRIDGE_CONFIG_UI_PORT=8581
    volumes:
      - '/servers/homebridge:/homebridge'
volumes:
  homebridge:
```