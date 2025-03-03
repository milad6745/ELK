```yml
version: "3.7"

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.6.0
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - ELASTICSEARCH_USERNAME=elastic
      - ELASTICSEARCH_PASSWORD=changeme
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - esdata:/usr/share/elasticsearch/data
    networks:
      - elastic_network

  kibana:
    image: docker.elastic.co/kibana/kibana:8.6.0
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=https://elasticsearch:9200
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
    networks:
      - elastic_network

volumes:
  esdata:
    driver: local

networks:
  elastic_network:
    driver: bridge
```
با این تغییرات، شما می‌توانید دستور زیر را برای راه‌اندازی سرویس‌ها اجرا کنید:

```
docker-compose up -d
```
این دستور هر دو سرویس Elasticsearch و Kibana را راه‌اندازی می‌کند و مشکلی در فایل شما وجود نخواهد داشت.


بعد از نصب سرور kibana به مشخضات زیر در دسترس است .
```
http://<ip>:5601/

```



برای اتصال Kibana به Elasticsearch با استفاده از **Enrollment Token**، باید در ابتدا در Elasticsearch یک **Enrollment Token** ایجاد کنید. این روش معمولاً برای تنظیم **Elastic Stack Security** و تأمین اتصال امن به Elasticsearch است.

### مراحل ایجاد و استفاده از **Enrollment Token**:

1. **ایجاد Enrollment Token در Elasticsearch**:
   برای این‌که Kibana بتواند به Elasticsearch وصل شود، شما باید یک **Enrollment Token** از طریق فرمان‌های امنیتی در Elasticsearch بسازید.

   ابتدا وارد کانتینر Elasticsearch شوید:
   ```bash
   docker exec -it elasticsearch /bin/bash
   ```

   سپس دستور زیر را برای ایجاد **Enrollment Token** اجرا کنید:
   ```bash
   bin/elasticsearch-setup-passwords auto
   ```





**دریافت کد تأیید از Kibana**:
   به منظور دریافت کد تأیید، باید به داخل کانتینر Kibana بروید و دستور زیر را برای دریافت کد تأیید اجرا کنید:
   
   ```bash
   docker exec -it kibana /bin/bash
   ```

سپس دستور زیر را برای دریافت کد تأیید اجرا کنید:

   ```bash
   bin/kibana-verification-code
   ```

این مراحل باید مشکل شما را حل کند و Kibana به درستی به Elasticsearch متصل شود.


# اگر مشکل داشتیم با Authentication این راخ را دنبال کنیم .

این خطا نشان می‌دهد که Kibana نمی‌تواند به Elasticsearch متصل شود، زیرا احراز هویت (authentication) انجام نشده است. در نسخه‌های جدید Elasticsearch (از جمله نسخه ۸.x)، احراز هویت به طور پیش‌فرض فعال است و Kibana باید به درستی پیکربندی شود تا بتواند با Elasticsearch ارتباط برقرار کند.

---

### **علت خطا:**
- Kibana نتوانسته است با استفاده از توکن یا نام کاربری/رمز عبور به Elasticsearch متصل شود.
- ممکن است تنظیمات احراز هویت در Kibana یا Elasticsearch به درستی پیکربندی نشده باشد.

---

### **راه‌حل‌ها:**

#### ۱. **استفاده از توکن احراز هویت (Service Account Token):**
   - در فایل `docker-compose.yml` شما، یک توکن (`ELASTICSEARCH_SERVICE_ACCOUNT_TOKEN`) تنظیم شده است. مطمئن شوید که این توکن معتبر است.
   - اگر توکن را خودتان ایجاد نکرده‌اید، می‌توانید یک توکن جدید ایجاد کنید:
     1. وارد کانتینر Elasticsearch شوید:
        ```bash
        docker exec -it elasticsearch /bin/bash
        ```
     2. دستور زیر را اجرا کنید تا یک توکن جدید ایجاد شود:
        ```bash
        bin/elasticsearch-service-tokens create elastic/kibana kibana-token
        ```
     3. خروجی توکن را کپی کنید و در فایل `docker-compose.yml` قرار دهید:
        ```yaml
        environment:
          - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
          - ELASTICSEARCH_SERVICE_ACCOUNT_TOKEN=<توکن-جدید>
        ```

#### ۲. **استفاده از نام کاربری و رمز عبور:**
   - اگر می‌خواهید از نام کاربری و رمز عبور استفاده کنید، خطوط مربوط به `ELASTICSEARCH_USERNAME` و `ELASTICSEARCH_PASSWORD` را در Kibana فعال کنید:
     ```yaml
     environment:
       - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
       - ELASTICSEARCH_USERNAME=elastic
       - ELASTICSEARCH_PASSWORD=milad123
     ```
   - مطمئن شوید که نام کاربری و رمز عبور درست هستند.

#### ۳. **بررسی اتصال شبکه:**
   - مطمئن شوید که Kibana می‌تواند به Elasticsearch دسترسی پیدا کند. برای این کار، می‌توانید از داخل کانتینر Kibana دستور `curl` را اجرا کنید:
     ```bash
     docker exec -it kibana /bin/bash
     curl -u elastic:milad123 http://elasticsearch:9200
     ```
   - اگر پاسخ دریافت نکردید، ممکن است مشکل شبکه یا تنظیمات Elasticsearch باشد.

#### ۴. **غیرفعال کردن احراز هویت (فقط برای محیط توسعه):**
   - اگر در محیط توسعه هستید و نیازی به احراز هویت ندارید، می‌توانید آن را در Elasticsearch غیرفعال کنید. برای این کار:
     1. وارد کانتینر Elasticsearch شوید:
```bash
docker exec -it elasticsearch /bin/bash
elasticsearch@27528661d290:~$ echo "xpack.security.enabled: false" >> elasticsearch.yml
```
     4. Elasticsearch را ری‌استارت کنید:
        ```bash
        docker-compose restart elasticsearch
        ```
   - توجه: این روش فقط برای محیط‌های توسعه مناسب است و در محیط تولید توصیه نمی‌شود.

---

### **بررسی‌های نهایی:**
- پس از اعمال تغییرات، Docker Compose را دوباره راه‌اندازی کنید:
  ```bash
  docker-compose down
  docker-compose up -d
  ```
- لاگ‌های Kibana و Elasticsearch را بررسی کنید تا مطمئن شوید مشکل برطرف شده است:
  ```bash
  docker logs -f kibana
  docker logs -f elasticsearch
  ```
