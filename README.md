# MikroScript
Mikrotik Mikhmon Expire Monitor

<p align="center">
  <img src="https://github.com/user-attachments/assets/f2958ade-af27-43b8-be91-c6af2201f5db" />
</p>

Mikrotik RouterOS pada versi 7.10 melakukan perubahan pada format tanggal (date format). Hal ini akan membuat script yang berbasis [/system clock get date] tidak berfungsi sebagaimana mestinya.

Salah satu script yang berbasis [/system clock get date] ini adalah Mikhmon Expire Monitor. Mikhmon Expire Monitor biasanya diletakkan di scheduler untuk otomatis berjalan secara berkala untuk melakukan penghapusan otomatis user-user hotspot yang sudah Expired (habis) masa aktif nya.

Dampak besar dari perubahan pada format tanggal pada Mikrotik RouterOS versi 7.10 ini adalah Mikrotik Mikhmon Expire Monitor tidak dapat melakukan penghapusan otomatis user-user yang sudah expired tersebut. Akibatnya user-user tersebut akan selalu aktif atau tidak pernah Expired.

<p align="center">
  <img src="https://github.com/user-attachments/assets/dab0ff13-ab20-4d4a-9c2b-3bb53f3b253e" />
</p>

Ini adalah format tanggal dari Mikrotik RouterOS versi 7.9.1. Terlihat perintah [/system clock get date] menghasilkan output oct/14/2023.

<p align="center">
  <img src="https://github.com/user-attachments/assets/4ef5aaa6-f569-4e21-aebc-e37f4d51245c" />
</p>

Dan ini adalah format tanggal dari Mikrotik RouterOS versi 7.11.2. Terlihat perintah [/system clock get date] menghasilkan output 2023-10-14.

Untuk mengatasi masalah tersebut, silahkan update Mikrotik Expire Monitor script di scheduler anda. Khusus untuk Mikrotik RouterOS versi 7.10 ke atas.

Perlu dicatat: Agar anda membackup terlebih dulu konfigurasi Mikrotik anda untuk mencegah hal-hal yang tidak diinginkan.

[ [mikhmon-expire-monitor-v7.11.2](https://github.com/JZ02BNK/MikroScript/blob/main/mikhmon-expire-monitor-v7.11.2) ] 

[ [Auto Remove Mikrotik Userman Expired User](https://github.com/JZ02BNK/MikroScript/blob/main/Auto%20Remove%20Mikrotik%20Userman%20Expired%20User) ] 
