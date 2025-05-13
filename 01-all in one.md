برای آمادگی جهت مصاحبه درباره ELK Stack (Elasticsearch, Logstash, Kibana – و گاهی به آن **Elastic Stack** گفته می‌شود چون Beats هم به آن اضافه شده)، باید هم **مفاهیم اصلی** را بدانید و هم **تجربه عملی یا کاربردی** داشته باشید.

در ادامه، هم **شرح اجزا**ی ELK را می‌دهم و هم لیستی از چیزهایی که برای مصاحبه باید بلد باشید:

---

### 🔍 ELK Stack چیست؟

ELK مخفف سه ابزار اصلی است:

| ابزار             | وظیفه                                                                                                        |
| ----------------- | ------------------------------------------------------------------------------------------------------------ |
| **Elasticsearch** | موتور جست‌وجو و آنالیز داده‌های ذخیره‌شده. داده‌ها در قالب document-based در قالب JSON ذخیره می‌شوند.        |
| **Logstash**      | برای جمع‌آوری (ingest)، پردازش، فیلتر کردن و ارسال لاگ‌ها یا داده‌های ورودی از منابع مختلف به Elasticsearch. |
| **Kibana**        | رابط گرافیکی برای تحلیل و نمایش داده‌های Elasticsearch در قالب داشبورد، نمودار، جست‌وجو، و مانیتورینگ.       |

به طور خلاصه:

**Logstash --> Elasticsearch --> Kibana**

---

### ✅ چه چیزهایی برای مصاحبه باید بدانید؟

#### 1. **مفاهیم پایه و معماری**

* ELK Stack چیست و چرا استفاده می‌شود؟
* هر جزء (Elasticsearch، Logstash، Kibana) چه وظیفه‌ای دارد؟
* تفاوت بین Logstash و Beats چیست؟
* داده چطور از فایل لاگ (مثلاً syslog یا Apache) به Elasticsearch منتقل می‌شود؟
* معماری data pipeline در ELK

#### 2. **Elasticsearch**

* مفهوم Index، Document، Shard و Replica
* Query DSL: چطور جست‌وجو می‌کنید؟ Match, Term, Range, Bool
* فیلترها vs کوئری‌ها
* تفاوت بین Aggregations و Search
* مفاهیم Reindexing، Aliases، Mapping، Dynamic Mapping
* نحوه scale کردن (Horizontal scaling, Sharding, Replication)
* عملکرد Cluster، Node، و Index
* Monitoring & health status (green, yellow, red)
* تفاوت بین structured و unstructured data

#### 3. **Logstash**

* ساختار pipeline (Input, Filter, Output)
* استفاده از فیلترهایی مثل `grok`, `mutate`, `date`, `geoip`
* مثال پیکربندی Logstash: خواندن لاگ از فایل و ارسال به Elasticsearch
* Performance tuning (pipeline workers, batching)
* تفاوت بین Logstash و Beats
* استفاده از environment variable در پیکربندی

#### 4. **Kibana**

* ساخت داشبورد و visualization
* استفاده از Discover، Visualize، Dashboards، Lens
* ساختن alert در Kibana (Watcher / Alerts)
* چطور از Dev Tools برای query زدن به ES استفاده می‌کنید؟
* Security در Kibana (roles, spaces)

#### 5. **موارد پیشرفته و DevOpsی**

* چطور لاگ‌های سیستم توزیع‌شده را جمع‌آوری و مانیتور می‌کنید؟
* Index lifecycle management (ILM)
* استفاده از Filebeat برای ارسال لاگ
* Log Rotation و مشکل حجم زیاد داده‌ها
* معماری multi-node برای Elasticsearch
* Troubleshooting مثال: Kibana بالا نمیاد؟ No data in index؟

---

### 🎯 مثال‌هایی برای آمادگی بیشتر

**سوال مصاحبه‌ای ممکن:**

1. «وقتی داده‌ها در Kibana نمایش داده نمی‌شوند، چطور مشکل را دیباگ می‌کنید؟»
2. «اگر Elasticsearch در حالت Yellow باشد، چه می‌کنید؟»
3. «از چه فیلترهایی در Logstash برای لاگ‌های nginx استفاده می‌کنید؟»
4. «چطور از ILM استفاده می‌کنید تا فقط لاگ ۳۰ روز گذشته نگهداری شود؟»

---

### منابع پیشنهادی

* مستندات رسمی Elastic: [https://www.elastic.co/guide/en/](https://www.elastic.co/guide/en/)
* پروژه‌های واقعی در GitHub
* تمرین با Docker (ELK Stack در یک Docker Compose)

