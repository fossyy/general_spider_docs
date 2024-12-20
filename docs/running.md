# How To Run Spider General Engine 🚀

--- 

## Dashboard Deploy Spider

![Deploy Spider View](_images/run/dsploy-view.png)

--- 

# Deploy New Spider

Antarmuka ini digunakan untuk melakukan deploy spider dalam sebuah aplikasi scraping. Berikut adalah penjelasan untuk setiap elemen pada halaman ini:

--- 

## Select Website Domain
Dropdown ini digunakan untuk memilih domain website yang akan menjadi target scraping. Pilih salah satu domain yang telah terdaftar sebelumnya.

**Ilustrasi**

![DSPLOY Domain](_images/run/dsploy-domain.png)

---

## Select Configuration
Dropdown ini digunakan untuk memilih konfigurasi yang akan digunakan untuk spider. Konfigurasi mencakup pengaturan seperti XPath, header, cookie, dan pengaturan lainnya.

**Ilustrasi**

![DSPLOY Domain](_images/run/dsploy-select-config.png)

---

## Select Proxies
Bagian ini digunakan untuk menentukan proxy yang akan digunakan selama proses scraping. Proxy membantu menyembunyikan identitas IP asli dan mencegah pemblokiran.

### Proxies Available

**Ilustrasi**

![DSPLOY Available Proxies](_images/run/dsploy-avail-proxies.png)


- Jika tidak ada proxy yang tersedia, akan muncul pesan:  
  _"No proxies available at the moment. Please check the proxy status page for available proxy information."_


--- 

### Proxies No Available

**Ilustrasi**

![DSPLOY No Proxies](_images/run/dsploy-no-proxies.png)

#### Add Rotating Proxies

!> **Penting:** Jika `Rotating Tor Proxy` tidak tersedia/kosong kunjung tab `Proxies` untuk menambahkan `Rotating Tor Proxy`.

**Ilustrasi**

![DSPLOY Tab Proxies](_images/run/dsploy-add-proxies.png)

---

## Cookie JSON (Optional)
Di sini, pengguna dapat menambahkan cookie dalam format JSON untuk dikirim bersama permintaan HTTP.

Contoh format JSON:
```json
{
  "cookie_1": "value_1",
  "cookie_2": "value_2",
  "cookie_3": "value_3",
  "cookie_4": "value_4"
}
```
--- 

## Output Destination
Dropdown ini digunakan untuk memilih tujuan penyimpanan data hasil scraping. Beberapa opsi umum mungkin termasuk:

### Local Storage Server

**Ilustrasi**

![DSPLOY Output Destination](_images/run/dsploy-out-dst-local.png)

?> **Tips:** Jika memilih Output Destination `local` ini akan menyimpan result didalam `server`.

--- 

### Kafka Brokers

#### Kafka Brokers Available

**Ilustrasi**

![DSPLOY Output Destination](_images/run/dsploy-out-dst-kafka.png)

?> **Tips:** Jika memilih Output Destination `kafka` ini akan menyimpan result didalam `kafka cluster`.

#### Kafka Brokers No Available

**Ilustrasi**

![DSPLOY Kafka No Brokers](_images/run/dsploy-kafka-no-brokers.png)

!> **Penting:** Jika `Kafka Brokers & Kafka Topics` tidak tersedia/kosong kunjung tab `Kafka Brokers` untuk menambahkan `Kafka Brokers`.

##### Add Kafka Brokers

**Ilustrasi**

![DSPLOY View Kafka Brokers](_images/run/dsploy-kafka-view-brokers.png)

?> **Note:** Gambar dibawah ini untuk menambahkan `Kafka Brokers & Kafka Topics`.

**Ilustrasi**

![DSPLOY Add Kafka Brokers](_images/run/dsploy-kafka-add-brokers.png)

--- 

## Deployment Time
Pengguna dapat menentukan waktu deploy spider:

### Run Now

Spider langsung dijalankan setelah deploy.

**Ilustrasi**

![DSPLOY Deployment Time Now](_images/run/dsploy-deploy-time-now.png)


### Schedule Run

Spider dijadwalkan untuk dijalankan pada waktu tertentu.

**Ilustrasi**

![DSPLOY Deployment Time Scheduled](_images/run/dsploy-deploy-time-scheduled.png ':size=1000')


--- 

## Additional Custom Settings
Bagian ini memungkinkan pengguna untuk menambahkan pengaturan tambahan dalam format JSON. Pengaturan ini dapat mencakup:

DOWNLOAD_DELAY: Waktu jeda antar permintaan (dalam detik).
CONCURRENT_REQUESTS: Jumlah permintaan yang dijalankan secara bersamaan.
Contoh format JSON:

```json
{
  "DOWNLOAD_DELAY": 1,
  "CONCURRENT_REQUESTS": 16
}
```

?> **Tips:** Jika ingin melihat `additional custom settings` lebih lanjut kunjungi [Scrapy Settings](https://docs.scrapy.org/en/latest/topics/settings.html).

--- 


