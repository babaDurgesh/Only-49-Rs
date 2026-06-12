# 🤖 Telegram Subscription Bot

Ek powerful Telegram bot jo premium channels ka subscription manage karta hai — UPI payment, QR code, aur auto expiry ke saath.

---

## 🚀 Render.com Pe Deploy Kaise Karein

### Step 1 — GitHub Pe Code Upload Karo

1. [github.com](https://github.com) pe account banao (agar nahi hai)
2. **New Repository** banao — naam kuch bhi rakho (jaise `tg-sub-bot`)
3. Apna code upload karo — ya locally `git` use karo:

```bash
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/TUMHARA_USERNAME/tg-sub-bot.git
git push -u origin main
```

> 💡 **Zaroor yeh files rakho apne repo mein:**
> - `bot.py` — main bot code
> - `requirements.txt` — dependencies
> - `render.yaml` *(optional but recommended)*

---

### Step 2 — requirements.txt Banao

Project folder mein `requirements.txt` file banao aur yeh likho:

```
pyTelegramBotAPI
pymongo
qrcode[pil]
apscheduler
flask
Pillow
dnspython
```

---

### Step 3 — render.yaml Banao *(Optional but Recommended)*

```yaml
services:
  - type: worker
    name: telegram-bot
    env: python
    buildCommand: pip install -r requirements.txt
    startCommand: python bot.py
    envVars:
      - key: BOT_TOKEN
        sync: false
      - key: MONGO_URI
        sync: false
      - key: ADMIN_ID
        sync: false
      - key: CONTACT_USERNAME
        sync: false
```

> ⚠️ `type: worker` use karo — `web` nahi! Bot ko koi web traffic nahi chahiye, sirf background process chahiye.

---

### Step 4 — Render Pe Account Banao

1. [render.com](https://render.com) pe jao
2. **Sign Up** karo — GitHub se login karo (easiest)

---

### Step 5 — New Service Create Karo

1. Dashboard mein **"New +"** button dabao
2. **"Background Worker"** select karo

   > ⚠️ "Web Service" mat chunna — warna free tier mein 15 min baad sleep ho jayega

3. **"Connect a repository"** — apna GitHub repo select karo

---

### Step 6 — Build Settings Configure Karo

| Field | Value |
|-------|-------|
| **Name** | `telegram-bot` (kuch bhi) |
| **Environment** | `Python 3` |
| **Build Command** | `pip install -r requirements.txt` |
| **Start Command** | `python bot.py` |
| **Instance Type** | `Free` |

---

### Step 7 — Environment Variables Add Karo

"Environment" tab mein jaao aur yeh variables add karo:

| Key | Value | Example |
|-----|-------|---------|
| `BOT_TOKEN` | Tumhara bot token | `123456:ABCdef...` |
| `MONGO_URI` | MongoDB Atlas URI | `mongodb+srv://...` |
| `ADMIN_ID` | Tumhara Telegram user ID | `123456789` |
| `CONTACT_USERNAME` | Admin ka username (bina @) | `myusername` |
| `UPI_ID` | Tumhara UPI ID | `name@paytm` |

> 🔐 Yeh values kabhi GitHub pe mat daalo — sirf Render ke environment variables mein rakho!

---

### Step 8 — Deploy Karo

**"Create Background Worker"** button dabao — Render automatically:
- Code pull karega
- Dependencies install karega
- Bot start karega

Deploy hone mein **2-3 minute** lagte hain.

---

### Step 9 — Logs Check Karo

Deploy hone ke baad **"Logs"** tab mein dekho:

```
✅ Enhanced Bot is running...
```

Yeh message aaye toh bot successfully chal raha hai! 🎉

---

## 🍃 MongoDB Atlas Free Setup

Bot ke liye free database chahiye — MongoDB Atlas M0 tier bilkul free hai:

1. [mongodb.com/atlas](https://www.mongodb.com/atlas) pe account banao
2. **Free M0 Cluster** banao (AWS / Singapore region best hai India ke liye)
3. **Database User** banao — username & password yaad rakho
4. **Network Access** mein `0.0.0.0/0` add karo (Render ke IP allow karne ke liye)
5. **Connect → Drivers** se URI copy karo:

```
mongodb+srv://USERNAME:PASSWORD@cluster0.xxxxx.mongodb.net/sub_management
```

Ise `MONGO_URI` environment variable mein daalo.

---

## ⚠️ Important Notes

### Free Tier Limitations
- Render free tier mein **750 hours/month** milte hain Background Worker ke liye
- Agar zyada chahiye toh **$7/month** ke Starter plan pe upgrade karo

### Bot Ko 24x7 Rakhne Ka Tarika
- `render.yaml` mein `type: worker` use karo (web nahi)
- `keep_alive()` Flask server already code mein hai — yeh sirf Render web service ke liye tha, worker mein zaruri nahi
- Worker services sleep nahi hoti — 24x7 chalti hain ✅

### Code Update Kaise Karein
GitHub pe push karo — Render automatically redeploy kar dega:

```bash
git add .
git commit -m "update"
git push
```

---

## 📁 Project Structure

```
tg-sub-bot/
├── bot.py              # Main bot file
├── requirements.txt    # Python dependencies
├── render.yaml         # Render config (optional)
└── README.md           # Yeh file
```

---

## 🛠️ Admin Commands

| Command | Kaam |
|---------|------|
| `/start` | Admin panel kholo |
| `/add` | Naya channel add karo |
| `/channels` | Channels manage karo |
| `/uploadqr` | QR code upload karo |
| `/setupi` | UPI ID set karo |
| `/setwelcome` | Welcome message edit karo |
| `/broadcast` | Sab users ko message bhejo |
| `/forcesub` | Force subscribe channels |
| `/stats` | Bot statistics dekho |

---

## ❓ Troubleshooting

**Bot start nahi ho raha?**
→ Logs mein error dekho, `BOT_TOKEN` sahi hai ya nahi check karo

**MongoDB connection error?**
→ Atlas mein `0.0.0.0/0` Network Access add kiya hai ya nahi check karo

**Bot kuch ghanton baad band ho jata hai?**
→ "Web Service" ki jagah "Background Worker" use karo

**Payments approve nahi ho rahi?**
→ `ADMIN_ID` sahi set hai ya nahi verify karo (`/getid` bot se pata kar sakte ho)
