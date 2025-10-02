---
title: Development Installation
parent: Sova (Backend)
nav_order: 3
---

# Development Installation

Set up Parley Chat for development and testing purposes.

## Prerequisites

First, install Python using [Python Installation](python-installation.md)

## Installation Steps

**1. Create a directory for Parley Chat and navigate to it**

```sh
mkdir Parley-Chat
cd Parley-Chat
```

**2. Clone the Repository**

```sh
git clone https://github.com/Parley-Chat/Sova.git
```

**3. (Optional but recommended) Clone the Mura (Frontend) Repository**

To also host the Frontend with the Backend clone the Mura (Frontend) Repository:
```sh
git clone https://github.com/Parley-Chat/Mura.git
```

**4. Navigate to Sova (Backend) Directory**

```sh
cd Sova
```

**5. Install Dependencies**

Install the required Python packages:
```sh
pip install -r requirements.txt
```

**6. Generate Configuration File**

Run Parley Chat backend to create a config file:
```sh
python3.13 main.py
```

{: .note }
Replace `python3.13` with `python` on Windows

**7. Configure the Backend**

Open `config.toml` with a text editor to configure the Backend:

**Linux:**
```sh
nano config.toml
```

**Windows:**
```sh
notepad config.toml
```

You can configure the Backend to run in debug mode using `dev=true` in the config file.

**8. Start the Server**

Run the following command to start the server:

```sh
python3.13 main.py
```

{: .note }
Replace `python3.13` with `python` if you're on Windows

{: .note }
You can also run in development mode/debug mode using `--dev` in the command

You can access it at `http://your-domain:port/uri-prefix-if-set/` (or `https://` if SSL is configured)

## Management Commands

### Shutdown
To shut it down, use `Ctrl+C` in the terminal you're running it in.

### Update
To update the installation:

```sh
git pull
```

Then run it again using the start command from step 8.