---
title: SSL Certificates
parent: Sova (Backend)
nav_order: 4
---

# SSL Certificates

SSL certificates are **required** for production use of Parley Chat. HTTP is unsupported due to WebCrypto API requirements.

## Quick Setup

### Self-Signed SSL Certificate (Development Only)

For development and testing purposes, you can generate self-signed SSL certificates:

```sh
python3.13 self_ssl.py
```

{: .note }
Replace python3.13 with python if you're on Windows

{: .warning }
Self-signed certificates will only work with the frontend hosted by the backend itself and should not be used in production.

### Manual Certificate Setup

Obtain certificates using Let's Encrypt or your certificate provider and place them in the `certs` directory:
- `cert.pem` - Your SSL certificate
- `key.pem` - Your private key

## Automatic SSL Certificate Management with Certbot

For production deployments, you can use Certbot to automatically obtain and renew SSL certificates from Let's Encrypt:

### Installation

**Install Certbot:**
```sh
sudo apt update
sudo apt install certbot
```

### Initial Certificate Setup

**Stop any services using ports 80/443:**
```sh
docker compose down
```

**Obtain certificates (replace your-domain.com with your actual domain):**
```sh
sudo certbot certonly --standalone -d your-domain.com
```

**Copy certificates to certs directory:**
```sh
sudo cp /etc/letsencrypt/live/your-domain.com/fullchain.pem certs/cert.pem
sudo cp /etc/letsencrypt/live/your-domain.com/privkey.pem certs/key.pem
sudo chown $USER:$USER certs/*.pem
```

### Automatic Renewal Setup

**Set up automatic renewal:**

```sh
sudo crontab -e
```

Add this line to check for renewal twice daily (midnight and noon):

```cron
0 0,12 * * * /usr/bin/certbot renew --quiet \
  --pre-hook "/usr/bin/docker compose -f /path/to/Parley-Chat/Sova/docker-compose.yml stop nginx" \
  --post-hook "/usr/bin/docker compose -f /path/to/Parley-Chat/Sova/docker-compose.yml start nginx" \
  --deploy-hook "cp /etc/letsencrypt/live/your-domain.com/fullchain.pem /path/to/Parley-Chat/Sova/certs/cert.pem && \
                 cp /etc/letsencrypt/live/your-domain.com/privkey.pem /path/to/Parley-Chat/Sova/certs/key.pem && \
                 chown appuser:appuser /path/to/Parley-Chat/Sova/certs/*.pem && \
                 /usr/bin/docker compose -f /path/to/Parley-Chat/Sova/docker-compose.yml exec -T nginx nginx -s reload"
```

{: .note }
Replace `/path/to/Parley-Chat/Sova` with your actual installation path.

{: .note }
The `pre-hook` and `post-hook` temporarily stop nginx so Certbot in **standalone mode** can bind to port 80 for domain validation. If you switch to `webroot` mode later, you can drop these two hooks and avoid downtime entirely.

{: .note }
`certbot renew` only renews certificates that are within 30 days of expiration, so running it twice a day is safe and avoids hitting Let's Encrypt rate limits.

### Verification

**Test the renewal process:**
```sh
sudo certbot renew --dry-run
```

**Check certificate expiration:**
```sh
sudo certbot certificates
```

## Troubleshooting

### Common Issues

**Port 80/443 already in use:**
- Stop all services using these ports before running certbot
- Use `sudo netstat -tlnp | grep :80` to find processes using port 80

**Permission issues:**
- Ensure the `certs` directory exists and has correct permissions
- Use `sudo chown $USER:$USER certs/*.pem` after copying certificates

**Domain validation fails:**
- Ensure your domain points to the server's IP address
- Check firewall settings allow traffic on port 80

### Alternative Methods

**Using webroot method (if you have a running web server):**
```sh
sudo certbot certonly --webroot -w /var/www/html -d your-domain.com
```

**Using nginx plugin:**
```sh
sudo certbot --nginx -d your-domain.com
```