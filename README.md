# Media Server Stack with Traefik and Docker Compose

This repository contains a Docker Compose configuration for setting up a comprehensive media server stack. It leverages **Traefik** as a reverse proxy and load balancer, along with **Authelia** for authentication. The stack includes various services for media management, streaming, personal cloud storage, password management, and more.

## Features

- **Secure Reverse Proxy with Traefik:**
  - Automatically obtains and renews SSL certificates via Let's Encrypt.
  - Routes incoming traffic to the appropriate services based on domain names.
  - Supports dynamic service discovery and load balancing.

- **Authentication Middleware with Authelia:**
  - Provides two-factor authentication for enhanced security.
  - Integrates seamlessly with Traefik to protect services.

- **Media Management Services:**
  - **Sonarr:** Automated TV show management.
  - **Radarr:** Automated movie management.
  - **Deluge:** BitTorrent client for downloading media.
  - **Jellyfin:** Media server for streaming content to various devices.

- **Additional Services:**
  - **Vaultwarden:** Self-hosted password manager compatible with Bitwarden clients.
  - **Nextcloud:** Personal cloud storage and file sharing platform.
  - **Linkwarden:** Bookmark and link management tool.
  - **Minecraft Server:** Host your own Minecraft server with ease.

- **Database Support:**
  - **MariaDB and PostgreSQL:** Databases for Nextcloud and Linkwarden respectively.

- **Infrastructure Advantages:**
  - All services are containerized for easy deployment and management.
  - Centralized logging and monitoring capabilities can be added.
  - Scalability through Docker Compose and potential integration with orchestration tools.

## Stack Overview

### Reverse Proxy and Authentication

- **Traefik:**
  - Acts as a reverse proxy for routing HTTP and HTTPS traffic.
  - Utilizes DNS challenge for SSL certificate generation.
  - Configured to work with dynamic subdomains for each service.

- **Authelia:**
  - Provides authentication in front of selected services.
  - Supports single sign-on (SSO) and two-factor authentication (2FA).

### Media Management

- **Sonarr and Radarr:**
  - Automatically monitors, downloads, and organizes TV shows and movies.
  - Integrates with Deluge for downloading media content.

- **Deluge:**
  - A lightweight, open-source BitTorrent client.
  - Managed through a web interface secured by Traefik and Authelia.

- **Jellyfin:**
  - An open-source media server for streaming content.
  - Supports multiple users and devices.

### Additional Services

- **Vaultwarden:**
  - A lightweight Bitwarden server API implementation.
  - Allows secure password storage and synchronization across devices.

- **Nextcloud:**
  - A self-hosted file share and communication platform.
  - Offers features similar to Dropbox and Google Drive.

- **Linkwarden:**
  - A self-hosted bookmark manager to organize and archive webpages.
  - Features tagging, full-text search, and snapshot capabilities.

- **Minecraft Server:**
  - Run your own Minecraft server with customizable settings.
  - Supports whitelisting and operator permissions.

### Databases

- **MariaDB:**
  - Relational database for Nextcloud.
  - Stores user data, file metadata, and application settings.

- **PostgreSQL:**
  - Relational database for Linkwarden.
  - Manages bookmark data and user information.

## Prerequisites

- **Docker and Docker Compose:** Ensure both are installed and updated to the latest versions.
- **Domain Name:** A registered domain with DNS records pointing to your server.
- **DNS Provider API Token:** Access token for your DNS provider to enable Let's Encrypt DNS challenges.
- **Traefik Network:** An external Docker network named `traefik-network` for proxying services.

## Getting Started

### Clone the Repository
```
git clone https://github.com/your-username/media-server-stack.git
cd media-server-stack
```

### Configure Environment Variables

Create a `.env` file in the project root with the following content:

```
MY_DOMAIN=example.com
DNS_PROVIDER=cloudflare
DNS_PROVIDER_TOKEN=your-dns-provider-token
LETSENCRYPT_EMAIL=your-email@example.com
TZ=Your/Timezone
MINECRAFT_OPS=YourMinecraftUsername
```

- Replace the placeholder values with your actual configuration.
- `MY_DOMAIN` should be your main domain without subdomains.

### Set Up Secrets

Create a `secrets` directory and add your database passwords:

```
mkdir secrets
echo \"your_db_root_password\" > secrets/db_root_password.txt
echo \"your_db_password\" > secrets/db_password.txt
```

### Update Volume Paths

Edit the `docker-compose.yml` file to update the volume paths under each service to match your system's directory structure.

Example:

```
volumes:
  - /path/to/your/media/Movies:/movies
  - /path/to/your/media/TV:/tv
```

### Create the External Network

If you haven't already created the `traefik-network`, do so with:

```
docker network create traefik-network
```

### Start the Services

Bring up the stack using Docker Compose:

```
docker-compose up -d
```

### Verify Service Status

Check that all services are running:

```
docker-compose ps
```

- Logs can be viewed using:

```
docker-compose logs -f
```

## Configuration

### Traefik

- **Dynamic Configuration:** Traefik uses labels in the `docker-compose.yml` to route traffic.
- **SSL Certificates:** Automatically obtained via Let's Encrypt using the DNS challenge.

### Authelia

- **Authentication Backend:** Configured to use file-based or other supported backends.
- **Two-Factor Authentication:** Can be enabled per service by adjusting middleware settings in the compose file.

## Usage

### Managing Services

- **Start Services:** "docker-compose up -d"
- **Stop Services:** "docker-compose down"
- **Restart a Service:** "docker-compose restart service_name"

### Updating Containers

To update all services to the latest image versions:

```
docker-compose pull
docker-compose up -d
```

## Security Considerations

- **Secure Secrets:** Keep your `secrets` directory secure and do not expose it publicly.
- **Access Control:** Use Authelia to protect services that should not be publicly accessible.
- **Firewall:** Ensure your server's firewall only exposes necessary ports (80 and 443 for Traefik).
- **Regular Updates:** Keep your Docker images and host system updated to patch security vulnerabilities.

## License

This project is licensed under [GPLv2.0](https://www.gnu.org/licenses/old-licenses/gpl-2.0.txt).

## Acknowledgments

- **Traefik:** [traefik.io](https://traefik.io/)
- **Authelia:** [authelia.com](https://www.authelia.com/)
- **Jellyfin:** [jellyfin.org](https://jellyfin.org/)
- **Vaultwarden:** [github.com/dani-garcia/vaultwarden](https://github.com/dani-garcia/vaultwarden)
- **Nextcloud:** [nextcloud.com](https://nextcloud.com/)
- **Linkwarden:** [github.com/linkwarden/linkwarden](https://github.com/linkwarden/linkwarden)
- **ITZG Minecraft Server:** [github.com/itzg/docker-minecraft-server](https://github.com/itzg/docker-minecraft-server)
