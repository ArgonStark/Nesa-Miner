# Nesa-Miner

# استقرار یک نود ماینر روی تست‌نت شبکه نسا
> نسا یک بلاکچین لایه 1 است که به توسعه‌دهندگان امکان می‌دهد هر مدل هوش مصنوعی را برای راه‌اندازی اپلیکیشن غیرمتمرکز (Dapp) یا رول‌آپ بلاکچین خود روی شبکه‌اش یکپارچه کنند.
>
> با مطالعه وایت‌پیپر نسا، این شبکه به عنوان یک پلتفرم بلاکچین جامع برای یکپارچه‌سازی مدل‌های هوش مصنوعی در توسعه رول‌آپ‌ها و قراردادهای هوشمند پتانسیل بالایی را نشان می‌دهد.

## الزامات سخت‌افزاری
![image](https://github.com/user-attachments/assets/fc99390f-66cf-4931-8aca-675597fa0db4)
> استفاده از GPU ضروری نیست و می‌توانید ماینر خود را فقط با CPU اجرا کنید.

## مرحله 1: نصب پیش‌نیازها
### 1. نصب پیش‌نیازها
```console
sudo apt update && sudo apt upgrade -y
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
### 2. نصب داکر
```console
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# بررسی نسخه داکر
docker --version

# دادن مجوزهای لازم به داکر
sudo groupadd docker
sudo usermod -aG docker $USER
```

## مرحله 2: دریافت کلید API از Hugging Face
1. به [وب‌سایت Hugging Face](https://huggingface.co/) مراجعه کنید.

2. وارد حساب کاربری خود شوید یا ثبت‌نام کنید.

3. به صفحه [توکن‌های دسترسی API](https://huggingface.co/settings/tokens) در تنظیمات حساب خود بروید.

4. روی **"ایجاد توکن جدید"** کلیک کنید، به تب **"Write"** بروید و یک کلید API با کلیک روی **"ایجاد توکن"** ایجاد کنید.

5. کلید ایجاد شده را کپی کنید تا در نصب ماینر خود استفاده کنید.

## مرحله 3: دریافت کلید خصوصی LeapWallet
یک کیف پول کازموس در Leap Wallet ایجاد کرده و کلید خصوصی کیف پول خود را استخراج کنید.

![Screenshot_1](https://github.com/user-attachments/assets/876ae952-fb94-4f89-800d-6ca25631ca44)

## مرحله 4: باز کردن پورت‌ها
```console
sudo ufw allow ssh
sudo ufw allow 22
sudo ufw allow 31333
sudo ufw enable
```

## مرحله 5: نصب و اجرای ماینر نسا
```
bash <(curl -s https://raw.githubusercontent.com/nesaorg/bootstrap/master/bootstrap.sh)
```
* **انتخاب یک حالت**: حالت **Wizardy** را انتخاب کنید.
* **انتخاب یک نام کاربری (Moniker)**: یک نام منحصربه‌فرد برای نود خود وارد کنید.
* **Hostname نود**: رد کنید و `Enter` را بزنید.
* **ایمیل نود**: رد کنید و `Enter` را بزنید.
* **کد معرفی برای دریافت امتیاز بیشتر**: `nesa1yszutrgc7a06jq9h2ssu32cs86gpu94t2h8tgz`
* **کلید خصوصی کیف پول**: کلید خصوصی کیف پول خود را برای ثبت‌نام ماینر و دریافت پاداش وارد کنید.
* نهایی‌سازی تنظیمات: تنظیمات خود را مرور کرده و قبل از شروع نود تأیید کنید. اسکریپت بوت‌استرپ خلاصه‌ای از تنظیمات شما ارائه می‌دهد و امکان اعمال تغییرات قبل از شروع ماینر را فراهم می‌کند.

![image](https://github.com/user-attachments/assets/69540b5a-1461-41a4-8a20-6efe4d5686f7)

## مرحله 6: دستورات مفید

### مشاهده لاگ‌های کانتینر
```
docker logs -fn 100 orchestrator
```
```
docker logs -fn 100 mongodb
```

بررسی کانتینرها: اکنون باید 4 کانتینر جدید داشته باشید.
```
docker ps
```

### دریافت Peer-ID نود (کلید عمومی کیف پول)
```console
cat $HOME/.nesa/identity/node_id.id
```

### دریافت آدرس وضعیت نود
```
PUB_KEY=$(cat $HOME/.nesa/identity/node_id.id)
echo https://node.nesa.ai/nodes/$PUB_KEY
```
![image](https://github.com/user-attachments/assets/1c33ea05-6d59-4c7e-a061-76324b2e0134)

## اختیاری: به‌روزرسانی یا راه‌اندازی مجدد نود
* برای نودهای موجود که فقط نیاز به به‌روزرسانی تنظیمات یا راه‌اندازی مجدد دارند:

```
bash <(curl -s https://raw.githubusercontent.com/nesaorg/bootstrap/master/bootstrap.sh)
```

**1. گزینه "Advanced Wizardry" را انتخاب کنید.**

**2. گزینه Yes را برای بوت‌استرپ کردن آن انتخاب کنید، حتی اگر در حال اجرا باشد.**

* این کار تنظیمات شما را به‌روزرسانی کرده و در صورتی که کانتینرها در حال اجرا باشند، آنها را بازنشانی می‌کند یا اگر اجرا نمی‌شوند، آنها را راه‌اندازی می‌کند.

* پس از این، از `cat ~/.nesa/identity/node_id.id` استفاده کنید تا **node_id** خود را بررسی کنید و مطمئن شوید همان چیزی است که انتظار دارید. آمار را در https://node.nesa.ai/ بررسی کنید.

## توکنومیک:
توکن $NES روی شبکه اصلی (mainnet) عرضه خواهد شد و 8.8% از آن به شرکت‌کنندگان تست‌نت انگیزشی ایردراپ خواهد شد.

![GMpSQF5XAAABwMI](https://github.com/user-attachments/assets/a3bb334d-a55a-41f8-9a04-a333444e04fe)
