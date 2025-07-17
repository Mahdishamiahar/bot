from telebot import TeleBot, types

TOKEN = "7227213916:AAEqslEJI7dhdWyLrMTVeUrnK9Sa60WjYnY"
CHANNEL_ID = "VIPZEXNET"

bot = TeleBot(TOKEN)

# بررسی عضویت در کانال
def is_member(user_id):
    try:
        member = bot.get_chat_member(f"@{CHANNEL_ID}", user_id)
        return member.status in ['member', 'administrator', 'creator']
    except:
        return False

# شروع ربات
@bot.message_handler(commands=['start'])
def send_welcome(message):
    user_id = message.from_user.id

    if is_member(user_id):
        # دکمه‌های Reply Keyboard
        markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
        markup.add(
            types.KeyboardButton("💬 پشتیبانی"),
            types.KeyboardButton("🌐 سایت رسمی ربات"),
            types.KeyboardButton("📥 دریافت نرم‌افزار"),
            types.KeyboardButton("💳 خرید سرویس"),
            types.KeyboardButton("📲 باز کردن برنامه")
        )
        bot.send_message(user_id, "خوش آمدید به ربات VIPZEXNET 🌟", reply_markup=markup)

    else:
        join_button = types.InlineKeyboardMarkup()
        btn = types.InlineKeyboardButton("📢 عضویت در کانال", url="https://t.me/VIPZEXNET")
        join_button.add(btn)
        bot.send_message(user_id, "🌟 برای استفاده از ربات لطفاً ابتدا در کانال عضو شوید:", reply_markup=join_button)

# پردازش دکمه‌ها
@bot.message_handler(func=lambda m: True)
def handle_buttons(message):
    if message.text == "💬 پشتیبانی":
        bot.send_message(message.chat.id, "ارتباط با پشتیبانی: @Supp_ort_VIPZEXNET")
    elif message.text == "🌐 سایت رسمی ربات":
        bot.send_message(message.chat.id, "🌐 سایت رسمی: https://mahdishamiahar.github.io/Web.VIPZEXNET/")
    elif message.text == "📥 دریافت نرم‌افزار":
        bot.send_message(message.chat.id, "📥 دانلود نرم‌افزار:\nhttps://github.com/Mahdishamiahar/app.VIPZEXNET.git")
    elif message.text == "💳 خرید سرویس":
        bot.send_message(message.chat.id, "💳 خرید از طریق پشتیبانی:\n@Supp_ort_VIPZEXNET")
    elif message.text == "📲 باز کردن برنامه":
        # دکمه باز کردن برنامه به صورت inline
        markup = types.InlineKeyboardMarkup()
        btn = types.InlineKeyboardButton("ورود به برنامه", url="https://mahdishamiahar.github.io/VIPZEXNETbot/")
        markup.add(btn)
        bot.send_message(message.chat.id, "برای ورود به برنامه روی دکمه زیر کلیک کنید:", reply_markup=markup)
    else:
        bot.send_message(message.chat.id, "لطفاً یکی از گزینه‌ها را انتخاب کنید.")

# اجرای ربات
bot.infinity_polling()
