# Nginx

```jsx
version: '3'

services:
  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "2095:80"
    volumes:
      - /servers/nginx/html:/usr/share/nginx/html
    restart: always
```