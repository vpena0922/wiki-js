# Channels DVR

```jsx
version: '3.3'
services:
  channels-dvr:
    image: fancybits/channels-dvr:latest
    container_name: channels-dvr
    network_mode: host
    ports:
      - "8089:8089"
    restart: on-failure:10
    devices:
      - /dev/dri:/dev/dri
    volumes:
      - /opt/channels/dvr/config:/channels-dvr
      - /opt/channels/dvr/recordings:/shares/DVR
```