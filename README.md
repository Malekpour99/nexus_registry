# Traefik - Nexus Project

## How To Setup

### Prepare Environment Variables

```sh
cp .env.example .env
```

- If you want to have a different name than `.env` you must run your docker compose command with `--env-file <your-desired-env>` flag!
- Update `BASE_DOMAIN` and `CERT_EMAIL` values

```sh
# Generate Traefik authentication credentials
echo $(htpasswd -nb <username> <password>) | sed -e s/\\$/\\$\\$/g
```

- replace `username` and `password` placeholders with your desired username and password and then assign the generated string to `TRAEFIK_AUTH_CREDENTIALS` in your environment variables

### Initial Project Run

```sh
# running service (detached)
docker compose up -d
```

- Extracting Nexus admin initial password

```sh
# connecting to nexus service container
docker compose exec -it nexus /bin/sh

# showing auto-generated initial password
cat /nexus-data/admin.password
```

- After login, you will be prompted by nexus to change your password.
- Only enable *anonymous access* for secure internal systems which you are certain about them!

#### Nexus Storage & Repository

- Create a new **file**-base blob storage
- Create a **docker (hosted)** repository
  - add your port for HTTP/HTTPS connection requests
  - enable anonymous pulls (only in safe internal environments)
  - dedicate your recently created blob to this storage

#### Nexus Security

`Settings` → `Security` → `Realms`

Move **Docker Bearer Token Realm** to the **Active** column.

### Routine Project Run

```sh
# running service (detached)
docker compose up -d
```
