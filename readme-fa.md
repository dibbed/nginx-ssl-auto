<div align="center">

# Nginx SSL Auto 🚀

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/dibbed/nginx-ssl-auto/graphs/commit-activity)

ابزار مدیریت خودکار گواهی SSL برای Nginx با استفاده از Let's Encrypt. این ابزار روند راه‌اندازی و مدیریت گواهی‌های SSL برای برنامه‌های وب شما را ساده می‌کند.

</div>

## ✨ ویژگی‌ها

- 🔐 **تولید خودکار گواهی SSL**: استفاده از Let's Encrypt برای تولید گواهی‌های رایگان
- 🌐 **مدیریت پیکربندی Nginx**: ایجاد و مدیریت خودکار فایل‌های پیکربندی Nginx
- 🔄 **هدایت خودکار HTTP به HTTPS**: امکان فعال یا غیرفعال کردن هدایت خودکار
- 🧪 **آزمون پورت‌ها**: تست اتصال پورت قبل از راه‌اندازی SSL
- 🛡️ **مدیریت خطا و بازگردانی**: بازگردانی خودکار در صورت بروز خطا
- ⚙️ **قابل تنظیم**: پیکربندی از طریق متغیرهای محیطی
- 🔧 **شناسایی ابزارها**: شناسایی و نصب خودکار ابزارهای مورد نیاز (Nginx، Certbot)
- 🖥️ **رابط خط فرمان**: استفاده آسان از طریق CLI
- 🚀 **اسکریپت ساده راه‌انداز**: استفاده سریع از طریق `run.py`
- 🔍 **نظارت بر گواهی SSL**: بررسی وضعیت انقضای گواهی

## 📋 پیش‌نیازها

- سیستم عامل لینوکس مبتنی بر Ubuntu/Debian
- Python 3.8 یا بالاتر
- دسترسی Sudo
- نام دامنه اشاره‌کننده به سرور شما
- برنامه وب در حال اجرا روی پورت مشخص

## 🚀 نصب

```bash
git clone https://github.com/dibbed/nginx-ssl-auto.git
cd nginx-ssl-auto
pip install -e .
```

### مراحل راه‌اندازی

1. **تنظیم متغیرهای محیطی**:

```bash
cp .env.example .env
# ویرایش فایل .env با تنظیمات خود
```

2. **نصب پیش‌نیازهای سیستم** (اگر قبلاً نصب نشده):

```bash
sudo apt-get update
sudo apt-get install nginx certbot python3-certbot-nginx
```

## ⚙️ متغیرهای محیطی

| متغیر                      | مقدار پیش‌فرض                | توضیح                              |
| -------------------------- | ---------------------------- | ---------------------------------- |
| `NGINX_SITES_AVAILABLE`    | `/etc/nginx/sites-available` | مسیر دایرکتوری سایت‌های Nginx      |
| `NGINX_SITES_ENABLED`      | `/etc/nginx/sites-enabled`   | مسیر فعال‌سازی سایت‌های Nginx      |
| `LETSENCRYPT_EMAIL_DOMAIN` | `admin`                      | ایمیل برای اعلان‌های Let's Encrypt |
| `LETSENCRYPT_WEBROOT`      | `/var/www/html`              | مسیر وب روت برای چالش‌های ACME     |
| `SSL_PROTOCOLS`            | `TLSv1.2 TLSv1.3`            | پروتکل‌های SSL                     |
| `SSL_CIPHERS`              | `HIGH:!aNULL:!MD5`           | رمزنگاری SSL                       |
| `DEFAULT_HTTP_PORT`        | `80`                         | پورت HTTP پیش‌فرض                  |
| `DEFAULT_HTTPS_PORT`       | `443`                        | پورت HTTPS پیش‌فرض                 |
| `SUDO_COMMAND`             | `sudo`                       | مسیر دستور Sudo                    |
| `APT_GET_COMMAND`          | `apt-get`                    | دستور مدیریت پکیج                  |
| `SYSTEMCTL_COMMAND`        | `systemctl`                  | دستور کنترل سرویس                  |
| `PORT_TEST_TIMEOUT`        | `10`                         | زمان انتظار برای تست پورت (ثانیه)  |

## 📖 استفاده

### اسکریپت ساده راه‌انداز (پیشنهادی)

```bash
# نصب گواهی SSL
python run.py example.com 3000 setup

# حذف گواهی SSL
python run.py example.com 3000 remove

# بررسی تاریخ انقضای گواهی
python run.py example.com 3000 check

# تبدیل خودکار دامنه به حروف کوچک
python run.py EXAMPLE.COM 3000 setup  # به example.com تبدیل می‌شود
```

### رابط خط فرمان

```bash
# نصب گواهی SSL
nginx-ssl-auto setup example.com 3000

# نصب با گزینه‌ها
nginx-ssl-auto setup example.com 3000 --no-redirect --test-port

# حذف گواهی SSL
nginx-ssl-auto remove example.com

# بررسی تاریخ انقضای گواهی
nginx-ssl-auto check example.com

# نمایش پیکربندی فعلی
nginx-ssl-auto config
```

### API پایتون

```python
from nginx_ssl_auto import setup_ssl_certificate, remove_ssl_certificate, check_ssl_expiry

# نصب گواهی SSL برای یک دامنه
result = setup_ssl_certificate(
    domain_name="example.com",
    forward_port=3000,
    ssl_redirect=True,
    test_port=True
)

if result["mode"]:
    print("گواهی SSL با موفقیت نصب شد!")
else:
    print(f"خطا: {result['error']}")

# حذف گواهی SSL
result = remove_ssl_certificate("example.com")
if result["mode"]:
    print(result["message"])

# بررسی تاریخ انقضای گواهی
result = check_ssl_expiry("example.com")
if result["success"]:
    if result["is_active"]:
        print("گواهی SSL فعال و معتبر است")
    else:
        print("گواهی SSL منقضی شده است")
else:
    print(f"خطا: {result['error']}")
```

### استفاده پیشرفته

```python
from nginx_ssl_auto import SSLCertificateManager

# استفاده مستقیم از کلاس مدیریت
manager = SSLCertificateManager("example.com", 3000)
result = manager.setup_ssl_certificate(ssl_redirect=False, test_port=True)

# پیکربندی دلخواه با متغیرهای محیطی
import os
os.environ['LETSENCRYPT_EMAIL_DOMAIN'] = 'webmaster'
os.environ['SSL_PROTOCOLS'] = 'TLSv1.3'
os.environ['PORT_TEST_TIMEOUT'] = '15'

result = setup_ssl_certificate(
    domain_name="myapp.com",
    forward_port=8080,
    ssl_redirect=False,
    test_port=True
)

# بررسی تاریخ انقضای گواهی
result = check_ssl_expiry("myapp.com")
if result["success"]:
    if result["is_active"]:
        print("✅ گواهی SSL فعال و معتبر است")
    else:
        print("⚠️  گواهی SSL منقضی شده است")
else:
    print(f"❌ خطا: {result['error']}")
```

## 📁 ساختار پروژه

```
nginx-ssl-auto/
├── nginx_ssl_auto/ # پکیج اصلی
│   ├── **init**.py # مقداردهی اولیه پکیج
│   ├── core.py # عملکرد اصلی
│   └── cli.py # رابط خط فرمان
├── tests/ # تست‌ها
│   ├── **init**.py
│   ├── test_core.py # تست عملکرد اصلی
│   └── test_cli.py # تست CLI
├── .env.example # قالب متغیرهای محیطی
├── .gitignore # فایل‌های نادیده گرفته شده گیت
├── LICENSE # مجوز MIT
├── README.md # این فایل
├── pyproject.toml # پیکربندی پروژه
├── requirements.txt # وابستگی‌ها
└── run.py # اسکریپت ساده راه‌انداز
```

## 🔧 نمونه پیکربندی

### محیط توسعه

```bash
# .env برای توسعه
LETSENCRYPT_EMAIL_DOMAIN=dev
LETSENCRYPT_WEBROOT=/var/www/dev
PORT_TEST_TIMEOUT=5
```

### محیط تولید

```bash
# .env برای تولید
LETSENCRYPT_EMAIL_DOMAIN=admin
SSL_PROTOCOLS=TLSv1.3
SSL_CIPHERS=HIGH:!aNULL:!MD5:!RC4:!MD5:!aNULL
PORT_TEST_TIMEOUT=30
```

## 🧪 تست

```bash
# نصب وابستگی‌های توسعه
pip install -e ".[dev]"

# اجرای تست‌ها
pytest

# اجرای تست‌ها با پوشش‌دهی کد
pytest --cov=nginx_ssl_auto

# اجرای تست خاص
pytest tests/test_core.py::TestSSLCertificateManager::test_validate_domain_name_valid
```

## 🛠️ رفع مشکلات

### مشکلات رایج

1. **عدم دسترسی**: مطمئن شوید دسترسی Sudo دارید

   ```bash
   sudo -l
   ```

2. **Nginx یافت نشد**: نصب Nginx در صورت نبود

   ```bash
   sudo apt-get install nginx
   ```

3. **Certbot یافت نشد**: نصب Certbot

   ```bash
   sudo apt-get install certbot python3-certbot-nginx
   ```

4. **پورت قابل دسترسی نیست**: بررسی اجرای برنامه شما

   ```bash
   netstat -tlnp | grep :3000
   ```

5. **OpenSSL یافت نشد**: نصب OpenSSL برای بررسی گواهی
   ```bash
   sudo apt-get install openssl
   ```

### حالت اشکال‌زدایی

فعال کردن لاگینگ دیباگ:

```bash
export PYTHONPATH=.
python3 -c "
import logging
logging.basicConfig(level=logging.DEBUG)
from nginx_ssl_auto import setup_ssl_certificate, check_ssl_expiry
setup_ssl_certificate('example.com', 3000)
check_ssl_expiry('example.com')
"
```

## 🤝 مشارکت

1. Fork کردن مخزن
2. ایجاد شاخه ویژگی (`git checkout -b feature/amazing-feature`)
3. ثبت تغییرات (`git commit -m 'افزودن ویژگی جدید'`)
4. Push به شاخه (`git push origin feature/amazing-feature`)
5. باز کردن Pull Request

### راه‌اندازی محیط توسعه

```bash
# کلون و راه‌اندازی محیط توسعه
git clone https://github.com/dibbed/nginx-ssl-auto.git
cd nginx-ssl-auto
python -m venv venv
source venv/bin/activate  # ویندوز: venv\Scripts\activate
pip install -e ".[dev]"
```

## 📄 مجوز

این پروژه تحت مجوز MIT است - جزئیات در [LICENSE](LICENSE)

## 📞 تماس

- **نویسنده**: dibbed
- **GitHub**: [@dibbed](https://github.com/dibbed)
- **لینک پروژه**: [https://github.com/dibbed/nginx-ssl-auto](https://github.com/dibbed/nginx-ssl-auto)

## 🙏 قدردانی

- [Let's Encrypt](https://letsencrypt.org/) برای ارائه گواهی‌های رایگان
- [Certbot](https://certbot.eff.org/) برای ابزار اتوماسیون گواهی
- [Nginx](https://nginx.org/) برای وب سرور

---

<div align="center">

📄 For English version [click here](README.md)

</div>

⭐ اگر این پروژه به شما کمک کرد، لطفاً یک ستاره بدهید!
