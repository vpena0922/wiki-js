# w/ Okta

---

### Final Working Config

For reference, here’s your setup that works:

- **Docker Compose**:
    
    ```yaml
    version: '3.9'
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
          WEBAPP_CONTEXT: ROOT
          GUACAMOLE_AUTH_PROVIDER: net.sourceforge.guacamole.auth.saml.SAMLAuthenticationProvider
          SAML_IDP_METADATA_URL: https://dev-20023127.okta.com/app/exkni2un2sFEpK6Mt5d7/sso/saml/metadata
          SAML_ENTITY_ID: "https://rdp.vpena.net"
          SAML_CALLBACK_URL: "https://rdp.vpena.net"
          SAML_STRICT: "false"
          JDBC_USER_REQUIRED: "false"
          LOGBACK_LEVEL: "debug"
        depends_on:
          - guacdb
          - guacd
        volumes:
          - '/servers/guacamole/extensions/guacamole-auth-sso-openid-1.5.5.jar:/etc/guacamole/extensions/guacamole-auth-sso-openid-1.5.5.jar'
          - '/servers/guacamole/extensions/guacamole-auth-sso-saml-1.5.5.jar:/etc/guacamole/extensions/guacamole-auth-sso-saml-1.5.5.jar'
          - '/servers/guacamole/extensions/guacamole-auth-jdbc-mysql-1.5.5.jar:/etc/guacamole/extensions/guacamole-auth-jdbc-mysql-1.5.5.jar'
          - '/servers/guacamole/guacamole.properties:/etc/guacamole/guacamole.properties'
    volumes:
      db-data:
    
    ```
    
- **`/servers/guacamole/guacamole.properties`**:
    
    ```
    # MySQL configuration
    mysql-hostname: guacdb
    mysql-port: 3306
    mysql-database: guacamole_db
    mysql-user: guacamole_user
    mysql-password: MariaDBUserPass
    
    # Guacd configuration
    guacd-hostname: guacd
    guacd-port: 4822
    
    # Authentication (example: simple authentication)
    auth-provider: simple
    simple-user.username: guacadmin
    simple-user.password: guacadmin  # CHANGE THIS PASSWORD!
    
    # Enable for better performance (optional, but recommended)
    enable-websocket-tunnel: true
    
    saml-idp-metadata-url: https://dev-20023127.okta.com/app/dev-20023127_guacamole_1/exkni2un2sFEpK6Mt5d7/sso/saml/metadata
    saml-entity-id: https://rdp.vpena.net
    saml-callback-url: https://rdp.vpena.net/api/ext/saml/callback
    saml-strict: false
    logback-level: trace
    ```
    
- **Okta**:
    - SSO URL: `https://rdp.vpena.net/api/ext/saml/callback`
    - Audience URI: `https://rdp.vpena.net`
    - Name ID Format: `Unspecified`
    - Application Username: `Okta username`

---