# Guacamole

### **Docker image pull**

First of all lets download the image from docker-hub, by the time this guide is written the latest tag are 1.4.0 and I will be using MariaDB instead of Postgres.

```bash
docker pull guacamole/guacamole:1.4.0
docker pull guacamole/guacd:1.4.0
docker pull mariadb:10.9.5
```

### **Grab the latest sql info from the latest images**

Run this command to retrive the db.sql

```bash
docker run --rm guacamole/guacamole:1.4.0 /opt/guacamole/bin/initdb.sh --mysql > initdb.sql
```

### **Making initial DB docker-compose.yml**

using any linux editor - in my case using nano create a new docker-compose.yml.

```yaml
version: '3'
services:
  guacdb:
    container_name: guacamoledb
    image: mariadb:10.9.5
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'MariaDBRootPass'
      MYSQL_DATABASE: 'guacamole_db'
      MYSQL_USER: 'guacamole_user'
      MYSQL_PASSWORD: 'MariaDBUserPass'
    volumes:
      - './db-data:/var/lib/mysql'
volumes:
  db-data:
```

after saving the files please run `docker-compose up -d`

Next you need to copy the SQL file into the docker container

```bash
docker cp initdb.sql guacamoledb:/initdb.sql
```

Last but not least begin to input it to the DB by running this:

```bash
docker exec -it guacamoledb bash
cat /initdb.sql | mysql -u root -p guacamole_db
exit
```

and now time for your to turn off the DB by running `docker-compose down`

### **Complete the docker-compose.yml with all the necessary image**

Now your best practice is to backup your docker-compose files by typing `cp docker-compose.yml docker-compose.yml.bak` Next you will edit and add more detail

```yaml
version: '3'
services:
  guacdb:
    container_name: guacamoledb
    image: mariadb
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'MariaDBRootPass'
      MYSQL_DATABASE: 'guacamole_db'
      MYSQL_USER: 'guacamole_user'
      MYSQL_PASSWORD: 'MariaDBUserPass'
    volumes:
      - '/servers/guacamole/db-data:/var/lib/mysql'
  guacd:
    container_name: guacd
    image: guacamole/guacd
    restart: unless-stopped
  guacamole:
    container_name: guacamole
    image: guacamole/guacamole
    restart: unless-stopped
    ports:
      - 8080:8080
    environment:
      GUACD_HOSTNAME: "guacd"
      MYSQL_HOSTNAME: "guacdb"
      MYSQL_DATABASE: "guacamole_db"
      MYSQL_USER: "guacamole_user"
      MYSQL_PASSWORD: "MariaDBUserPass"
      LDAP_HOSTNAME: "192.168.0.51"
      LDAP_PORT: "6389"
      LDAP_USER_BASE_DN: "OU=Lab Users,DC=local,DC=vpena,DC=net"
      LDAP_USERNAME_ATTRIBUTE: "sAMAccountName"
      LDAP_SEARCH_BIND_DN: "CN=LDAP Service,CN=Users,DC=local,DC=vpena,DC=net"
      LDAP_SEARCH_BIND_PASSWORD: "LDAPService"
      WEBAPP_CONTEXT: ROOT
    depends_on:
      - guacdb
      - guacd
volumes:
  db-data:

```

Now you will be able to run `docker-compose up -d` and you should have your Guacamole up and running on your server.

## **How do I access Guacamole?**

Very simple just open your browser and put in your Guacamole IP with port 8080

For example: http://mylocalip.home:8080/guacamole <- guacamole cant be access via root directory, so you will have to add /guacamole.

The original username/password are `guacadmin/guacadmin`

For security reason I include TOTP in my installation guide, so be sure to have your google authenticator ready for scanning. Else make sure you remove `TOTP_ENABLED: "true"` line from your docker-compose.yml file.

## **Steps to Bypass Guacamole Login Using Traefik**

### **1. Enable Basic Authentication Middleware**

Traefik can automatically send authentication headers when forwarding requests.

1. Open your **Traefik configuration** (if using a dynamic config file, e.g., `traefik.yml` or a Docker label setup).
2. Define the middleware to inject Basic Authentication credentials:
    
    **If using Traefik's dynamic config (file-based):**
    
    ```yaml
    http:
      middlewares:
        guacamole-auth:
          headers:
            customRequestHeaders:
              Authorization: "Basic dXNlcm5hbWU6cGFzc3dvcmQ="
    ```
    
    The **Authorization** header is the Base64-encoded value of `username:password`. You can generate it using:
    
    ```
    echo -n "admin:admin123" | base64
    ```
    
    Example output:
    
    ```
    YWRtaW46YWRtaW4xMjM=
    ```
    
    Then use:
    
    ```yaml
    Authorization: "Basic YWRtaW46YWRtaW4xMjM="
    ```
    

---

### **2. Apply Middleware to Your Guacamole Service**

Now, attach the middleware to your Guacamole service.

- **If using Traefik in a Docker-Compose setup, add this label to your Guacamole container:**
    
    ```yaml
    labels:
      - "traefik.http.routers.guacamole.rule=Host(`guacamole.example.com`)"
      - "traefik.http.routers.guacamole.middlewares=guacamole-auth"
    ```
    
- **If using `traefik.yml` (static config file), add:**
    
    ```yaml
    http:
      routers:
        guacamole:
          rule: "Host(`guacamole.example.com`)"
          service: guacamole
          middlewares:
            - "guacamole-auth"
    ```
    

---

### **3. Restart Traefik & Test**

After making these changes:

```
docker-compose down && docker-compose up -d
```

[w/ Okta](Guacamole%20d3245d312b9a42eab98fe91059c87f8e/w%20Okta%201a28c820bc44808dbb8ff21bfed15d49.md)