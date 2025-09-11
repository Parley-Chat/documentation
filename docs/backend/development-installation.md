---
title: Development Installation
parent: Backend
nav_order: 3
---

# Development Installation

Set up Parley Chat for development and testing purposes.

## Prerequisites

First, install Python using [Python Installation](python-installation.md)

## Installation Steps

### 1. Clone the Repository

```sh
git clone https://github.com/Parley-Chat/Parley-Chat
```

### 2. Navigate to Backend Directory

```sh
cd Parley-Chat/Backend
```

### 3. Install Dependencies

Install the required Python packages:

```sh
pip install -r requirements.txt
```

### 4. Generate Configuration File

Run Parley Chat backend to create a config file:

```sh
python3.13 main.py
```

{: .note }
Replace python3.13 with python if you're on Windows

### 5. Configure the Backend

Open `config.toml` with an editor to configure the Backend:

```sh
nano config.toml
```

You can configure the Backend to run in debug mode using `dev=true` in the config file.

### 6. Start the Server

Run the following command to start the server:

```sh
python3.13 main.py
```

{: .note }
Replace python3.13 with python if you're on Windows

{: .note }
You can also run in development mode/debug mode using `--dev` in the command

You can access it at `https://your-domain:port/uri-prefix-if-set/`

## Management Commands

### Shutdown
To shut it down, use `Ctrl+C` in the terminal you're running it in.

### Update
To update the installation:

```sh
git pull
```

Then run it again using the start command from step 6.