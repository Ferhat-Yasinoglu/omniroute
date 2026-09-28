# FerhatRoute

> نسخه شخصی فرهات از OmniRoute. یک دروازه هوش مصنوعی که روی سیستم خودم اجرا می‌شود.
>
> _FerhatRoute is my personal edition of OmniRoute, a self-hosted AI gateway. This file is written in Persian._

**FerhatRoute** همان OmniRoute است که برای استفاده شخصی خودم نگه‌داری می‌کنم. کد اصلی از پروژه
[diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute) با مجوز MIT گرفته شده و این مخزن
یک fork از آن است: [Ferhat-Yasinoglu/omniroute](https://github.com/Ferhat-Yasinoglu/omniroute).

## این چیست؟

یک آدرس محلی می‌دهد که با API اوپن‌ای‌آی، Claude و Gemini سازگار است. هر ابزاری که با OpenAI API کار
می‌کند به آن وصل می‌شود و درخواست‌ها بین صدها سرویس مدل زبانی مسیریابی می‌شوند. اگر یک سرویس خراب
شود یا سهمیه‌اش تمام شود، خودکار به سرویس سالم بعدی می‌رود.

- **یک endpoint برای همه ابزارها:** `http://localhost:20128/v1`
- **شروع بدون کلید:** مدل `auto` بلافاصله بعد از نصب جواب می‌دهد.
- **fallback خودکار** با سه لایه مقاومت: circuit breaker، cooldown و model lockout.
- **فشرده‌سازی خودکار توکن** با موتورهای RTK و Caveman.
- **همه‌چیز محلی:** داده‌ها و کلیدها روی سیستم خودم می‌مانند و رمزنگاری می‌شوند.

## نصب و اجرا

پیش‌نیاز: Node.js نسخه ۲۲ یا ۲۴. نسخه ۲۳ پشتیبانی نمی‌شود.

### راه ۱: نصب سراسری با npm (ساده‌ترین)

```bash
npm i -g omniroute
omniroute
```

### راه ۲: Docker

```bash
docker run -d --name omniroute --restart unless-stopped --stop-timeout 40 \
  -p 127.0.0.1:20128:20128 -v omniroute-data:/app/data diegosouzapw/omniroute:latest
```

### راه ۳: از سورس همین مخزن

```bash
git clone https://github.com/Ferhat-Yasinoglu/omniroute
cd omniroute
cp .env.example .env && npm install
PORT=20128 npm run dev
```

## بعد از نصب

| مورد                  | مقدار                                                            |
| --------------------- | ---------------------------------------------------------------- |
| داشبورد               | `http://localhost:20128`                                         |
| آدرس API برای ابزارها | `http://localhost:20128/v1`                                      |
| رمز اولیه             | متغیر `INITIAL_PASSWORD` (پیش‌فرض در `.env.example`: `CHANGEME`) |
| پوشه داده             | `~/.omniroute/` یا مقدار متغیر `DATA_DIR`                        |
| پورت                  | متغیر `PORT` (پیش‌فرض `20128`)                                   |

بعد از اولین ورود به داشبورد، رمز را عوض کن.

تست سریع:

```bash
curl http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"auto","messages":[{"role":"user","content":"سلام!"}]}'
```

## اتصال ابزارها

در هر ابزاری که با OpenAI API کار می‌کند (Claude Code، Cursor، Cline و مانند آن) آدرس پایه را روی
`http://localhost:20128/v1` بگذار و به جای اسم مدل از `auto` یا یکی از واریانت‌هایش استفاده کن:

| مدل           | کاربرد                    |
| ------------- | ------------------------- |
| `auto`        | پیش‌فرض متعادل            |
| `auto/coding` | اولویت کیفیت برای کدنویسی |
| `auto/fast`   | کمترین تأخیر              |
| `auto/cheap`  | ارزان‌ترین گزینه          |

## متغیرهای مهم `.env`

| متغیر              | کار                                                |
| ------------------ | -------------------------------------------------- |
| `PORT`             | پورت سرور، پیش‌فرض `20128`                         |
| `DATA_DIR`         | پوشه داده، پیش‌فرض `~/.omniroute/`                 |
| `INITIAL_PASSWORD` | رمز اولیه داشبورد                                  |
| `JWT_SECRET`       | امضای توکن ورود، ساخت با `openssl rand -base64 48` |
| `API_KEY_SECRET`   | رمز کلیدهای API، ساخت با `openssl rand -hex 32`    |
| `REQUIRE_API_KEY`  | اجبار کلید API برای درخواست‌ها، پیش‌فرض `false`    |
| `APP_LOG_LEVEL`    | سطح لاگ، پیش‌فرض `info`                            |

## گرفتن آپدیت از پروژه اصلی

```bash
git remote add upstream https://github.com/diegosouzapw/OmniRoute
git fetch upstream
git merge upstream/main
```

## یادداشت‌های من

- [ ] رمز اولیه را عوض کنم.
- [ ] کلید یا اکانت سرویس‌هایی که دارم را در داشبورد اضافه کنم.
- [ ] یک combo شخصی برای کدنویسی بسازم.

## مجوز

MIT، مثل پروژه اصلی. حق نشر کد اصلی برای diegosouzapw است و متن مجوز در فایل `LICENSE` نگه داشته
می‌شود.
