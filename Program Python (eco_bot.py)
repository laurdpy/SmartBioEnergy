import requests
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
from datetime import datetime
import pytz

# Telegram Bot Token
BOT_TOKEN = "7628970118:AAGSvVTaEM27nMTYJ-TootRpg-PZIEicCiM"

# Daftar device dan token ThingsBoard
DEVICES = {
    "Sensor MQ-4 (Gas Metana)": "sensormq4",
    "Sensor DHT22": "dht22sensor",
    "pH Sensor": "phsensor",
    "Pressure Sensor": "pressuresensor"
}

# Fungsi ambil data dari ThingsBoard untuk 1 device
def get_device_data(token):
    try:
        url = f"https://demo.thingsboard.io/api/v1/{sensormq4}/telemetry"
        response = requests.get(url)
        data = response.json()
        return {
            "metana": data.get("metana", [{"value": "?"}])[0]["value"],
            "suhu": data.get("suhu", [{"value": "?"}])[0]["value"],
            "ph": data.get("ph", [{"value": "?"}])[0]["value"],
            "kelembaban": data.get("kelembaban", [{"value": "?"}])[0]["value"],
            "tekanan": data.get("tekanan", [{"value": "?"}])[0]["value"]
        }
    except Exception as e:
        print(f"Gagal ambil data dari device {token}:", e)
        return None

# /start
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "🌱 Selamat datang di EcoDigister IoT Bot! 🌱\n\n"
        "Bot ini mendukung proyek:\n"
        "*GREEN ENERGY REVOLUTION: GENERATOR BIOGAS BERBASIS INTERNET OF THINGS SEBAGAI SOLUSI CERDAS PENGELOLAAN LIMBAH ORGANIK*\n\n"
        "Gunakan /bantuan untuk melihat fitur yang tersedia.",
        parse_mode='Markdown'
    )

# /status
async def status(update: Update, context: ContextTypes.DEFAULT_TYPE):
    pesan = "📡 *Status Semua Digester:*\n\n"
    for nama, token in DEVICES.items():
        data = get_device_data(token)
        if data:
            pesan += (
                f"🔹 *{nama}*\n"
                f"💨 CH4: {data['metana']} %\n"
                f"🌡️ Suhu: {data['suhu']} °C\n"
                f"💧 Kelembaban: {data['kelembaban']} %\n"
                f"⚗️ pH: {data['ph']}\n"
                f"📈 Tekanan: {data['tekanan']} kPa\n\n"
            )
        else:
            pesan += f"🔹 *{nama}*\n⚠️ Gagal mengambil data.\n\n"
    await update.message.reply_text(pesan, parse_mode='Markdown')

# /update
async def update_data(update: Update, context: ContextTypes.DEFAULT_TYPE):
    jakarta = pytz.timezone("Asia/Jakarta")
    now = datetime.now(jakarta)
    waktu = now.strftime("%d %B %Y, pukul %H:%M WIB")
    pesan = f"📥 *Pembaruan Terakhir:*\n\n🗓️ {waktu}\n\n"
    for nama, token in DEVICES.items():
        data = get_device_data(token)
        if data:
            try:
                aman = all([
                    30 <= float(data['suhu']) <= 45,
                    6.5 <= float(data['ph']) <= 8,
                    40 <= float(data['kelembaban']) <= 80,
                    float(data['tekanan']) <= 50,
                    float(data['metana']) >= 50
                ])
                status = "✅ Stabil" if aman else "⚠️ Tidak Stabil"
            except:
                status = "❓ Data tidak valid"
            pesan += f"🔹 *{nama}*: {status}\n"
        else:
            pesan += f"🔹 *{nama}*: ⚠️ Data tidak tersedia\n"
    await update.message.reply_text(pesan, parse_mode='Markdown')

# /grafik
async def grafik(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "🌐 Lihat grafik di ThingsBoard:\n"
        "https://demo.thingsboard.io/dashboard/11c066d0-144b-11f0-93f2-e96356202050?publicId=030f8290-a9a9-11ef-b5a8-ed1aed9a651f"
    )

# /bantuan
async def bantuan(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "📘 *Bantuan - Daftar Perintah:*\n\n"
        "/start - Mulai menggunakan bot\n"
        "/status - Lihat status semua digester\n"
        "/update - Pembaruan terbaru\n"
        "/grafik - Lihat grafik real-time\n"
        "/tentang - Info proyek\n"
        "/kontak - Hubungi pengembang",
        parse_mode='Markdown'
    )

# /tentang
async def tentang(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "♻ Tentang EcoDigister IoT Bot ♻\n\n"
        "Bot ini merupakan bagian dari proyek GREEN ENERGY REVOLUTION: Generator Biogas Berbasis Internet of Things-"
        "sebuah inovasi cerdas dalam pengelolaan limbah organik.\n\n"
        "Dengan memanfaatkan sensor real-time dan teknologi IoT, EcoDigister membantu memantau proses produksi biogas secara digital, "
        "sekaligus memberikan notifikasi otomatis apabila parameter lingkungan seperti metana, suhu, pH, kelembaban, dan tekanan berada di luar ambang batas.\n\n"
        "🌱 Tujuan utama kami: menciptakan sistem energi terbarukan yang ramah lingkungan, efisien, dan terintegrasi dengan teknologi terkini.\n\n"
        "🚀 Dikembangkan oleh tim kreatif untuk mendukung green technology dan sustainable future.\n\n"
        "Gunakan command /status untuk melihat kondisi biogas secara langsung!\n"
        "\n📡 EcoDigister IoT – Smart Monitoring for a Greener Tomorrow",
        parse_mode='Markdown'
    )

# /kontak
async def kontak(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "📞 Kontak Pengembang: 082242210846 (Della)\n"
        "👤 Capek Prigel Team\n"
        "📧 ecodigister.iot@gmail.com\n"
        "📱 Telegram: @EcoDigisterIoTBot"
    )

# Setup Bot
app = ApplicationBuilder().token(BOT_TOKEN).build()
app.add_handler(CommandHandler("start", start))
app.add_handler(CommandHandler("status", status))
app.add_handler(CommandHandler("update", update_data))
app.add_handler(CommandHandler("grafik", grafik))
app.add_handler(CommandHandler("bantuan", bantuan))
app.add_handler(CommandHandler("tentang", tentang))
app.add_handler(CommandHandler("kontak", kontak))

print("✅ EcoDigister IoT Bot Multi-Device aktif...")
app.run_polling()
