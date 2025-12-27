# AI-Powered OCR Telegram Bot

This is a self-hosted Telegram bot that converts images to text using advanced Vision AI models via OpenRouter.

The project is built on **n8n** (workflow automation) and runs inside **Docker**. It uses **Ngrok** to create a secure tunnel, allowing the bot to run on your local computer or server without exposing ports publicly.

## Prerequisites

Before you begin, ensure you have the following installed and configured:

### 1. Software

* **Docker Desktop**: Required to run the containers.
* [Windows/Mac/Linux Installation Guide](https://www.docker.com/products/docker-desktop/)
* *Note: Ensure Docker is running before proceeding.*



### 2. Accounts & Keys

You will need API keys from the following services (all offer free tiers):

1. **Telegram Bot Token**:
* Open Telegram and search for **@BotFather**.
* Send command `/newbot`, give it a name, and copy the **HTTP API Token**.


2. **Ngrok Authtoken**:
* Sign up at [ngrok.com](https://ngrok.com).
* Go to your dashboard -> "Your Authtoken" and copy it.
* *This is required to make your local bot accessible to Telegram servers.*


3. **OpenRouter API Key**:
* Sign up at [openrouter.ai](https://openrouter.ai).
* Create a new API Key.
* *This connects the bot to AI models like Llama or Qwen.*

---

## Installation Guide

### 1. Clone the Repository

Open your terminal (Command Prompt / Terminal) and run:

```bash
git clone https://github.com/YOUR_USERNAME/ocr-bot.git
cd ocr-bot

```

### 2. Configure Environment Variables

Create a `.env` file to store your secrets. You can copy the example file:

```bash
# Windows
copy .env.example .env

# Mac/Linux
cp .env.example .env

```

Open the `.env` file in any text editor (Notepad, VS Code) and fill in your keys:

```ini
# Ngrok Configuration
# Get your static domain from Ngrok dashboard (optional but recommended) or leave generic
NGROK_AUTHTOKEN=2N7z...YOUR_NGROK_TOKEN...
NGROK_DOMAIN=plutonic-unmirthful-sharilyn.ngrok-free.dev

# n8n Security (Set your own username/password for the admin panel)
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=securepassword123

```

### 3. Start the Application

Run the following command to download and start the services. This might take a few minutes the first time.

```bash
docker compose up -d

```

Check if everything is running:

```bash
docker compose ps

```

*You should see `n8n` and `ngrok` containers with status "Up".*

---

## Workflow Setup (n8n)

The infrastructure is running, now you need to load the bot logic.

1. **Access n8n**: Open your browser and go to `http://localhost:5678`.
2. **Login**: Use the username and password you set in the `.env` file.
3. **Setup Credentials**: Go to **Credentials** (key icon on the left) → **Add Credential**.

**A. Telegram API (Required)**
* Search for **Telegram API**.
* Enter your Bot Token (from BotFather) and click Save.

**B. OpenRouter - Option 1: Header Auth (For Current Workflow)**
* Search for **Header Auth**.
* **Name**: `Authorization`
* **Value**: `Bearer sk-or-v1-YOUR_KEY`
* ⚠️ **Important**: You must type the word `Bearer` followed by a space before your key.

**C. OpenRouter - Option 2: OpenAI Compatible (For AI Agent Nodes)**
* Search for **OpenAI API**.
* **API Key**: Paste your OpenRouter key (`sk-or-v1...`).
* Set the **Base URL** to `https://openrouter.ai/api/v1`
* Click Save.


4. **Import Workflow**:
* Click the **Workflow** menu (top left) → **Import from File**.
* Select `workflows/ocr_bot.json` from this project folder.


5. **Activate**:
* Toggle the switch in the top-right corner from **Inactive** to **Active**.



## Usage

1. Open your bot in Telegram.
2. Send an image (photo or document) containing text.
3. The bot will reply with the transcribed text.

## License

MIT
