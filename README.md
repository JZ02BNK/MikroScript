# MikroScript
Mikrotik Mikhmon Expire Monitor

![image](https://github.com/user-attachments/assets/f2958ade-af27-43b8-be91-c6af2201f5db)

Mikrotik RouterOS pada versi 7.10 melakukan perubahan pada format tanggal (date format). Hal ini akan membuat script yang berbasis [/system clock get date] tidak berfungsi sebagaimana mestinya.

Salah satu script yang berbasis [/system clock get date] ini adalah Mikhmon Expire Monitor. Mikhmon Expire Monitor biasanya diletakkan di scheduler untuk otomatis berjalan secara berkala untuk melakukan penghapusan otomatis user-user hotspot yang sudah Expired (habis) masa aktif nya.

Dampak besar dari perubahan pada format tanggal pada Mikrotik RouterOS versi 7.10 ini adalah Mikrotik Mikhmon Expire Monitor tidak dapat melakukan penghapusan otomatis user-user yang sudah expired tersebut. Akibatnya user-user tersebut akan selalu aktif atau tidak pernah Expired.

![image](https://github.com/user-attachments/assets/dab0ff13-ab20-4d4a-9c2b-3bb53f3b253e)

Ini adalah format tanggal dari Mikrotik RouterOS versi 7.9.1. Terlihat perintah [/system clock get date] menghasilkan output oct/14/2023.

![image](https://github.com/user-attachments/assets/4ef5aaa6-f569-4e21-aebc-e37f4d51245c)

Dan ini adalah format tanggal dari Mikrotik RouterOS versi 7.11.2. Terlihat perintah [/system clock get date] menghasilkan output 2023-10-14.

Untuk mengatasi masalah tersebut, silahkan update Mikrotik Expire Monitor script di scheduler anda. Khusus untuk Mikrotik RouterOS versi 7.10 ke atas.

Perlu dicatat: Agar anda membackup terlebih dulu konfigurasi Mikrotik anda untuk mencegah hal-hal yang tidak diinginkan.

================================================================================

:local dateint do={
   :local days [ :pick $d 8 12 ];
   :local month [ :pick $d 5 7 ];
   :local year [ :pick $d 0 4 ];
   :return [:tonum ("$year$month$days")];
            }

:local timeint do={
   :local hours [ :pick $t 0 2 ];
   :local minutes [ :pick $t 3 5 ];
   :return ($hours * 60 + $minutes);
}

:local date [ /system clock get date ];
:local time [ /system clock get time ];
:local today [$dateint d=$date] ;
:local curtime [$timeint t=$time];
:local tyear [ :pick $date 0 4 ];
:local lyear ($tyear-1);

:foreach i in [ /ip hotspot user find where comment~"$tyear-" || comment~"$lyear-" ] do={
                    :local comment [ /ip hotspot user get $i comment];
                    :local limit [ /ip hotspot user get $i limit-uptime];
                    :local name [ /ip hotspot user get $i name];
                    :local gettime [:pic $comment 11 19];
                    :if ([:pic $comment 4] = "-" and [:pic $comment 7] = "-") do={
                            :local tempexpd [:pick $comment 0 10];
                            :local expd [$dateint d=$tempexpd];
                            :local expt [$timeint t=$gettime];
                            :if (($expd < $today and $expt < $curtime) or ($expd < $today and $expt > $curtime) or ($expd = $today and $expt < $curtime) and $limit != "00:00:01") do={
                                         :if ([:pic $comment 20] = "N") do={
                                              [ /ip hotspot user set limit-uptime=1s $i ];
                                              [ /ip hotspot active remove [find where user=$name] ];
                                          } else={
                                                   [ /ip hotspot user remove $i ];
                                                   [ /ip hotspot active remove [find where user=$name] ];
                                                  }
                            }
                     }
    }
