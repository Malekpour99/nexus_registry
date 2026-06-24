# Traefik - Nexus Project

## Table of Contents

- [Traefik - Nexus Project](#traefik---nexus-project)
  - [Table of Contents](#table-of-contents)
  - [How To Setup](#how-to-setup)
    - [Prepare Environment Variables](#prepare-environment-variables)
    - [Initial Project Run](#initial-project-run)
      - [Nexus Storage \& Repository](#nexus-storage--repository)
      - [Nexus Security](#nexus-security)
    - [Routine Project Run](#routine-project-run)
  - [Links](#links)

## How To Setup

### Prepare Environment Variables

```sh
cp .env.example .env
```

- If you want to have a different name than `.env` you must run your docker compose command with `--env-file <your-desired-env>` flag!
- Update `BASE_DOMAIN` and `CERT_EMAIL` values

```sh
# Generate Traefik authentication credentials
echo $(htpasswd -nb <username> <password>) | sed -e 's/\$/$$/g'
```

- replace `username` and `password` placeholders with your desired username and password and then assign the generated string to `TRAEFIK_AUTH_CREDENTIALS` in your environment variables (if your password contains special characters (e.g. `$`), make sure to escape them properly using `\` or surround your password with single quotes `'`).

### Initial Project Run

```sh
# running service (detached)
docker compose up -d
```

- Extracting Nexus **admin** initial password

```sh
# connecting to nexus service container
docker compose exec -it nexus /bin/sh

# showing auto-generated initial password for 'admin' user
cat /nexus-data/admin.password
```

- After login, you will be prompted by nexus to change your password.
- Only enable _anonymous access_ for secure internal systems which you are certain about them!

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

## Links

- **Traefik Dashboard**: <https://traefik.${BASE_DOMAIN}>
  - You can use the credentials you set in `TRAEFIK_AUTH_CREDENTIALS` to login to the dashboard.
- **Nexus Repository**: <https://registry.${BASE_DOMAIN}>
