---
title: Python Installation
parent: Backend
nav_order: 2
---

# Python Installation

Parley Chat requires Python 3.13 for optimal performance and compatibility.

## Windows Installation

1. Download Python 3.13 from [python.org](https://python.org)
2. **Important**: Check "Add to PATH" during installation to avoid issues later
3. Verify installation by opening Command Prompt and running:
   ```cmd
   python --version
   ```

## Ubuntu Installation

Run the following commands to install Python 3.13 and dependencies:

```sh
apt-get update && \
apt-get install -y software-properties-common curl && \
add-apt-repository -y ppa:deadsnakes/ppa && \
apt-get update && \
apt-get install -y python3.13 python3.13-venv python3.13-dev build-essential libssl-dev libffi-dev cargo && \
apt-get remove -y python3-blinker && \
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.13
```