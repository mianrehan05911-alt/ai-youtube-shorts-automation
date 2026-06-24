# 🎬 AI YouTube Shorts Automation — The AI Pixel

A fully automated YouTube Shorts publishing system built with **n8n** that
handles everything from topic ideation to video upload — with zero manual
work after setup.

> Built in under 24 hours. 4 Shorts published automatically on Day 1.

---

## 🧠 Workflow Architecture

![n8n Workflow Canvas](n8n-canvas.png)

The system runs across 5 phases — Ideation, Production, Storage, Editing,
and Publishing — all connected inside a single n8n workflow with 16 nodes
working in sequence.

---

## 🔄 How It Works — Phase by Phase

### 🧠 Phase 1 — The AI Brain (Ideation & Planning)

| Node | Role | What It Does |
|------|------|-------------|
| ⏱️ Schedule Trigger | Ignition Switch | Starts the entire workflow automatically at the set time — no manual clicks needed |
| 🤖 Topic Generator Agent | Creative Director | Uses Groq LLM to generate a unique, trending topic for today's Short |
| ✍️ Script Writer Agent | Content Writer | Writes a highly engaging 30-second script based on the topic to hook the audience |
| 🔍 SEO Metadata Agent | SEO Expert | Reads the script and generates viral YouTube Title, Description, and Hashtags |
| ⚙️ Parse Metadata | Data Cleaner | Cleans the AI JSON output and separates title, tags, and script for next nodes |

### 🎬 Phase 2 — The Production House (Sourcing & Voice)

| Node | Role | What It Does |
|------|------|-------------|
| 🌐 Fetch Pexels Video | Stock Scout | Hits Pexels API and finds a high-quality background video matching the topic |
| 🔗 Extract Video URL | Link Extractor | Pulls only the direct .mp4 link from Pexels response data |
| 📥 HTTP Request | Video Downloader | Downloads the actual video file into n8n for processing |
| 🎙️ ElevenLabs TTS | Voice Artist | Converts the AI script into a professional human-like voiceover |

### 🗄️ Phase 3 — The Memory & Backup (Storage)

| Node | Role | What It Does |
|------|------|-------------|
| 📁 Save Audio to Drive | Cloud Backup | Uploads generated audio/video files to Google Drive automatically |
| 📊 Log to Google Sheets | Digital Ledger | Saves Date, Topic, Script, Title, and Drive links — full monthly record |

### 🎞️ Phase 4 — The Editor (Video Assembly)

| Node | Role | What It Does |
|------|------|-------------|
| 🎥 Creatomate | Video Editor | Merges Pexels video + ElevenLabs audio + adds dynamic auto-captions using template |
| ⏳ Wait Node | Buffer | Pauses workflow while Creatomate renders — prevents errors from early fetch |
| 🌐 HTTP Request 2 | Final Fetcher | Goes back to Creatomate and retrieves the fully rendered .mp4 file |

### 🚀 Phase 5 — The Publisher (Delivery & Alert)

| Node | Role | What It Does |
|------|------|-------------|
| ▶️ Upload a Video | YouTube Publisher | Uploads final video with SEO title and description directly to YouTube as a Short |
| 📲 Send a Text Message | Telegram Alert | Sends a formatted notification with Upload Time, Topic, Script, YouTube Link and Drive Backup |

---

## 🖼️ Screenshots

### n8n Workflow Canvas — All 16 Nodes
![n8n Canvas](n8n-canvas.png)

### YouTube Studio — Published Shorts
![Upload Proof](upload-proof.png)

### Telegram — Confirmation Message
![Telegram Confirmation](telegram-confirmation-messege.jpeg)

### Google Sheets — Content Log
![Google Sheets](google-sheet.png)

### Google Drive — Audio Backups
![Drive Audios](drive-audios.png)

---

## 🛠️ Full Tech Stack

| Tool | Purpose | Cost |
|------|---------|------|
| n8n (Self Hosted) | Workflow automation engine | Free |
| Groq LLM (llama3-70b) | Topic, Script & SEO generation | Free |
| Pexels API | Stock video sourcing | Free |
| ElevenLabs | Text to Speech voiceover | Free (10k chars/month) |
| Creatomate | Video + audio merging + auto captions | Paid/Trial |
| YouTube Data API v3 | Auto video upload | Free |
| Google Drive API | Audio/video cloud backup | Free |
| Google Sheets API | Content logging and tracking | Free |
| Telegram Bot API | Upload confirmation notifications | Free |

---

## ✨ Key Features

- ✅ Fully automated — zero manual work after setup
- ✅ AI generates unique topic, script, and SEO metadata daily
- ✅ Professional human-like voiceover via ElevenLabs
- ✅ High-quality stock video sourced automatically from Pexels
- ✅ Auto-captions added to every Short via Creatomate
- ✅ Published directly to YouTube with SEO-optimized metadata
- ✅ Every Short backed up to Google Drive
- ✅ Complete content log in Google Sheets
- ✅ Instant Telegram notification with YouTube link on every upload

---

## 📊 Proof of Working

**Day 1 Results — 4 Shorts Published Automatically:**

| Short Title | Status | Date |
|-------------|--------|------|
| The Top 5 Weird AI-Generated Art Styles That Will Blow Your Mind | ✅ Published | Jun 24, 2026 |
| The Hidden Dangers of Overusing AI-Generated Music | ✅ Published | Jun 24, 2026 |
| 10 Simple Life Hacks to Boost Your Morning Productivity | ✅ Published | Jun 24, 2026 |
| The Dark Side of AI: 3 Surprising Ways AI Can Manipulate | ✅ Published | Jun 24, 2026 |

---

## 📁 Repository Structure

```
ai-youtube-shorts-automation/
├── workflow.json                      → Importable n8n workflow (16 nodes)
├── n8n-canvas.png                     → Full workflow canvas screenshot
├── upload-proof.png                   → YouTube Studio showing published Shorts
├── telegram-confirmation-messege.jpeg → Telegram notification screenshot
├── google-sheet.png                   → Content log in Google Sheets
├── drive-audios.png                   → Audio backups in Google Drive
└── README.md                          → Project documentation
```

---

## 📦 Setup Instructions

### Step 1 — Accounts & API Keys Needed
- **Groq** — free at console.groq.com
- **Pexels API** — free at pexels.com/api
- **ElevenLabs** — free at elevenlabs.io (10k chars/month)
- **Creatomate** — creatomate.com (for video rendering + captions)
- **Google Cloud** — enable YouTube Data API v3 + Google Drive API + Google Sheets API
- **Telegram Bot** — create via @BotFather

### Step 2 — Self Host n8n
```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```
Add SSL (HTTPS) via Nginx + Let's Encrypt for Google & Telegram OAuth to work.

### Step 3 — Import Workflow
1. Open your n8n instance
2. Click **"New Workflow"**
3. Click **"..."** → **"Import from file"**
4. Select `workflow.json`

### Step 4 — Add Credentials
Connect these in n8n Settings → Credentials:
- Groq API Key
- Pexels API Key (in HTTP Request node header)
- ElevenLabs API Key (in HTTP Request node header)
- Creatomate API Key
- Google Drive OAuth2
- Google Sheets OAuth2
- YouTube OAuth2
- Telegram Bot Token

### Step 5 — Configure Nodes
- Update your **Telegram Chat ID** in the Send Message node
- Update your **Google Sheet ID** in the Log node
- Update your **Creatomate Template ID** in the Creatomate node
- Set your preferred **schedule time** in the Schedule Trigger node

### Step 6 — Activate & Test
- Turn on the workflow toggle
- Click **"Execute Workflow"** to test manually first
- Check Telegram for confirmation
- Check YouTube Studio for the uploaded Short

---

## 🎯 Purpose

This project was built to demonstrate advanced automation skills including:
- Multi-phase AI pipeline design (16 nodes)
- Integration of 9 different APIs in one workflow
- Real content production automation (not just a demo)
- Self-hosted n8n with HTTPS setup
- End-to-end media processing (script → voice → video → publish)

---

## 👤 Author

**Mian Rehan** — AI Automation Developer
[LinkedIn](https://www.linkedin.com/in/muhammad-rehan/) |
[GitHub](https://github.com/mianrehan05911-alt) |
[YouTube Channel](https://youtube.com/@TheAIPixel)
