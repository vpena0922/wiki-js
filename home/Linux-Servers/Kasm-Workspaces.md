# Kasm Workspaces

[kasm_configuration_export.zip](Kasm%20Workspaces%2075f75f5b468d44ba8027a84ee6873f49/kasm_configuration_export.zip)

# **Single Server Installation**

The simplest way to deploy Kasm Workspaces is to install all application services on a single server. End-user sessions will also be provisioned on this server. All interior [docker](https://kasmweb.com/docs/latest/glossary.html#term-Docker) communication occurs within the single server and there are no special configurations required.

***Single Server Architecture***

![https://kasmweb.com/docs/latest/_images/single_install2.png](https://kasmweb.com/docs/latest/_images/single_install2.png)

- **Ports and Protocols**
    
    
    |  |  |  |  |
    | --- | --- | --- | --- |
    |  |  |  |  |

# **Installation Guide**

**Standard Install**

```bash
cd /tmp
curl -O https://kasm-static-content.s3.amazonaws.com/kasm_release_1.15.0.06fdc8.tar.gz
tar -xf kasm_release_1.15.0.06fdc8.tar.gz
sudo bash kasm_release/install.sh
```

- Log into the Web Application running on port 443 at **https://<WEBAPP_SERVER>**
- The Default usernames are **admin@kasm.local** and **user@kasm.local**. The passwords will be randomly generated and presented at the end of the install unless the `-admin-password` or/and `-user-password` are specified.

```yaml
Installation Complete

Kasm UI Login Credentials

------------------------------------
  username: admin@kasm.local
  password: 3Ze7Pyv9mNe9M
------------------------------------
  username: user@kasm.local
  password: Oney5tpvNAqnC
------------------------------------

Kasm Database Credentials
------------------------------------
  username: kasmapp
  password: PYJS4uOIEL0U4BgULGpj
------------------------------------

Kasm Redis Credentials
------------------------------------
  password: nZFv8Neyh9pAcLLI9jPn
------------------------------------

Kasm Manager Token
------------------------------------
  password: 5YFaAC0p7R1k7vqO4LkP
------------------------------------

Kasm Guac Token
------------------------------------
  password: pilBRDhAPuQfDL4lmVgs4q
------------------------------------

Service Registration Token
------------------------------------
  password: FIqkZUQG0JeZGcRJjxXg
------------------------------------
```