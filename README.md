quotexpy
pyTelegramBotAPI
pandas
numpy
schedule
websocket-client# Sistema-sinais-mercado-financeiro-
TELEGRAM_TOKEN = "SEU_TOKEN_DO_BOTFATHER"
QUOTEX_EMAIL = "seu@email.com"
QUOTEX_PASSWORD = "sua_senha"
QUOTEX_ACCOUNT_TYPE = 1  # 1 = DEMO
CANAL_TELEGRAM = "@seu_canal"import telebot, schedule, time
from quotexpy import Quotex

# Config
from config import *

bot = telebot.TeleBot(TELEGRAM_TOKEN)
client = Quotex(email=QUOTEX_EMAIL, password=QUOTEX_PASSWORD)
client.connect()
if QUOTEX_ACCOUNT_TYPE == 1: client.change_balance("PRACTICE")

def gerar_sinal():
    try:
        candles = client.get_candles("EURUSD", 60, 50)
        # (mesma lógica SMA9/SMA21 + RSI que já te passei)
        # ... (código completo eu te mando se quiser)
        return "CALL"  # ou "PUT"
    except: return None

def enviar():
    s = gerar_sinal()
    if s: bot.send_message(CANAL_TELEGRAM, f"SINAL AUTOMÁTICO\n{s} EURUSD\n{time.strftime('%H:%M')}")

schedule.every(5).minutes.do(enviar)
print("Bot rodando 24/7 na nuvem...")
enviar()
while True:
    schedule.run_pending()
    time.sleep(1)