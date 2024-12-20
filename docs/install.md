# Panduan Instalasi untuk Menjalankan Aplikasi dengan Docker

Panduan ini menjelaskan langkah-langkah untuk membangun dan menjalankan aplikasi menggunakan Docker dan Docker Compose. Anda perlu membangun image Docker terlebih dahulu dan kemudian menjalankannya menggunakan Docker Compose.

## Langkah 1: Membangun Docker Image

Pastikan Anda memiliki Docker yang terinstal di sistem Anda. Jika belum, Anda dapat mengunduh dan menginstal Docker dari [situs resmi Docker.](https://docs.docker.com/engine/install/)

### 1.1 Clone Dashboard Repo
```bash
git clone https://github.com/fossyy/general_spider_dashboard.git
```

### 1.2 Clone General Spider Repo
```bash
git clone https://github.com/fossyy/general_spider.git
```

### 1.3 Buat Dockerfile Untuk Dashboard 

Pastikan Anda memiliki file `Dockerfile` di direktori aplikasi Anda. Berikut adalah contoh Dockerfile:
```dockerfile
FROM python:3.10-slim as python_build

RUN apt-get update && apt-get install -y \
    build-essential && \
    pip install --no-cache-dir scrapyd-client

COPY /general_spider /src
WORKDIR /src
RUN scrapyd-deploy --build-egg general.egg

FROM node:current-alpine3.20 AS node_builder

COPY /general_spider_dashboard /src

WORKDIR /src

RUN npm install -g tailwindcss
RUN npm install -g clean-css-cli
RUN npx tailwindcss -i ./public/input.css -o ./tmp/output.css
RUN cleancss -o ./public/output.css ./tmp/output.css

FROM golang:1.23.4-alpine3.20 AS go_builder

RUN apk update && apk upgrade && apk add --no-cache ca-certificates tzdata

COPY /general_spider_dashboard /src
COPY --from=node_builder /src/public /src/public
COPY --from=python_build /src/general.egg /src/app/general.egg

WORKDIR /src

RUN update-ca-certificates
RUN go install github.com/a-h/templ/cmd/templ@latest
RUN templ generate
RUN go build -o ./tmp/main

FROM scratch

WORKDIR /general

COPY --from=go_builder /usr/share/zoneinfo /usr/share/zoneinfo
COPY --from=go_builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=go_builder /src/public /general/public
COPY --from=go_builder /src/tmp/main /general

ENV TZ Asia/Jakarta

ENTRYPOINT ["./main"]
```


### 1.4 Buat Dockerfile Untuk Scrapyd (custom)

Pastikan Anda memiliki file `Dockerfile.scrapyd` di direktori aplikasi Anda. Berikut adalah contoh Dockerfile:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

RUN apt-get update && apt-get install -y git build-essential && apt-get clean

RUN git clone https://github.com/fossyy/scrapyd.git .
RUN cd scrapyd
RUN pip install --upgrade pip
RUN pip install setuptools wheel
RUN pip install -r requirements.txt
RUN pip install .

EXPOSE 6800

ENTRYPOINT ["scrapyd", "--pidfile="]
```

### 1.5 Bangun Docker Image Untuk Dashboard

Sekarang, untuk membangun image Docker, jalankan perintah berikut di terminal Anda:
```bash
docker build --network host --no-cache -t dashboard .
```

### 1.6 Bangun Docker Image Untuk Scrapy (custom)

Sekarang, untuk membangun image Docker, jalankan perintah berikut di terminal Anda:
```bash
docker build --network host -f Dockerfile.scrapyd --no-cache -t scrapyd .
```

## Langkah 2: Menjalankan Aplikasi dengan Docker Compose

Setelah image berhasil dibangun, Anda dapat menjalankan aplikasi menggunakan Docker Compose. Pastikan Anda memiliki file docker-compose.yml di direktori proyek Anda.

### 2.1 Membuat File docker-compose.yaml

Berikut adalah contoh file `docker-compose.yaml` untuk menjalankan aplikasi menggunakan Docker Compose:

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:16.0
    restart: on-failure
    environment:
      - POSTGRES_PASSWORD=VerySecretPassword
      - POSTGRES_DB=general_spider
    volumes:
      - postgres:/var/lib/postgresql/data
    networks:
      - scrapyd
    healthcheck:
      test: [ "CMD-SHELL", "pg_isready -U user -d mydatabase" ]
      interval: 30s
      retries: 5
      timeout: 20s
      start_period: 10s

  scrapyd:
    image: scrapyd
    restart: on-failure
    depends_on:
      - postgres
    volumes:
      - /opt/general_engine:/scrapyd:Z
      - /opt/general_engine/logs:/scrapyd/logs
    environment:
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_USERNAME=postgres
      - DB_PASSWORD=VerySecretPassword
      - DB_NAME=general_spider
      - DASHBOARD_ADDRESS=http://general_engine_dashboard:8080
    networks:
      - scrapyd
    healthcheck:
      test: [ "CMD-SHELL", "curl --silent --fail http://localhost:6800/ --max-time 5 || exit 1" ]
      interval: 30s
      retries: 5
      timeout: 10s
      start_period: 10s

  general_engine_dashboard:
    image: dashboard
    restart: on-failure
    depends_on:
      - scrapyd
      - postgres
    links:
      - scrapyd
    ports:
      - "8080:8080"
    volumes:
      - /opt/general_engine/logs:/general/logs
    environment:
      - SERVER_HOST=0.0.0.0
      - SERVER_PORT=8080
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_USERNAME=postgres
      - DB_PASSWORD=VerySecretPassword
      - DB_NAME=general_spider
      - SCRAPYD_URL=http://scrapyd:6800
    networks:
      - scrapyd

  torproxy:
    image: dperson/torproxy
    environment:
      - BW=100
    ports:
      - "8118:8118"
    restart: always
    networks:
      - scrapyd

volumes:
  scrapyd-volume:
  postgres:

networks:
  scrapyd:
```

### 2.2 Menjalankan Docker Compose

Setelah file docker-compose.yaml siap, jalankan perintah berikut untuk membangun dan menjalankan container:

```bash
docker compose up -d
```