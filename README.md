# Telegram bot 
requirements.txt
telethon
python-dotenv
flask
#main.py
from telethon import TelegramClient, events
from telegram import Bot
import asyncio
import re
from keep_alive import keep_alive

# --- Telegram Credentials ---
API_ID = 26274677
API_HASH = "cbfdb766be065635f71c9b331c416dca"
BOT_TOKEN = "7562031573:AAHXAbl2zogqdbHNhk4E2U_L6rhHYfRMDEY"

# --- Channels ---
SOURCE_CHANNEL = "@binance_box_channel"
TARGET_CHANNEL = "@RedpacketKingdom"

# --- Telegram Clients ---
client = TelegramClient("user_session", API_ID, API_HASH)
bot = Bot(token=BOT_TOKEN)

# --- Remove leading emoji ---
def remove_leading_emoji(text):
    return re.sub(r"^[^\w\s]*", "", text).strip()

# --- Message Handler ---
@client.on(events.NewMessage(chats=SOURCE_CHANNEL))
async def handler(event):
    raw_text = event.raw_text.strip()
    cleaned = remove_leading_emoji(raw_text)
    if len(cleaned.split()) == 1 and len(cleaned) >= 8:
        final = f"🎁 {cleaned}"
        try:
            await bot.send_message(chat_id=TARGET_CHANNEL, text=final)
            print("Sent:", final)
        except Exception as e:
            print("Error sending:", e)

# --- Main Event Loop ---
async def main():
    await client.start()
    print("Userbot is running...")
    await client.run_until_disconnected()

# --- Run Bot ---
keep_alive()
asyncio.run(main())

#keep_alive.py
from flask import Flask
from threading import Thread

app = Flask('')

@app.route('/')
def home():
    return "Bot is alive!"

def run():
    app.run(host='0.0.0.0', port=5000, debug=False, threaded=True)

def keep_alive():
    Thread(target=run).start()
