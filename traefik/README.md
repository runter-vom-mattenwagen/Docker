### Creating user

The admin account is stored in vol_conf/users and provided to traefik using the correspondending rule in docker-compose-yml.

To add or update the credentials, use htpasswd which is part of apache2-utils.

Example:
```
htpasswd vol_conf/users admin
```

If the users file does not already exist, use parameter "-c" to create it.

### Certificates

The configuration is prepared for SSL offloading to all services. Just put your certs as cert.key and cert.crt in vol_certs.


