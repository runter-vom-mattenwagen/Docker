# Traefik
_Almost ready-to-use configuration for serving dockerized web applications_

This configuration creates Traefik as a self-configuring reverse proxy for Docker containers. Traefik monitors the Docker socket and configures itself according to the labels of the associated containers. Traefik listens on ports 80 and 443, but redirects all connection from http (port 80) to https (port 443).

## Preparation

### Creating user

The admin account is stored in vol_conf/users and made available to traefik via the correspondending rule in docker-compose.yml.

To add or update the credentials, use htpasswd which is part of apache2-utils.

Example:
```
htpasswd vol_conf/users admin
```

If the users file does not exist yet, use parameter "-c" to create it.

### Certificates

The configuration is prepared for SSL offloading to all services. Just put your certs as cert.key and cert.crt in vol_certs. The certificate is used for all containers accessed through Traefik. Thus, the certificate must be either a SAN certificate containing all hostnames or a wildcard certificate.

### Hostname

Open ```.env``` in an editor and set ```VIRTUAL_HOSTNAME``` to the name of the host where Traefik is started. This can be the real hostname or an alias (CNAME). Of course, it must be resolved by a DNS.

If everything went fine, your Traefik dashboard should appear when open the URL "https://<DOCKER_SERVER>/dashboard/" in a browser. Notice the trailing "/".

## Container configuration

Responsible for how Traefik handles a container are labels that are set when the container is started:

```
    labels:
      - traefik.http.routers.<SERVICE_NAME>.entrypoints=https
      - traefik.http.routers.<SERVICE_NAME>.tls=true
      - traefik.http.routers.<SERVICE_NAME>.rule=Host(`myhost.domain.local`)
      - traefik.http.services.<SERVICE_NAME>.loadbalancer.server.port=80
```

This means: The URL https://myhost.domain.local leads to container <SERVICE_NAME>:80

This requires an exlusive hostname for each service. If a service is to be accessed via a subdirectory, the path must be appended to the rule: 

```&& (PathPrefix(`/subdir1`) || PathPrefix(`/subdir1`))```

However, the service must know that it is accessed via the subdirectory.

