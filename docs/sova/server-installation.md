---
title: Server Installation
parent: Sova (Backend)
nav_order: 1
---

# Server Installation

Production deployment of Parley Chat using Docker for optimal performance and security.

{: .note }
For development installation, use [Development Installation](development-installation.md) instead.

## Prerequisites

- Docker (recommended: Ubuntu with Docker CE or Windows with Docker Desktop)
- Git
- SSL certificates for HTTPS (production requirement)

## Installation Steps

### 1. Create a directory for Parley Chat

```sh
mkdir Parley-Chat
```

### 2. Navigate to Parley Chat directory

```sh
cd Parley-Chat
```

### 3. Install Docker

On Ubuntu:

```sh
curl -sSL https://get.docker.com/ | bash
```

{: .note }
In restricted networks, use http_proxy and https_proxy environment variables to install docker

### 4. Clone the Repository

```sh
git clone https://github.com/Parley-Chat/Sova
```

### 5. Navigate to Sova (Backend) Directory

```sh
cd Sova
```

### 6. Create Configuration File

Create an empty `config.toml` file (required for Docker to mount it as a file):

**Ubuntu:**
```sh
touch config.toml
```

**Windows:**
```sh
type nul > config.toml
```

### 7. Generate Configuration

Run Parley Chat backend to create a config file:

```sh
docker compose run --build --rm app
```

### 8. Configure the Backend

Open `config.toml` with an editor to configure the Backend:

```sh
nano config.toml
```

{: .note }
Some changes may require you to edit `docker-compose.yml` and `nginx.conf` too, like changing webserver port and URI Prefix

### 9. SSL Certificates

{: .note }
SSL certificates are **required** for production use. HTTP is unsupported due to WebCrypto API requirements.

Place your SSL certificates in the `certs` directory:
- `cert.pem` - Your SSL certificate
- `key.pem` - Your private key

For detailed instructions on obtaining and configuring SSL certificates, including automatic setup with Let's Encrypt and certbot, see [SSL Certificates](ssl-certificates.md).

### 10. Deploy

Start the services:

```sh
docker compose up -d --build
```

Wait for the build to complete. You can access it at `https://your-domain:port/uri-prefix-if-set/`

## Management Commands

### Update
To update the installation:
```sh
git pull
docker compose up -d --build
```

### Shutdown
To shut down all services:
```sh
docker compose down
```

### Start (without rebuilding)
To start without rebuilding/updating:
```sh
docker compose up -d
```