# Create the bot

## **Create the Bot**

The Telegram or Discord bot is used to send alerts, reports, and real-time updates from your Cardano tracker.\
It connects directly to your hosting script (Google Apps Script, Python, or Node.js).

***

### ⚙️ **Telegram Bot**

1. **Open Telegram** and search for **@BotFather**.
2. Type `/newbot` and follow the instructions:
   * Give your bot a name
   * Choose a username ending with `_bot` (e.g., `CardanoTracker_bot`)
3.  **Copy the API Token** BotFather gives you — it looks like:

    ```
    1234567890:ABCdEfGhIjKlmNoPqRsTuvWxYz12345678
    ```
4. **Store the token securely** in your environment variables or a protected config file.

***

#### 💻 **Basic Example**

**Google Apps Script**

```js
function sendMessage(chatId, text) {
  const token = "<YOUR_BOT_TOKEN>";
  const url = `https://api.telegram.org/bot${token}/sendMessage`;
  const payload = {
    method: "post",
    contentType: "application/json",
    payload: JSON.stringify({ chat_id: chatId, text: text })
  };
  UrlFetchApp.fetch(url, payload);
}
```

**Python**

```python
import requests

token = "<YOUR_BOT_TOKEN>"
chat_id = "<YOUR_CHAT_ID>"

def send_message(text):
    url = f"https://api.telegram.org/bot{token}/sendMessage"
    requests.post(url, json={"chat_id": chat_id, "text": text})
```

***

#### ✅ **Next**

Once your bot works, connect it with your **output logic** to send:

* Auto alerts (e.g., ADA balance changes)
* Daily summaries
* Transaction notifications

***

### ⚙️ **Discord Bot**

The Discord bot works similarly to the Telegram bot — it delivers notifications, reports, or custom messages from your Cardano tracker directly into your Discord channels.\
It is optional but useful for community or team-based tracking setups.

***

#### ⚙️ **Steps to Create a Discord Bot**

1. **Go to the Discord Developer Portal.**
2. Click **“New Application”** → give your bot a name (e.g., `CardanoTrackerBot`).
3. Go to the **Bot** tab → click **“Add Bot.”**
4.  Copy the **Bot Token** — you’ll use it in your code.

    > ⚠️ Treat this token as a password. Do **not** share or upload it publicly.
5. Under **OAuth2 → URL Generator**, select:
   * **Scopes:** `bot`
   * **Bot Permissions:** `Send Messages`, `Embed Links`, `Read Message History`
6. Copy the generated **Invite Link** and use it to add the bot to your Discord server.

***

#### 💻 **Basic Example**

**Python (discord.py)**

```python
import discord
import os

TOKEN = os.getenv("DISCORD_TOKEN")  # store your token securely

intents = discord.Intents.default()
client = discord.Client(intents=intents)

@client.event
async def on_ready():
    print(f'Bot connected as {client.user}')

@client.event
async def on_message(message):
    if message.content.startswith("!ping"):
        await message.channel.send("Pong 🏓")

def send_update(channel_id, content):
    channel = client.get_channel(channel_id)
    if channel:
        asyncio.run_coroutine_threadsafe(channel.send(content), client.loop)

client.run(TOKEN)
```

***

#### 🧩 **Integration with Hosting Layer**

* The **Hosting Layer** (Google App Script / Node.js / Python) can call the Discord bot using its REST API or Webhook.
* For simple notifications, you can use **Discord Webhooks** instead of a full bot:

**Webhook Example**

```js
function sendDiscordWebhook(content) {
  const webhookUrl = "<YOUR_DISCORD_WEBHOOK_URL>";
  const payload = { content: content };
  UrlFetchApp.fetch(webhookUrl, {
    method: "post",
    contentType: "application/json",
    payload: JSON.stringify(payload),
  });
}
```

***

#### ✅ **Next**

Once connected, your tracker can:

* Post ADA or wallet updates to a Discord channel
* Share daily summaries or alerts
* Integrate both **Telegram and Discord** for multi-channel notifications
