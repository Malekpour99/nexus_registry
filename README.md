# Traefik - Nexus Project

## How To Setup

### Generate Traefik Password Hash

```sh
echo $(htpasswd -nb user password) | sed -e s/\\$/\\$\\$/g
```
