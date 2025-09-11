---
title: Backend
nav_order: 2
has_children: true
---

# Backend Documentation

This section covers everything needed to set up and run the Parley Chat backend server.

## Installation Options

### Production Deployment
Recommended for hosting Parley Chat in production:

- **[Server Installation](server-installation.md)** - Docker-based production deployment with NGINX

### Development Setup
Perfect for contributors and developers who want to modify the code:

1. **[Python Installation](python-installation.md)** - Install Python 3.13 and required dependencies
2. **[Development Installation](development-installation.md)** - Set up the development environment

## Additional Configuration

- **[SSL Certificates](ssl-certificates.md)** - Set up SSL certificates with automatic renewal using Let's Encrypt

## Requirements

- **Development**: Python 3.13, pip
- **Production**: Docker and Docker Compose
- **Security**: SSL certificates (production only)