# CSU-Usopp-Local-Llama 🦙

A comprehensive Docker Compose setup for running Ollama and Open Web UI with GPU acceleration. This guide provides all the necessary instructions to manage and maintain your local LLM services.

## 📋 Table of Contents
- [Setup & Configuration](#-setup--configuration)
- [Core Commands](#-core-commands)
- [Model Management](#-model-management)
- [Maintenance & Troubleshooting](#-maintenance--troubleshooting)
- [Data Persistence](#-data-persistence)

## 🛠️ Setup & Configuration

All commands should be run from the project directory where the `docker-compose.yml` file is located.

**Project Structure:**
```bash
/home/cjung/Erfan/ollama/
├── docker-compose.yml
```

## 🚀 Core Commands

### Starting the Services
Launch both Ollama and Open Web UI in the background:

```bash
sudo docker compose up -d
```

After running this command, you can access the web interface at **http://localhost:3000**.

### Stopping the Services 🛑
Gracefully stop both services:

```bash
sudo docker compose down
```

> **Note:** This stops and removes the containers but leaves your data and models untouched.

### Checking Service Status 🔍
View the current status of your running services:

```bash
sudo docker compose ps
```

This shows if containers are `Up` (running) or `Exited` (stopped).

## 📦 Model Management

You can manage your LLMs through either the command line or web interface.

### Listing Installed Models
See all downloaded models:

```bash
sudo docker compose exec ollama ollama list
```

### Installing (Pulling) a New Model

#### Method 1: Web UI (Recommended) 🌐
1. Navigate to **http://localhost:3000**
2. Start a new chat
3. In the "Select a Model" field, type the model name (e.g., `llama3`, `codellama`, `gemma:2b`)
4. Press Enter - download begins automatically

#### Method 2: Command Line 💻
Pull a model directly from terminal:

```bash
# Example: Download the Llama 3 model
sudo docker compose exec ollama ollama pull llama3
```

### Uninstalling (Removing) a Model
Free up disk space by removing unused models:

```bash
# Example: Remove the Llama 3 model
sudo docker compose exec ollama ollama rm llama3
```

## 🔧 Maintenance & Troubleshooting

### Viewing Service Logs
Check logs when something isn't working:

```bash
# View Web UI logs
sudo docker compose logs open-webui

# View Ollama logs  
sudo docker compose logs ollama

# Follow logs in real-time
sudo docker compose logs -f open-webui
```

### Updating Services 🔄
Update Ollama and Open Web UI to latest versions:

```bash
# 1. Pull latest images
sudo docker compose pull

# 2. Recreate containers with new images
sudo docker compose up -d
```

### Accessing Container Shell
Get an interactive shell for advanced debugging:

```bash
# Access ollama container
sudo docker compose exec ollama bash
```

### Full Reset ⚠️
**Warning: This is destructive and cannot be undone!**

To completely reset your setup (deletes all models and chat history):

```bash
sudo docker compose down -v
```

## 💾 Data Persistence

This setup uses named **Docker Volumes** for data storage:

| Volume | Purpose |
|--------|---------|
| `ollama` | Stores all downloaded LLM models |
| `open-webui` | Stores chat history, user settings, and app data |

**Key Benefits:**
- ✅ Data survives container restarts and updates
- ✅ Independent of container lifecycle  
- ✅ Automatically reattached on `docker compose up -d`
- ✅ Safe to run `docker compose down` without data loss

---

## 🎯 Quick Start

1. **Start the services:**
   ```bash
   sudo docker compose up -d
   ```

2. **Open your browser:** Navigate to http://localhost:3000

3. **Download a model:** Type a model name (e.g., `llama3`) in the chat interface

4. **Start chatting:** Begin using your local LLM!

## 📞 Support

If you encounter issues:
1. Check the logs: `sudo docker compose logs -f`
2. Verify service status: `sudo docker compose ps`
3. Try restarting: `sudo docker compose restart`

---
*Happy chatting with your local LLM! 🦙✨*
