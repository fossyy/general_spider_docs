# Monitoring And Management Spider General Engine 🖥️

## User Interface Spider

Bagian ini menunjukkan antarmuka pengguna (User Interface) untuk memantau dan mengelola spider yang sedang berjalan atau terjadwal.

![DSCON View Spider](_images/spider/dsman-view-spider.png)

Tampilan antarmuka utama yang mencakup daftar spider, statusnya, dan opsi pengelolaan seperti menjalankan, menghentikan, atau menjadwalkan spider.

## Running Now Spider Details

![DSCON Spider Spider Details](_media/spider/dsman-view-spider-details-video.mp4 ':include :type=video controls autoplay width=100% height=800px')

Bagian ini menampilkan detail spider yang sedang berjalan secara real-time. Video yang disematkan memberikan panduan visual untuk memahami informasi berikut:

- Status spider yang sedang berjalan.
- Statistik seperti jumlah halaman yang sudah di-scrape, error yang ditemukan, dan waktu eksekusi.
- Fitur penghentian atau penyesuaian langsung pada spider yang aktif.

## Scheduled Spider Details

![DSCON Spider Scheduled Details](_media/spider/dsman-view-spider-scheduled-details-video.mp4 ':include :type=video controls autoplay width=100% height=800px')

Bagian ini menampilkan detail spider yang telah dijadwalkan untuk berjalan pada waktu tertentu. Video yang disematkan memberikan panduan visual untuk memahami fitur berikut:

- Daftar spider yang dijadwalkan beserta waktu eksekusinya.
- Kemampuan untuk mengedit jadwal, menunda, atau menghapus spider dari daftar jadwal.
- Informasi status seperti "pending" atau "queued."

!> **Penting:** Saat ini `1 Browser TOR` dan `1 Reverse Tor Privoxy`, karena Spider tidak mendukung SOCKS5 secara langsung dan membutuhkan reverse proxy untuk digunakan.

