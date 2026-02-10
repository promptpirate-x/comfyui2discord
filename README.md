<div align="center">

<img src="assets/logo.png" alt="Prompt Pirate Logo" width="200" height="200" style="border-radius: 50%;">

# 🏴‍☠️ ComfyUI Discord Bot

### Generate AI images & videos directly from Discord using your local ComfyUI instance

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Discord.py](https://img.shields.io/badge/discord.py-2.3+-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discordpy.readthedocs.io/)
[![ComfyUI](https://img.shields.io/badge/ComfyUI-Compatible-green?style=for-the-badge)](https://github.com/comfyanonymous/ComfyUI)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

[Features](#-features) •
[Installation](#-installation) •
[Commands](#-commands) •
[Workflows](#-workflows) •
[Troubleshooting](#-troubleshooting)

---

</div>

## 🔥 What is this?

A **Discord bot** that bridges your local [ComfyUI](https://github.com/comfyanonymous/ComfyUI) instance with Discord, letting you and your community generate AI images using **any ComfyUI workflow** — directly from chat.

Drop in your workflow JSONs, and the bot automatically detects prompts, samplers, and image inputs. No coding required for new workflows.

> Works with ComfyUI Portable / `python_embeded` version on **Windows**.

<div align="center">

```
/generate prompt:a pirate ship sailing through a galaxy of stars workflow:Turbo Text To Image
```

</div>

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🎨 **Text-to-Image** | Generate images from text prompts with full parameter control |
| 🖼️ **Image-to-Image** | Upload reference images for img2img workflows |
| 📂 **Multi-Workflow** | Drop `.json` files in `/workflows` — auto-detected with friendly names |
| ⚡ **Slash Commands** | Full Discord slash command support with autocomplete |
| 🔤 **Prefix Commands** | Classic `!generate` prefix commands also supported |
| 📊 **Live Progress** | Real-time WebSocket progress updates during generation |
| 🧬 **LoRA Browser** | Browse, search, and preview your LoRA collection from Discord |
| 🏗️ **Model Browser** | List and select checkpoints/models with preview images |
| ⚙️ **Full Parameter Control** | Width, height, steps, CFG, sampler, scheduler, denoise |
| 📬 **Queue System** | Queue management with position tracking |
| 💬 **DM Support** | Works in servers and Direct Messages |
| 🔄 **Hot Reload** | Reload workflows without restarting the bot |

---

## 📦 Installation

### Prerequisites

| Requirement | Details |
|------------|---------|
| **ComfyUI** | Running locally (default: `http://127.0.0.1:8188`) |
| **Python** | 3.8 or higher |
| **Discord Bot Token** | From the [Discord Developer Portal](https://discord.com/developers/applications) |

### Quick Start

**1. Clone the repository**
```bash
git clone https://github.com/promptpirate-x/comfyui-discord-bot.git
cd comfyui-discord-bot
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```

**3. Configure environment**

Create a `.env` file or use `run_bot.bat` (Windows):
```env
DISCORD_TOKEN=your_discord_bot_token_here
COMFYUI_SERVER=127.0.0.1:8188
WORKFLOWS_DIR=workflows
COMFYUI_MODELS_PATH=E:\ComfyUI\models
```

**4. Add your workflows**

Export workflows from ComfyUI in **API format**:
> Settings → Enable Dev Mode → Save (API Format)

Drop the `.json` files into the `workflows/` folder.

**5. Run the bot**
```bash
python comfyui_discord_bot.py
```
Or on Windows, double-click `run_bot.bat`.

---

## 🤖 Commands

### Slash Commands

| Command | Description |
|---------|-------------|
| `/generate` | Generate an image (text-to-image or image-to-image) |
| `/workflows` | List all available workflows |
| `/queue` | Check the current ComfyUI queue status |
| `/reload` | Reload workflows and rescan models |
| `/loras` | Browse available LoRAs with previews |
| `/lora_preview` | Show preview image for a specific LoRA |
| `/models` | List available checkpoints/models |

### Prefix Commands

| Command | Aliases | Description |
|---------|---------|-------------|
| `!generate <prompt>` | `!gen`, `!img` | Generate with default workflow |
| `!generate "Workflow Name" <prompt>` | — | Generate with a specific workflow |
| `!workflows` | `!wf`, `!list` | List workflows |
| `!queue` | — | Check queue status |
| `!reload` | — | Reload workflows |
| `!help_comfy` | — | Show help message |

### `/generate` Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `prompt` | Text | *(Required)* Your image description |
| `workflow` | Autocomplete | Choose from available workflows |
| `image1` | File | Upload an image for img2img |
| `image2` | File | Upload a second reference image |
| `width` | Integer | Image width (default: workflow setting) |
| `height` | Integer | Image height (default: workflow setting) |
| `steps` | Integer | Sampling steps |
| `cfg` | Float | CFG scale |
| `sampler` | Text | Sampler name (e.g., `euler`, `dpmpp_2m`) |
| `scheduler` | Text | Scheduler (e.g., `karras`, `simple`) |
| `denoise` | Float | Denoise strength for img2img |

---

## 📁 Workflows

The bot uses a **plug-and-play workflow system**. Any ComfyUI workflow exported in API format will work automatically.

### How It Works

```
workflows/
├── z-image-turbo_text-to-image.json    → "Z Image Turbo Text To Image"
├── realistic-photo.json                → "Realistic Photo"
├── anime-style.json                    → "Anime Style"
└── qwen-edit-outpaint.json             → "Qwen Edit Outpaint"
```

Filenames are converted to friendly display names automatically. The bot **auto-detects** these node types in any workflow:

| Node Type | What It Controls |
|-----------|------------------|
| `CLIPTextEncode` | Your text prompt |
| `KSampler` / `Sampler` | Seed, steps, CFG, sampler, scheduler |
| `LatentImage` / `EmptyLatentImage` | Width and height |
| `LoadImage` | Image inputs for img2img |
| `LoraLoader` | LoRA model selection |
| `CheckpointLoader` / `UNETLoader` | Model selection |
| `Gemini` nodes | Google Gemini API integration |

### Adding a New Workflow

1. Open ComfyUI in your browser
2. Build or load your workflow
3. Go to **Settings** → Enable **Dev Mode Options**
4. Click **Save (API Format)**
5. Move the `.json` file to `workflows/`
6. In Discord, run `/reload`

That's it — the new workflow appears in the `/generate` autocomplete dropdown.

---

## 🛠️ Discord Bot Setup

<details>
<summary><b>📋 Step-by-step Discord Developer Portal setup</b></summary>

1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Click **New Application** → name it
3. Go to **Bot** → Click **Reset Token** → Copy the token
4. Enable **Message Content Intent** under Privileged Gateway Intents
5. Go to **OAuth2** → **URL Generator**
6. Select scopes: `bot`, `applications.commands`
7. Select permissions:
   - ✅ Send Messages
   - ✅ Attach Files
   - ✅ Read Message History
   - ✅ Embed Links
   - ✅ Use Slash Commands
8. Copy the generated URL and open it to invite the bot

**Permission Integer:** `274877975552`

**Quick Invite URL:**
```
https://discord.com/api/oauth2/authorize?client_id=YOUR_CLIENT_ID&permissions=274877975552&scope=bot%20applications.commands
```

Replace `YOUR_CLIENT_ID` with your Application ID from the General Information page.

</details>

---

## 📖 Usage Examples

**Basic text-to-image:**
```
/generate prompt:a beautiful sunset over the ocean
```

**Choose a workflow:**
```
/generate prompt:anime girl with blue hair workflow:Anime Style
```

**Custom parameters:**
```
/generate prompt:cyberpunk cityscape steps:30 cfg:7 sampler:dpmpp_2m scheduler:karras
```

**Full control:**
```
/generate prompt:portrait of a warrior width:768 height:1024 steps:50 cfg:8
```

**Image-to-image** (attach a file):
```
/generate prompt:make this look like a watercolor painting image1:[upload] denoise:0.7
```

**Prefix command with workflow:**
```
!generate "Realistic Photo" a forest path in autumn
```

### What It Looks Like

```
You:  /generate prompt:a pirate ship sailing through a galaxy
Bot:  🎨 Queuing your prompt: `a pirate ship sailing through a galaxy`
Bot:  ⏳ Generating... (Queue position: 1)
Bot:  ⏳ Generating... 30%
Bot:  ⏳ Generating... 60%
Bot:  📥 Retrieving images...
Bot:  ✅ Done!
Bot:  [Generated Image]
      Prompt: a pirate ship sailing through a galaxy
      Requested by: @PromptPirate
```

---

## 🔧 Troubleshooting

<details>
<summary><b>Bot doesn't respond to commands</b></summary>

- Enable **Message Content Intent** in Discord Developer Portal → Bot → Privileged Gateway Intents
- Verify the bot has **Send Messages** permission in your server
- Check that the bot shows as online in your server member list

</details>

<details>
<summary><b>Cannot connect to ComfyUI</b></summary>

- Make sure ComfyUI is running — `http://127.0.0.1:8188` should load in your browser
- Check firewall settings if using a remote instance
- For remote access, launch ComfyUI with: `python main.py --listen 0.0.0.0`

</details>

<details>
<summary><b>No workflows found</b></summary>

- Export workflows using **Save (API Format)**, not regular Save
- Place `.json` files in the `workflows/` directory
- Run `/reload` to rescan

</details>

<details>
<summary><b>Images not generating</b></summary>

- Open your workflow `.json` and verify node structure
- Make sure all required models/checkpoints are installed in ComfyUI
- Check the console output for error details

</details>

<details>
<summary><b>Progress updates not showing</b></summary>

- WebSocket connection may be blocked — check firewall for `ws://127.0.0.1:8188/ws`
- The bot will still work without WebSocket, just without live progress

</details>

---

## 📂 Project Structure

```
comfyui-discord-bot/
├── comfyui_discord_bot.py     # Main bot script
├── requirements.txt           # Python dependencies
├── run_bot.bat                # Windows launcher
├── .env                       # Configuration (create this)
├── models_database.json       # Auto-generated model/LoRA cache
├── workflows/                 # Drop your workflow JSONs here
│   ├── z-image-turbo_text-to-image.json
│   └── ...
└── assets/
    └── logo.png               # Project logo
```

---

## 🗺️ Roadmap

- [x] Text-to-image generation
- [x] Multi-workflow support with autocomplete
- [x] Image-to-image workflows
- [x] LoRA browser with previews
- [x] Model/checkpoint browser
- [x] Slash commands + prefix commands
- [x] Real-time progress via WebSocket
- [ ] Outpainting / image editing (`/edit` command)
- [ ] Video generation workflows
- [ ] Midjourney-style parameters (`--ar 16:9`, `--style raw`)
- [ ] User preference memory
- [ ] Generation history gallery
- [ ] Prompt enhancement via AI

---

## 🤝 Contributing

Contributions are welcome! Some ideas:

- Add new workflow templates
- Implement Midjourney-style syntax parsing
- Add support for negative prompts as a parameter
- Create a web dashboard for queue monitoring
- Add user-specific generation limits/roles

---

## 📄 License

This project is provided as-is for personal and educational use.

---

<div align="center">

**Made with 🏴‍☠️ by [Prompt Pirate](https://github.com/promptpirate-x)**

*Powered by [ComfyUI](https://github.com/comfyanonymous/ComfyUI) and [discord.py](https://discordpy.readthedocs.io/)*

</div>
