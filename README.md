## -- quic error can be fixed by adding an SSL certificate
https://medium.com/@life-is-short-so-enjoy-it/homelab-nginx-proxy-manager-setup-ssl-certificate-with-domain-name-in-cloudflare-dns-732af64ddc0b

we can get the ssl via cloudflare one

# nginx-proxy-manager
Setup readme how to make this work

If you run the default vaultwarden container, it won't listen on port 49154 but on 80.

Your port mappings -p 49152:80 do not force the container to listen on 49152, but your Docker host server! Since you are using Docker hostnames and talking to the containers directly, you have to use the correct listening ports of the specific container itself. For vaultwarden it is TCP/80.

So try the correct listening port of the container, not your custom port mapping. Then it will work.

Note: NPM reverse proxy must be in the same network as the container you want to proxy. Otherwise it cannot talk to your containers or resolve the hostnames.
---
# --

```
version: '3.8'
services:
  app:
    networks:
    -  'installation_default'
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    # Uncomment the next line if you uncomment anything in the section
    # environment:
      # Uncomment this if you want to change the location of
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
networks:
  installation_default:
    external: true
```


Pinging it from container B to container A is not guaranteed there is a communication

#  --

In nginx proxy manager you can specify the IP of the docker container or you can also specify the docker name
