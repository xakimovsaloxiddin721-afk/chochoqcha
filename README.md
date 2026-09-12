# SMM Bot (Telegram)

Virtual raqam, Telegram Stars, nakrutka (obunachi/layk/ko'rish) va gift/premium sotadigan Telegram bot.
Python + aiogram 3 asosida yozilgan, SQLite bilan ishlaydi (kichik/o'rta hajmdagi loyihalar uchun yetarli).

## 📁 Struktura

```
smm-bot/
├── bot/
│   ├── handlers/         # har bir menyu bo'yicha alohida fayl
│   │   ├── start.py
│   │   ├── balance.py
│   │   ├── topup.py          # HUMO/UZCARD (admin tasdig'i) + Telegram Stars
│   │   ├── gift_premium.py
│   │   ├── nakrutka.py
│   │   ├── support.py
│   │   └── admin.py
│   ├── keyboards/
│   │   ├── main_menu.py
│   │   └── inline.py
│   ├── services/
│   │   ├── sms_activate.py   # virtual raqam uchun stub (to'ldirish kerak)
│   │   └── fivesim.py        # virtual raqam uchun stub (to'ldirish kerak)
│   ├── database/
│   │   └── db.py             # aiosqlite bilan users/transactions/orders
│   └── states.py             # FSM holatlar
├── config.py                 # katalog narxlari, .env o'qish
├── main.py                   # bot ishga tushirish nuqtasi
├── requirements.txt
└── .env.example
```

## ⚙️ O'rnatish

```bash
git clone <your-repo-url>
cd smm-bot
python3 -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
```

`.env` faylini oching va to'ldiring:

```
BOT_TOKEN=           # @BotFather'dan olingan token
ADMIN_IDS=           # sizning Telegram user_id (bir nechta bo'lsa vergul bilan)
SUPPORT_GROUP_ID=    # ixtiyoriy
CARD_HUMO_NUMBER=
CARD_UZCARD_NUMBER=
CARD_OWNER_NAME=
```

Botni ishga tushirish:

```bash
python main.py
```

## 💳 To'lov tizimi

- **HUMO / UZCARD** — hozircha qo'lda tasdiqlanadi: foydalanuvchi chek yuboradi → admin guruhga/shaxsiy chatga tushadi → admin ✅/❌ tugmasi bilan tasdiqlaydi → balans avtomatik qo'shiladi.
- **Telegram Stars** — to'liq avtomatik, `provider_token` shart emas, Telegram o'zi qayta ishlaydi (`currency="XTR"`).
- Kelajakda **Payme** yoki **Click** API qo'shish uchun `bot/services/` ichiga alohida fayl qo'shib, `topup.py`'dagi `topup_methods_kb()`'ga yangi tugma qo'shishning o'zi kifoya.

## 🚀 Nakrutka va Gift/Premium

- Narxlar `config.py` ichida — o'zgartirish uchun shu yerni tahrirlang.
- Buyurtmalar `orders` jadvaliga yoziladi va admin'ga yuboriladi; admin ✅ Bajarildi / ❌ Bekor qilish orqali holatni belgilaydi.
- Virtual raqam xizmatlari (SMS-Activate, 5sim) uchun `bot/services/` ichida stub fayllar bor — API kalitlaringizni qo'shib, real xaridni ulashingiz mumkin.

## ☁️ Deploy qilish

**VPS (tavsiya etiladi):**
```bash
sudo apt install python3-venv -y
git clone <repo> && cd smm-bot
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env  # to'ldiring
nohup python main.py &
```

24/7 ishlashi uchun `systemd` service yoki `pm2`/`screen` ishlatishni tavsiya qilamiz.

**Railway / Render:** repo'ni ulang, environment variables (`.env` ichidagilar) panel orqali kiriting, start command: `python main.py`.

## 🔧 GitHub'ga joylash

```bash
cd smm-bot
git init
git add .
git commit -m "Initial commit: SMM bot skeleton"
git branch -M main
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```

`.env` fayli `.gitignore`da — tokeningiz tasodifan push qilinmaydi.
