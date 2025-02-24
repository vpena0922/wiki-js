# Apache HTTP Server

```jsx
version: '3.3'
services:
  apache:
    image: httpd:latest
    container_name: apache
    ports:
    - '2095:80'

    volumes:
    - /servers/apache/website:/usr/local/apache2/htdocs
```