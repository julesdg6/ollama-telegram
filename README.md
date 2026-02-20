<div align="center">
  <br>
  <a href="">
    <img src="res/github/ollama-telegram-readme.png" width="200" height="200">
  </a>
  <h1>🦙 Ollama Telegram Bot</h1>
  <p>
    <b>Chat with your LLM, using Telegram bot!</b><br>
    <b>Feel free to contribute!</b><br>
  </p>
  <br>
  <p align="center">
    <img src="https://img.shields.io/docker/pulls/ruecat/ollama-telegram?style=for-the-badge">
  </p>
  <br>
</div>

## Features
Here's features that you get out of the box:

- [x] Mention bot in group to get an answer
- [x] Bot usage whitelist

## Roadmap
- [x] Docker config & automated tags by [StanleyOneG](https://github.com/StanleyOneG), [ShrirajHegde](https://github.com/ShrirajHegde)
- [x] History and `/reset` by [ShrirajHegde](https://github.com/ShrirajHegde)
- [ ] v2.0 Migration

## Prerequisites
- [Telegram-Bot Token](https://core.telegram.org/bots#6-botfather)

## Installation (Non-Docker)
+ Install latest [Python](https://python.org/downloads)
+ Clone Repository
    ```
    git clone https://github.com/ruecat/ollama-telegram
    ```
+ Install requirements from requirements.txt
    ```
    pip install -r requirements.txt
    ```
+ Enter all values in .env.example

+ Rename .env.example -> .env

+ Launch bot

    ```
    python3 run.py
    ```
## Installation (Docker Image)
The official image is available at dockerhub: [ruecat/ollama-telegram](https://hub.docker.com/r/ruecat/ollama-telegram)

+ Download [.env.example](https://github.com/ruecat/ollama-telegram/blob/main/.env.example) file, rename it to .env and populate the variables.
+ Create `docker-compose.yml` (optionally: uncomment GPU part of the file to enable Nvidia GPU)

    ```yml
    version: '3.8'
    services:
      ollama-telegram:
        image: ruecat/ollama-telegram
        container_name: ollama-telegram
        restart: on-failure
        env_file:
          - ./.env
      
      ollama-server:
        image: ollama/ollama:latest
        container_name: ollama-server
        volumes:
          - ./ollama:/root/.ollama
        
        # Uncomment to enable NVIDIA GPU
        # Otherwise runs on CPU only:
    
        # deploy:
        #   resources:
        #     reservations:
        #       devices:
        #         - driver: nvidia
        #           count: all
        #           capabilities: [gpu]
    
        restart: always
        ports:
          - '11434:11434'
    ```

+ Start the containers

    ```sh
    docker compose up -d
    ```


## Installation (Build your own Docker image)
+ Clone Repository
    ```
    git clone https://github.com/ruecat/ollama-telegram
    ```

+ Enter all values in .env.example

+ Rename .env.example -> .env

+ Run ONE of the following docker compose commands to start:
    1. To run ollama in docker container (optionally: uncomment GPU part of docker-compose.yml file to enable Nvidia GPU)
        ```
        docker compose up --build -d
        ```

    2. To run ollama from locally installed instance (mainly for **MacOS**, since docker image doesn't support Apple GPU acceleration yet):
        ```
        docker compose up --build -d ollama-tg
        ```

## Installation (Unraid)

Unraid users need to build the image locally before adding it via the Unraid web UI.

### 1. Build the image on your Unraid server

Open a terminal (e.g. via the Unraid web UI → **Terminal**) and run:

```bash
git clone https://github.com/julesdg6/ollama-telegram.git
cd ollama-telegram
docker build -t ollama-telegram .
```

### 2. Add the container via the Unraid template

A ready-made Unraid template is included in this repo at [`unraid/ollama-telegram.xml`](unraid/ollama-telegram.xml).

To use it, copy the template XML to Unraid's template directory and then add the container through the web UI:

```bash
cp /path/to/ollama-telegram/unraid/ollama-telegram.xml /boot/config/plugins/dockerMan/templates-user/
```

Then in the Unraid web UI go to **Docker** → **Add Container** and select **ollama-telegram** from the template drop-down. Set the following fields (all other fields have sensible defaults):

| Field | Value |
|---|---|
| **Repository** | `ollama-telegram:latest` |
| **Network** | `bridge` |
| **TOKEN** | Your Telegram bot token |
| **ADMIN_IDS** | Your Telegram user ID(s) |
| **USER_IDS** | Allowed user ID(s) |
| **OLLAMA_BASE_URL** | See networking note below |

### 3. Networking

How you set `OLLAMA_BASE_URL` depends on how Ollama is running:

| Ollama setup | `OLLAMA_BASE_URL` value |
|---|---|
| Docker container on the same custom network | `http://ollama` (use the container name) |
| Unraid host networking / bare-metal install | `http://<unraid-ip>` (e.g. `http://192.168.1.10`) |

> **Tip:** Do **not** include a trailing slash or port in `OLLAMA_BASE_URL`. The port is automatically appended from `OLLAMA_PORT` (default: `11434`), so keep them separate.

## Environment Configuration
|          Parameter          |                                                      Description                                                      | Required? | Default Value |                        Example                        |
|:---------------------------:|:---------------------------------------------------------------------------------------------------------------------:|:---------:|:-------------:|:-----------------------------------------------------:|
|           `TOKEN`           | Your **Telegram bot token**.<br/>[[How to get token?]](https://core.telegram.org/bots/tutorial#obtain-your-bot-token) |    Yes    |  `yourtoken`  |             MTA0M****.GY5L5F.****g*****5k             |
|         `ADMIN_IDS`         |                     Telegram user IDs of admins.<br/>These can change model and control the bot.                      |    Yes    |               | 1234567890<br/>**OR**<br/>1234567890,0987654321, etc. |
|         `USER_IDS`          |                       Telegram user IDs of regular users.<br/>These only can chat with the bot.                       |    Yes    |               | 1234567890<br/>**OR**<br/>1234567890,0987654321, etc. |
|         `INITMODEL`         |                                                      Default LLM                                                      |    No     |   `llama2`    |        mistral:latest<br/>mistral:7b-instruct         |
|      `OLLAMA_BASE_URL`      |                                                  Your OllamaAPI URL                                                   |    No     |               |          localhost<br/>host.docker.internal           |
|        `OLLAMA_PORT`        |                                                  Your OllamaAPI port                                                  |    No     |     11434     |                                                       |
|            `TIMEOUT`        |                                    The timeout in seconds for generating responses                                    |    No     |     3000      |                                                       |
| `ALLOW_ALL_USERS_IN_GROUPS` |                Allows all users in group chats interact with bot without adding them to USER_IDS list                 |    No     |       0       |                                                       |



## Credits
+ [Ollama](https://github.com/jmorganca/ollama)

## Libraries used
+ [Aiogram 3.x](https://github.com/aiogram/aiogram)
