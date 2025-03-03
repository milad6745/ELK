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
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
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

اگر سوال یا مشکلی دارید، خوشحال می‌شوم کمک کنم! 😊
