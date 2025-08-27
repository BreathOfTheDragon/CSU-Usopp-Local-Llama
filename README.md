# CSU-Usopp-Local-Llama



Ollama & Open Web UI Docker Setup Guide

This guide provides all the necessary instructions to manage and maintain the Ollama and Open Web UI services using Docker Compose. This setup is configured for GPU acceleration.

All you need is a single configuration file in your project directory.



This file defines and configures both the Ollama and Open Web UI services.

docker-compose.yml:

version: '3.8'

services:
  ollama:
    image: ollama/ollama
    container_name: ollama
    # This section grants the container access to all NVIDIA GPUs
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]
    volumes:
      - ollama:/root/.ollama
    networks:
      - ollama-net
    ports:
      - "11434:11434"
    restart: unless-stopped

  open-webui:
    image: ghcr.io/open-webui/open-webui:cuda
    container_name: open-webui
    depends_on:
      - ollama
    ports:
      - "3000:8080"
    environment:
      - 'OLLAMA_BASE_URL=http://ollama:11434'
    volumes:
      - open-webui:/app/backend/data
    networks:
      - ollama-net
    restart: unless-stopped

volumes:
  ollama:
  open-webui:

networks:
  ollama-net:
    driver: bridge

Core Commands

Run all docker compose commands from the same directory as your docker-compose.yml file.
Starting the Services 🚀

To start both Ollama and Open Web UI in the background:

sudo docker compose up -d

After running this, you can access the web interface at http://localhost:3000.
Stopping the Services 🛑

To gracefully stop both services:

sudo docker compose down

This stops and removes the containers but leaves your data and models untouched (see Data Persistence).
Checking the Status 🔍

To see the current status of your running services:

sudo docker compose ps

This will show if the containers are Up (running) or Exited (stopped).
Model Management

You can manage your LLMs either through the command line or the web interface.
Listing Installed Models

To see a list of all the models you have downloaded:

sudo docker compose exec ollama ollama list

Installing (Pulling) a New Model
Method 1: From the Web UI (Recommended)

    Navigate to http://localhost:3000.

    Start a new chat.

    In the "Select a Model" field, simply type the name of the model you want (e.g., llama3, codellama, gemma:2b).

    Press Enter. The download will begin automatically.

Method 2: From the Command Line

To pull a model directly from the terminal:

# Example: Download the Llama 3 model
sudo docker compose exec ollama ollama pull llama3

Uninstalling (Removing) a Model

To free up disk space by removing a model you no longer need:

# Example: Remove the Llama 3 model
sudo docker compose exec ollama ollama rm llama3

Maintenance & Troubleshooting
Viewing Service Logs

If something isn't working, the logs are the first place to look.

# View the logs for the Web UI
sudo docker compose logs open-webui

# View the logs for Ollama
sudo docker compose logs ollama

# To follow the logs in real-time, add the -f flag
sudo docker compose logs -f open-webui

Updating the Services 🔄

To update Ollama and Open Web UI to their latest versions:

# 1. Pull the latest images defined in your compose file
sudo docker compose pull

# 2. Recreate the containers with the new images
sudo docker compose up -d

Accessing a Container's Shell

For advanced debugging, you can get an interactive shell inside a running container:

# Get a bash shell inside the ollama container
sudo docker compose exec ollama bash

Starting Fresh (Full Reset) ⚠️

If you want to completely reset your entire setup, including deleting all downloaded models and chat history:

Warning: This is a destructive action and cannot be undone.

# This command stops the containers AND deletes the associated volumes.
sudo docker compose down -v

Data Persistence

This setup uses named Docker Volumes to store your data, which is the recommended practice.

    ollama volume: Stores all your downloaded LLM models.

    open-webui volume: Stores your chat history, user settings, and other application data.

These volumes are independent of the containers. This means you can stop, remove, or update the containers (docker compose down) without losing any of your important data. The data will be automatically re-attached the next time you run docker compose up -d.
