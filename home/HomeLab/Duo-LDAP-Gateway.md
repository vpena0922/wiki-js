# Duo LDAP Gateway

```yaml
; Complete documentation about the Duo Auth Proxy can be found here:
; https://duo.com/docs/authproxy_reference

; NOTE: After any changes are made to this file the Duo Authentication Proxy
; must be restarted for those changes to take effect.

; MAIN: Include this section to specify global configuration options.
; Reference: https://duo.com/docs/authproxy_reference#main-section
;[main]

; CLIENTS: Include one or more of the following configuration sections.
; To configure more than one client configuration of the same type, append a
; number to the section name (e.g. [ad_client2])

[ad_client]
host=127.0.0.1
service_account_username=_ldapservice
service_account_password=LDAPService
search_dn=dc=local,dc=vpena,DC=net

; SERVERS: Include one or more of the following configuration sections.
; To configure more than one server configuration of the same type, append a
; number to the section name (e.g. radius_server_auto1, radius_server_auto2)

[ldap_server_auto]
client=ad_client
ikey=DIR13A7ZHPDQUWRA3VYV
skey=qfmBtcosY4TneUUZC6fO4CJzkZjue8AStQB0FxVt
api_host=api-f857337b.duosecurity.com
failmode=safe
exempt_primary_bind=false
exempt_ou_1=CN=LDAP Service,CN=Users,DC=local,DC=vpena,DC=net
port=6389

```