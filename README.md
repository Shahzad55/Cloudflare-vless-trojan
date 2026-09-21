# روش ساخت 1: اسکریپت پروکسی Cloudflare-workers/pages V2026.9

### 1. این پروژه فقط از استقرار محلی پشتیبانی می‌کند
### 2. تمام تنظیمات این پروژه به‌صورت محلی ویرایش می‌شوند و از لینک‌های خارجی شخص ثالث مانند اشتراک‌ساز و تبدیل‌کننده اشتراک استفاده نمی‌شود
### 3. نیازی نیست نگران باشید که اطلاعات اشتراک Nodeها توسط سازنده اشتراک‌ساز یا سازنده تبدیل‌کننده اشتراک در Backend مشاهده شود
--------------------------------
## ویژگی‌های اسکریپت:
#### 1. مخصوص کاربران مبتدی و راه‌اندازی آسان! Nodeهای پیش‌فرض همگی IPهای رسمی CF هستند و نیازی به به‌روزرسانی مداوم اشتراک برای دریافت IPهای بهینه Client نیست
#### 2. برای کاهش هزینه‌های اضافی کاربران مبتدی، استفاده از Custom Domain در این پروژه توصیه نمی‌شود؛ اگر حتماً می‌خواهید از Custom Domain استفاده کنید، امکان آن وجود دارد
#### 3. پس از کلیک روی دکمه Deploy در CF، می‌توانید Nodeها را مستقیماً به‌صورت دستی بسازید یا از Share Link استفاده کنید؛ حداکثر فقط یک uuid/Password لازم است و سایر موارد نیازی به تغییر ندارند
#### 4. روش Workers (فقط Custom Domain را پشتیبانی می‌کند): از Nodeهای پروکسی vless+ws+tls، trojan+ws+tls، vless+ws و trojan+ws پشتیبانی می‌کند
#### 5. روش Pages: از Nodeهای پروکسی vless+ws+tls و trojan+ws+tls پشتیبانی می‌کند
#### 6. از Single Node Link، Aggregated General Node Link، Aggregated General Node Subscription، sing-box Node Subscription و clash Node Subscription پشتیبانی می‌کند
-------------------------------------------------------------

### پلتفرم‌های ارتباطی: [甬哥 Blog](https://ygkkk.blogspot.com)، [甬哥 YouTube Channel](https://www.youtube.com/@ygkkk)، [甬哥 TG Group](https://t.me/+jZHc6-A-1QQ5ZGVl)، [甬哥 TG Channel](https://t.me/+DkC9ZZUgEFQzMTZl)

--------------------------------

## 1: متغیرهای قابل تنظیم برای CF Vless Node

| کاربرد متغیر | نام متغیر | الزامات مقدار متغیر | مقدار پیش‌فرض متغیر | الزام |
| :--- | :--- | :--- | :--- | :--- |
| 1. uuid ضروری | uuid (حروف کوچک) | مطابق فرمت استاندارد uuid | uuid 万人骑: 86c50e3a-5b87-49dd-bd20-03c7f2735e40 | پیشنهادی |
| 2. دسترسی Nodeهای سراسری به سایت‌های CF | proxyip (حروف کوچک) | پورت 443: آدرس ipv4، [ipv6]، Domain. پورت غیر443: IPV4:Port، [IPV6]:Port، Domain:Port | proxyip: داخلی اسکریپت | اختیاری |
| 3. Node اشتراک: IP بهینه | ip1 تا ip13، در مجموع 13 مورد | CF Official IP، CF Reverse Proxy IP، CF Optimized Domain | CF Official Domain مربوط به ygkkk | اختیاری |
| 4. Node اشتراک: پورت متناظر IP بهینه | pt1 تا pt13، در مجموع 13 مورد | 13 پورت استاندارد CF، یا هر پورت مربوط به Reverse Proxy IP | 13 پورت استاندارد CF | اختیاری |


## 2: متغیرهای قابل تنظیم برای CF Trojan Node

| کاربرد متغیر | نام متغیر | الزامات مقدار متغیر | مقدار پیش‌فرض متغیر | الزام |
| :--- | :--- | :--- | :--- | :--- |
| 1. Password ضروری | pswd (حروف کوچک) | حروف و اعداد پیشنهاد می‌شود | Password 万人骑: trojan | پیشنهادی |
| 2. دسترسی Nodeهای سراسری به سایت‌های CF | proxyip (حروف کوچک) | پورت 443: آدرس ipv4، [ipv6]، Domain. پورت غیر443: IPV4:Port، [IPV6]:Port، Domain:Port | proxyip: داخلی اسکریپت | اختیاری |
| 3. Node اشتراک: IP بهینه | ip1 تا ip13، در مجموع 13 مورد | CF Official IP، CF Reverse Proxy IP، CF Optimized Domain | CF Official Domain مربوط به ygkkk | اختیاری |
| 4. Node اشتراک: پورت متناظر IP بهینه | pt1 تا pt13، در مجموع 13 مورد | 13 پورت استاندارد CF، یا هر پورت مربوط به Reverse Proxy IP | 13 پورت استاندارد CF | اختیاری |

#### نکته بسیار مهم درباره متغیرهای IP و Port در Nodeهای اشتراک (3 و 4) 【کاربران مبتدی می‌توانند متغیرهای (3 و 4) را نادیده بگیرند و از مقادیر پیش‌فرض استفاده کنند】

1. توجه کنید: فقط زمانی که حتماً از Client مبتنی بر Subscription استفاده می‌کنید و می‌خواهید IP بهینه را تغییر دهید، باید متغیرهای ip1 تا ip13 و pt1 تا pt13 را تنظیم کنید

2. ip1 تا ip7 و pt1 تا pt7 در Share Link اشتراک فقط از Nodeهای بدون TLS با پورت‌های سری 80 پشتیبانی می‌کنند

3. ip8 تا ip13 و pt8 تا pt13 در Share Link اشتراک فقط از Nodeهای دارای TLS با پورت‌های سری 443 پشتیبانی می‌کنند

4. برای تنظیم Official IP نیازی به تنظیم Port نیست (به‌صورت پیش‌فرض 13 پورت استاندارد CF تنظیم شده‌اند)؛ برای تنظیم Reverse Proxy IP باید Nodeهای دارای TLS و بدون TLS را جداگانه تنظیم کنید و متغیر Port نیز باید تنظیم شود

5. برای تنظیم متغیرهای Node اشتراک می‌توانید به [Video Tutorial](https://youtu.be/8s-ELRuFaeE?si=MjhcKbt20d2Q2eqp&t=447) مراجعه کنید

---------------------------------

## 3: Custom proxyip

اگرچه اسکریپت به‌صورت پیش‌فرض دارای proxyip متعلق به سایر توسعه‌دهندگان است، اما از Custom proxyip نیز پشتیبانی می‌کند

سه روش IP V4، IP V6 و Domain پشتیبانی می‌شوند (وقتی Port برابر 443 است، می‌توان :Port را ننویسید)

1. حالت متغیر Global Node (در بخش‌های 1 و 2 بالا توضیح داده شده است):

| Port مربوط به proxyip | فرمت IPv4 | فرمت IPv6 | فرمت Domain |
| :--- | :--- | :--- | :--- |
| Port 443 | IPV4 Address | [IPV6 Address] | Domain |
| Port غیر443 | IPV4 Address:Port | [IPV6 Address]:Port | Domain:Port |

2. حالت مسیر path برای Single Node:

| Port مربوط به proxyip | فرمت IPv4 | فرمت IPv6 | فرمت Domain |
| :--- | :--- | :--- | :--- |
| Port 443 | /pyip=IPV4 Address | /pyip=[IPV6 Address] | /pyip=Domain |
| Port غیر443 | /pyip=IPV4 Address:Port | /pyip=[IPV6 Address]:Port | /pyip=Domain:Port |

توجه:

1. تغییر proxyip از طریق path در Single Node فقط روی همان Single Node که Client فعلی در حال تنظیم آن است تأثیر می‌گذارد و روی سایر Single Nodeها یا Subscription Nodeها تأثیری ندارد

2. تغییر Global proxyip روی تمام Nodeهایی که proxyip را از طریق path تنظیم نکرده‌اند تأثیر می‌گذارد

3. اگر path مربوط به Node شامل کلیدواژه `/pyip=` باشد، proxyip این Node فقط از proxyip تنظیم‌شده در path استفاده می‌کند و Global proxyip بی‌اثر خواهد بود

---------------------------------

## 4: بدون socks5! ساخت یک‌کلیکی proxyip و Reverse Proxy IP با هر Port سری 80/443 با استفاده از پروتکل reality برای کاربران مبتدی

توصیه می‌شود برای ساخت، از VPSهای خالص IPV6 استفاده کنید که به چین نزدیک، ارزان و دارای ترافیک زیاد باشند. تا حد امکان از IPV4 استفاده نکنید، زیرا احتمال زیادی دارد IPهای Reverse Proxy توسط افراد دیگر به‌صورت خودکار اسکن شوند و وارد IP Libraryهای عمومی یا پولی Reverse Proxy آن‌ها شوند. اگر حتماً باید از IPV4 استفاده کنید، مرتباً ترافیک VPS خود را بررسی کنید؛ استفاده از proxyip و Client Optimized IP هر دو باعث مصرف ترافیک VPS می‌شوند

اسکریپت‌های پیشنهادی برای ساخت proxyip و Reverse Proxy IP: [x-ui-yg Script](https://github.com/yonggekkk/x-ui-yg)، [sing-box-yg Script](https://github.com/yonggekkk/sing-box-yg)

برای عملیات مربوطه به [Video Tutorial Advanced 1](https://youtu.be/QOnMVULADko) و [Video Tutorial Advanced 2](https://youtu.be/CVZStM0t8BA) مراجعه کنید

-------------------------------------------

## 5: مشاهده اطلاعات Configuration و Share Link

CF Vless: در نوار آدرس Browser وارد کنید: https:// Pages Domain یا Custom Domain /Custom UUID

CF Trojan: در نوار آدرس Browser وارد کنید: https:// Pages Domain یا Custom Domain /Custom Password

توجه:

1. اگر Pages Domain یا Custom Domain هر دو Block شده باشند، برای باز کردن آن باید Proxy فعال باشد

2. هنگام استفاده از Custom Domain، اطلاعات Configuration و Share Link در Pages Domain همچنان قابل استفاده هستند

---------------------------------

## 6: استفاده از Optimized IP

CF Official Optimized 80-series Ports: 80، 8080، 8880، 2052، 2082، 2086، 2095

CF Official Optimized 443-series Ports: 443، 2053، 2083، 2087، 2096، 8443

اگر نیازی به بالاترین سرعت روزانه یا انتخاب Country ندارید، از IP یا Domain پیش‌فرض CF Official استفاده کنید و نیازی به تغییر آن نیست

IPهای CF Official پیشنهادی و ساده برای کاربران مبتدی در ادامه آمده‌اند؛ از جابه‌جایی بین 13 پورت استاندارد پشتیبانی می‌کنند و به آن‌ها «IPهای همیشه پیشرو و زنده» گفته می‌شود

104.16.0.0

104.17.0.0

104.18.0.0

104.19.0.0

104.20.0.0

104.21.0.0

104.22.0.0

104.24.0.0

104.25.0.0

104.26.0.0

104.27.0.0

172.66.0.0

172.67.0.0

162.159.0.0

2606:4700::0 نیازمند محیط IPV6

CDN Optimized Domain: yg1.ygkkk.dpdns.org (عدد 1 در yg1 را می‌توان با هر عددی از 1 تا 11 جایگزین کرد)

---------------------------------

## 7: Clientهای پیشنهادی

#### مزیت فعال‌سازی قابلیت Fragment: نادیده گرفتن TLS Blocking روی Domain و در نتیجه امکان استفاده از TLS Node برای Domainهای مسدودشده مانند workers.

#### نکته: برای Custom Domain یا Pages Domain که TLS Blocking نشده‌اند، برای استفاده از TLS Node نیازی به فعال‌سازی Fragment نیست
 
Clientهای زیر در حال حاضر از این قابلیت پشتیبانی می‌کنند (با کلیک روی نام، به Download Address رسمی هدایت می‌شوید)

1. Android: [v2rayNG](https://github.com/2dust/v2rayNG/tags)، [Nekobox](https://github.com/starifly/NekoBoxForAndroid/releases)، [Karing](https://github.com/KaringX/karing/tags)، clash/mihomo و انواع Clientهای sing-box نیز قابل استفاده هستند

2. Windows: [v2rayN](https://github.com/2dust/v2rayN/tags)، [Hiddify](https://github.com/hiddify/hiddify-next/tags)، [Karing](https://github.com/KaringX/karing/tags)، clash/mihomo و انواع Clientهای sing-box نیز قابل استفاده هستند

3. iOS: Karing، Hiddify Proxy & VPN، Shadowrocket(小火箭)،Streisand

4. Soft Router: passwall، ssr-plus، homeproxy

توجه: Clientهای Shadowrocket(小火箭)، v2box، v2rayn و v2rayng دارای مشکل اجباری بودن TLS برای trojan+ws هستند که باعث می‌شود trojan+ws کار نکند. همچنین Subscription مربوط به clash شامل Nodeهای trojan+ws نیست. این مورد برای اطلاع‌رسانی ذکر شده است

برای مشکلات مربوط به استفاده از Client، به [CF vless/trojan Free Node Tutorial (6): Node کار نمی‌کند، مشکل کجاست؟ راهنمای تنظیم Client رایگان در چند پلتفرم و نکات جلوگیری از خطا](https://youtu.be/8E0l0nQWLxs) مراجعه کنید

---------------------------------

### مجموعه Video Tutorialهای CF:

2026.9.19 آخرین نسخه: [CF Free Node در عصر جدید: نادیده گرفتن خطای 1101، روش ساده Deployment با Workers+Pages و توضیح دوباره اسرار CF Node](https://youtu.be/KWtqRFbi568)

[🥇رتبه‌بندی 9 مشکل اصلی ساخت Proxy: رتبه 4 باعث گمراهی 99٪ کاربران سراسر اینترنت شده! رتبه 1 همه را به دردسر انداخته! کاملاً پرمحتوا!](https://youtu.be/pJwJBqBkcfw)

توصیه ویژه: [CF vless/trojan Free Node Tutorial (4): بررسی رابطه و ویژگی‌های Official IP بهینه، Reverse Proxy IP بهینه و Optimized Domain؛ مفهوم ProxyIP](https://youtu.be/NaLd-orwFUE)

توصیه ویژه: [CF vless/trojan Free Node Tutorial (6): Node کار نمی‌کند، مشکل کجاست؟ راهنمای تنظیم Client رایگان در چند پلتفرم و نکات جلوگیری از خطا](https://youtu.be/8E0l0nQWLxs)

توصیه پیشرفته: [CF vless/trojan Free Node Final Tutorial (7): نمایش اختصاصی «Fixed IP» واقعی، حل خطاهای Client در twitch و chatgpt؛ ساخت یک‌کلیکی Reverse Proxy IP و ProxyIP؛ بررسی خطر اسکن شدن IP شما توسط دیگران](https://youtu.be/QOnMVULADko)

توصیه پیشرفته: [CF vless/trojan Free Node Final Tutorial (8): ساخت ProxyIP عمومی برای تمام Portها، پشتیبانی هم‌زمان از Reverse Proxy IP بهینه‌شده در Client و آموزش نهایی ساخت Reverse Proxy IP](https://youtu.be/CVZStM0t8BA)

[مرور منتخب Live Stream: چهار ویژگی اصلی CF workers vless Free Node و مشکل قطع و مسدود شدن Node](https://youtu.be/9OHGpWlfdJ0)

-----------------------------------


# روش ساخت 2: اسکریپت Cloudflare-Socks5/Http Local Proxy
### پشتیبانی از Workers Domain، Pages Domain و Custom Domain
### دارای سه حالت اختیاری ECH-TLS، TLS معمولی و بدون TLS برای مقابله با انواع Blocking و Filtering

#### اسکریپت یا Docker Image زیر: `ygkkk/cfsh`؛ استفاده از آن روی Soft Router و سایر پلتفرم‌های Local توصیه می‌شود. Shortcut اسکریپت: bash cfsh.sh

```
curl -sSL https://raw.githubusercontent.com/yonggekkk/Cloudflare_vless_trojan/main/s5http_wkpgs/cfsh.sh -o cfsh.sh && chmod +x cfsh.sh && bash cfsh.sh
```

| کاربرد متغیر | نام متغیر | الزامات مقدار متغیر | مقدار پیش‌فرض متغیر | الزام |
| :--- | :--- | :--- | :--- | :--- |
| 1. CF Server Domain:Port | cf_domain | Domain:443-series Port یا 80-series Port | بدون مقدار؛ باید از CF یک Domain مربوط به workers/pages/Custom دریافت شود | الزامی |
| 2. CF Server Key | token | همان حروف و اعداد مورد استفاده در Server | بدون Key | اختیاری |
| 3. Client Local IP:Port | client_ip | بین 10000 تا 65000 | 30000 | اختیاری |
| 4. IP/Domain بهینه مشخص‌شده | cf_cdnip | CF Optimized IP یا Optimized Domain | yg(هر عددی از 1 تا 13).ygkkk.dpdns.org؛ برای China Mobile معمولاً به Hong Kong و برای Telecom/Unicom معمولاً به Japan/Singapore متصل می‌شود | اختیاری؛ همچنین استفاده از `cloudflare-ech.com` توصیه می‌شود که معمولاً به Europe/US متصل می‌شود |
| 5. ProxyIP مشخص‌شده | pyip | ipv4 یا [ipv6] یا Domain | استفاده از Server ProxyIP | اختیاری |
| 6. DNS مشخص‌شده DoH | dns | فرمت DoH مربوط به DNS | dns.alidns.com/dns-query | اختیاری |
| 7. ECH Switch | enable_ech | y=فعال، n=غیرفعال | ECH فعال | اختیاری |
| 8. Split Routing Switch | cnrule | y=Proxy داخلی/خارجی، n=Global Proxy | Proxy داخلی/خارجی | اختیاری |

| نکات تنظیم متغیر در سه حالت | ECH-TLS | TLS معمولی | بدون TLS |
| :--- | :--- | :--- | :--- |
| 1. cf_domain (CF Server Domain:Port) | workers/pages/Custom Domain:443-series Port | pages/Custom Domain:443-series Port | workers Domain:80-series Port |
| 2. enable_ech (ECH Switch) | y فعال | n غیرفعال | y فعال/n غیرفعال |

توجه:

CF 80-series Port: 80 (توصیه‌شده)، 8080، 8880، 2052، 2082، 2086، 2095

CF 443-series Port: 443 (توصیه‌شده)، 2053، 2083، 2087، 2096، 8443

IP Lookup برای سایت‌های غیرCF (نمایش IPهای CF با 104.28/2a09): https://www.whatismyip.com

IP Lookup برای سایت‌های CF (نمایش IP مربوط به proxyip): https://ip.sb

معتبر بودن ProxyIP روی امکان دسترسی به سایت‌های CF مانند سایت رسمی CF، X Twitter و ChatGPT تأثیر دارد

Video Tutorial: [CF Socks5/Http Free Proxy Tutorial: بررسی مزایا و معایب ECH Workers؛ پشتیبانی از سه حالت Proxy و استفاده مجدد از چند Port، و Custom proxyip در Client](https://youtu.be/Y_SHcD3prt8)


<img width="1182" height="517" alt="e5dfbfd7c9e6f15d4bd1c8409eecdffc" src="https://github.com/user-attachments/assets/ac0bcef0-54f9-4290-8c04-f84bbbe1cdf8" />

-------------------------------------------------------------

### سپاس از حمایت شما! WeChat Donation برای 甬哥侃侃侃ygkkk
![41440820a366deeb8109db5610313a1](https://github.com/user-attachments/assets/7dbaa3b1-cce4-415a-b46e-049531cf4d0d)

-------------------------------------------------------------

### منبع کد: [ca110us](https://github.com/ca110us/epeius)، [emn178](https://github.com/emn178/js-sha256/blob/master/src/sha256.js)، [3Kmfi6HP](https://github.com/3Kmfi6HP/EDtunnel)، [badafans](https://github.com/badafans/Cloudflare-IP-SpeedTest)، [XIU2](https://github.com/XIU2/CloudflareSpeedTest)
### بیانیه: تمام Codeها از جامعه Github دریافت شده و با استفاده از ChatGPT یکپارچه شده‌اند
