# TUGAS AKHIR PRAKTIKUM PREMOGRAMAN OSEANOGRAFI KEL 14
Tugas ini dibuat untuk memenuhi Tugas Akhir Praktikum Pemograman Oseanografi 2022.Tugas ini memuat Pengerjaan semua modul dalam praktikum pemograman oseanografi menggunakan bahasa pemrograman python yang dapat dilakukan pada beberapa platform seperti Google Colaboratory dan Jupyter Notebook. Sedangkan untuk library yang digunakan kali ini adalah Numpy,dan Matplotlib. Seluruh script yang dibuat adalah hasil kelompok 14 Oseanografi 2020. Semoga dapat bermanfaat!
## AUTHORS :
1. Dinika Izzati
2. Deva Yusuf Dhanadyaksa
3. Meiriza Adwina Setiorini
4. Nandika Abubakar Putri
5. Nanda Shifa Sherina
6. Oliver Kahn Parlindungan
7. Renata Gending Fadhillah 
## DAFTAR ISI :
1. MODUL 1 : ADVEKSI-DIVUSI 1D 
2. MODUL 2 : ADVEKSI-DIVUSI 2D 
3. MODUL 3 : HIDRODINAMIKA 1D
4. MODUL 4 : HIDRODINAMIKA 2D
## MODUL 1 : ADVEKSI-DIVUSI 1D
### Adveksi 
Adveksi merupakan mekanisme perpindahan massa suatu materi dari titik ke titik lainnya. Persamaan adveksi sendiri merupakan suatu persamaan gelombang linier orde satu dan termasuk dalam persamaan hiperbolik yang menggambarkan mekanisme trasnportasi suatu gas atau zat cair dengan arah tertentu. Persamaan adveksi memiliki 2 tipe hitungan yaitu eksplisit dan implisit. Persamaan umum adveksi ∂F/∂t=-u(∂F/∂x)
### Metode Penurunan 
1. FTCS 
merupakan gabungan dari selisih maju terhadap waktu dan selisih pusat terhadap ruang.
3. Leapfrog
4. Upstream





### Hidrodinamika 2D
Secara umum, model hidrodinamika 2D yang dijalankan cukup mampu merepresentasikan kondisi hidrodinamika di lokasi penelitian. Hasil simulasi model menunjukkan bahwa arus cenderung memiliki arah bolak-balik sesuai dengan pasut yang terjadi. Pola sebaran konsentrasi Ammonia, BOD, Nitrat, dan TSS secara signifikan dipengaruhi oleh pola arus yang terjadi. Dalam menyelesaikan persamaan Hdirodinamika 2D, dapat menggunakan script yang telah diberikan. Pada saat menggunakan script, diperlukan coding dengan menggunakan Google Colab dan library seperti matplotlib dan numphy, selain itu juga di perlukan modul siphon. Dimana dalam mengerjakan script ini diperlukan beberapa data untuk memenuhi nilai parameter yang ada. Berdasarkan data yang telah didapatkan, maka dapat dibuat grafik perhitungan nilai proses awal dengan beberapa parameter. Setelah memasukkan parameter yang diperlukan, maka akan didapat juga  pressure, windspeed, dan water temperature. Untuk mendapatkan hasil dari perhitungan yang telah dilakukan. Selanjutnya klik run untuk mendapatkan hasil grafik yang diperlukan. Dimana nantinya akan didapat 3 grafik dalam menyelesai persamaan ini, yaitu terdapat grafik  pressure, windspeed, dan water temperature.
### Script
# Copyright (c) 2018 Siphon Contributors.
# Distributed under the terms of the BSD 3-Clause License.
# SPDX-License-Identifier: BSD-3-Clause
"""
NDBC Bouy Meteorological Data Request
=====================================
The NDBC keeps a 45-day recent rolling file for each bouy. This examples shows how to access
the basic meteorogical data from a buoy and make a simple plot.
"""

import matplotlib.pyplot as plt

from siphon.simplewebservice.ndbc import NDBC

####################################################
# Get a pandas data frame of all of the observations, meteorogical data is the default
# observation set to query
df = NDBC.realtime_observations('46036') #Station ID
df.head()

##############################################
# Let's make a simple times series plot to checkout what the data look like.
fig, (ax1, ax2, ax3) = plt.subplots(3, 1, figsize=(12, 10))
ax2b = ax2.twinx()

# Pressure
ax1.plot(df['time'], df['pressure'], color='black')
ax1.set_ylabel('Pressure [hPa]')
fig.suptitle('Kelompok 14_Pemodelan', fontsize=18)


# Wind Speed, gust, direction
ax2.plot(df['time'], df['wind_speed'], color='tab:orange')
ax2.plot(df['time'], df['wind_gust'], color='tab:olive', linestyle='--')
ax2b.plot(df['time'], df['wind_direction'], color='tab:blue', linestyle='-')
ax2.set_ylabel('Wind Speed [m/s]')
ax2b.set_ylabel('Wind Direction')


# Water temperature
ax3.plot(df['time'], df['water_temperature'], color='tab:brown')
ax3.set_ylabel('Water Temperature [degC]')

plt.show()
