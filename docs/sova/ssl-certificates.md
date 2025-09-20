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

Add this line to check for renewal twice daily:
```
0 12,0 * * * certbot renew --quiet --deploy-hook "cp /etc/letsencrypt/live/your-domain.com/fullchain.pem /path/to/Parley-Chat/Backend/certs/cert.pem && cp /etc/letsencrypt/live/your-domain.com/privkey.pem /path/to/Parley-Chat/Backend/certs/key.pem && chown $USER:$USER /path/to/Parley-Chat/Backend/certs/*.pem && cd /path/to/Parley-Chat/Backend && docker compose restart nginx"
```

{: .note }
The `certbot renew` command only attempts renewal when certificates are within 30 days of expiration, so running it frequently doesn't hit Let's Encrypt's rate limits.

{: .note }
Replace `/path/to/Parley-Chat/Backend` with your actual installation path. The deploy-hook ensures certificates are copied and nginx is restarted when renewed.

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