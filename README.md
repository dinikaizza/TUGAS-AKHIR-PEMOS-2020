# TUGAS AKHIR PRAKTIKUM PREMOGRAMAN OSEANOGRAFI KEL 14
Tugas ini dibuat untuk memenuhi Tugas Akhir Praktikum Pemograman Oseanografi 2022.Tugas ini memuat Pengerjaan semua modul dalam praktikum pemograman oseanografi menggunakan bahasa pemrograman python yang dapat dilakukan pada beberapa platform seperti Google Colaboratory dan Jupyter Notebook. Sedangkan untuk library yang digunakan kali ini adalah Numpy,dan Matplotlib. Seluruh script yang dibuat adalah hasil kelompok 14 Oseanografi 2020. Semoga dapat bermanfaat!
## AUTHORS :
1. Dinika Izzati            26050120130087
2. Deva Yusuf Dhanadyaksa   26050120140148
3. Meiriza Adwina Setiorini 26050120140138
4. Nandika Abubakar Putri   26050120140054   
5. Nanda Shifa Sherina      26050120120035
6. Oliver Kahn Parlindungan 26050120140141
7. Renata Gending Fadhillah 26050120140137 
## DAFTAR ISI :
1. MODUL 1 : ADVEKSI-DIVUSI 1D 
2. MODUL 2 : ADVEKSI-DIFUSI 2D 
3. MODUL 3 : HIDRODINAMIKA 1D
4. MODUL 4 : HIDRODINAMIKA 2D
## MODUL 1 : ADVEKSI-DIFUSI 1D
### Adveksi 
Adveksi merupakan mekanisme perpindahan massa suatu materi dari titik ke titik lainnya. Persamaan adveksi sendiri merupakan suatu persamaan gelombang linier orde satu dan termasuk dalam persamaan hiperbolik yang menggambarkan mekanisme trasnportasi suatu gas atau zat cair dengan arah tertentu. Persamaan adveksi memiliki 2 tipe yaitu hitungan yaitu eksplisit dan implisit. Persamaan umum adveksi ∂F/∂t=-u(∂F/∂x)
#### Metode Penurunan 
1. FTCS 

merupakan gabungan dari selisih maju terhadap waktu dan selisih pusat terhadap ruang.
2. Leapfrog

Merupakan perluasan dari metode beda tengah (Central Difference) terhadap ruang dan waktu. Skema Leapfrog didapatkan dari turunan dari deret taylor, merupakan skema yang konsisten jika C≤1
![download](https://user-images.githubusercontent.com/105905933/170132423-b2970e2f-cf50-42ee-b7a8-b3a945dd4ce0.png)

3. Upstream

Merupakan skema yang digunakan untuk melengkapi ketidaksempurnaan metode leapfrog. Karena nilai konsentrasi dalam komputer dapat menjadi negativ. Oleh karena itu metode upstream dibuat untuk model positif dari konsentrasi di alam yang menuju ke lautan . metode ini menggunakan pendekatan beda maju untuk turunan waktu, sedangkan terhadap ruang melihat arah kecepatan (u). Jika u>0 maka pendekatan beda mundur. Sebaliknya jika u<0 maka pendekatan beda maju.
### DIFUSI

Merupakan sebuah proses dimana suatu zat bergerak dari konsentrasi tinggi ke konsentrasi rendah. Contoh pengaplikasian Oil Spill, persamaan awal :

![Screenshot (677)](https://user-images.githubusercontent.com/105905933/170133446-61b44ef8-7ce9-4325-b6c7-c65438551fa3.png)

Diskritisasi dilakukan secara eksplisit (FTCS), jika syarat batas terpenuhi = overflow
### Diskritisasi Penuruna model menggunakan metode leapfrog

![diskritisasi](https://user-images.githubusercontent.com/105905933/170133820-4710f73e-c5ad-4bfa-8097-f6c817725f94.png)


## MODUL 2 : ADVEKSI-DIFUSI 2D
Adveksi Difusi 2 D dan menguji kekonvergenan\ud solusi numerik. Metode beda hingga merupakan suatu metode numerik yang sering digunakan dalam\ud penyelesaian masalah Persamaan Differensial Parsial karena metode ini dapat memberikan solusi yang\ud cukup akurat. Kemampuan metode yang beda hingga dalam memberikan hasil pendekatan tersebut karena\ud didukung oleh kemajuan yang pesat dalam bidang komputer.
### Metode 
Ada dua skema dasar yang dapat digunakan untuk menyelesaikan persamaan differensial dengan metode beda hingga (FDM), yaitu skema ekspisit dan implisit.
### Pengaplikasian
Persamaan matematika lain yang memodelkan fenomena gejala alam adalah persamaan Adveksi-Difusi atau yang sering disebut dengan persamaan transpor. Persamaan Adveksi Diusi adalah persamaan matematis yang didesign untuk mempelajari fenomena transpor polutan. Hasil keluaran model ada dua bentuk data yaitu data spasial dan temporal, dimana data spasial berupa parameter konsentrasi sedimen deposisi/erosi, bottom    shear stress,   arus   pasang   surut   dan   salinitas.
### Penulisan Script

Import library pyton

    import matplotlib.pyplot as plt
    import numpy as np
    import sys

Pendefinisian persentase
   
   def percentage(part, whole):
        percentage = 100 * float(part)/float(whole)
        return str(round(percentage,2)) + "x"

Memasukkan Parameter Awal

    C = 0.61
    ad = 1.61

Arah Arus

    theta = 61
    #theta = 121
    #theta = 196
    #theta = 376

Parameter Lanjutan

    q = 0.95 
    x = 300 
    y = 300 
    dt = 0.5 
    dx = 3 
    dy = 3

Lama Simulasi
    
    Tend = 106
    #Tend = 1
    dt = 0.5

Nilai Polutan

    px = 150    #Nilai polutan pada grid x
    py = 136    #Nilai polutan pada grid y
    Ic = 516    #Jumlah nilai polutan

Perhitungan U(arah) dan V(kecepatan)

    u = C * np.sin(theta*np.pi/180)
    v = C * np.cos(theta*np.pi/180)
    dt_count = 1/((abs(u)/(q*dx))+(abs(v)/(q*dy))+(2*ad/(q*dx**2))+(2*ad/(q*dy*2)))

    Nx = int(x/dx)  #number of mesh in x direction
    Ny = int(y/dy)  #number of mesh in y direction
    Nt = int(Tend/dt)

Perhitungan titik polutan di buang
   
    px1 = int(px/dx)
    py1 = int(py/dy)

Fungsi disederhanakan
    
    lx = u*dt/dx
    ly = v*dt/dy
    ax = ad*dt/dx**2
    ay = ad*dt/dy**2
    cfl = (2*ax + 2*ay + abs(lx) + abs(ly))  #syarat kestabilan CFL

Perhitungan cfl

    if cfl >= q:
        print('CFL Violated, please use dt :'+str(round(dt_count,4)))
        sys.exit ()

Pembuatan grid 
    
    x_grid = np.linspace(0-dx, x+dx, Nx+2) #ghostnode boundary
    y_grid = np.linspace(0-dx, y+dy, Ny+2) #ghostnode boundary
    t = np.linspace(0, Tend, Nt+1)
    x_mesh, y_mesh = np.meshgrid(x_grid,y_grid)
    F = np.zeros((Nt+1, Ny+2, Nx+2))

Kondisi awal

    F[0,py1,px1]=Ic

Iterasi

    for n in range (0, Nt):
        for i in range (1,Ny+1):
            for j in range (1, Nx+1):
             F[n+1,i,j]=((F[n,i,j]*(1-abs(lx)-abs(ly))) + \
                    (0.5*(F[n,i-1,j]*(ly+abs(ly)))) + \
                    (0.5*(F[n,i+1,j]*(abs(ly)-ly))) + \
                    (0.5*(F[n,i,j-1]*(lx+abs(lx)))) + \
                    (0.5*(F[n,i,j+1]*(abs(lx)-lx))) + \
                    (ay*(F[n,i+1,j]-2*(F[n,i,j])+F[n,i-1,j])) +\
                    (ax*(F[n,i,j+1]-2*(F[n,i,j])+F[n,i,j-1])))
Syarat batas
   
    F[n+1,0,:] = 0 #bc bawah
    F[n+1,:,0] = 0 #bc kiri
    F[n+1,Ny+1,:] = 0 #bc atas
    F[n+1,:,Nx+1] = 0 #bc kanan

Output Gambar
   
    plt.clf()
    plt.pcolor(x_mesh, y_mesh, F[n+1, :, :], cmap = 'jet',shading='auto',edgecolor='k')
    cbar=plt.colorbar(orientation='vertical',shrink=0.95,extend='both')
    plt.clf()
    plt.pcolor(x_mesh,y_mesh,F[n+1,:,:],cmap='jet',shading='auto',edgecolor='k')
    cbar = plt.colorbar(orientation='vertical',shrink=0.95,extend='both')
    cbar.set_label(label='Concentration',size = 8)
    #plt.clim(0,100)
    plt.title('Kelompok 14_Pemodelan \n t='+str(round(dt*(n+1),3))+ ', Initial condition='+str(Ic),fontsize=10)
    plt.xlabel('x_grid',fontsize=9)
    plt.ylabel('y_grid',fontsize=9)
    plt.axis([0, x, 0, y])
    #plt.pause(0.01)
    plt.savefig(str(n+1)+'.jpg', dpi=300)
    plt.pause(0.01)
    plt.close()
    print('running timestep ke:' +str(n+1) + ' dari:' +str(Nt) + '('+ percentage(n+1,Nt)+')')
    print('Nilai CFL:' +str(cfl) + 'dengan arah: ' +str(theta))
    
    
### Hasil 
![image](https://user-images.githubusercontent.com/106167818/170089041-399d0637-f3e5-4135-93af-8cc49595dc06.png)
![image](https://user-images.githubusercontent.com/106167818/170089095-93c2add1-dac8-462c-bceb-8d738667c29d.png)
![image](https://user-images.githubusercontent.com/106167818/170089335-2dd88704-6d68-4388-970c-90fa7df8b2bb.png)
![image](https://user-images.githubusercontent.com/106167818/170090382-f07a3f94-afe0-4867-9f5f-849b0d02b3b1.png)
![image](https://user-images.githubusercontent.com/106167818/170090459-dbf304b1-515b-4105-8f79-17a8e2f9a494.png)
![image](https://user-images.githubusercontent.com/106167818/170091554-affe033a-c414-490a-a727-72295945841b.png)
![image](https://user-images.githubusercontent.com/106167818/170092778-d744fdc8-fe71-4354-a8c8-6b881360116b.png)
![image](https://user-images.githubusercontent.com/106167818/170093196-6e0ba3ea-d92f-4f73-a617-bbd633600313.png)
![image](https://user-images.githubusercontent.com/106167818/170093790-a4c976bb-f435-40b8-916a-3b12402b4f41.png)
![image](https://user-images.githubusercontent.com/106167818/170094443-ee9f9bd5-fd54-4521-bde2-6fbe8c501f7e.png)
![image](https://user-images.githubusercontent.com/106167818/170094503-003a0abf-13e7-4bf0-8576-db9c8a614d11.png)
![image](https://user-images.githubusercontent.com/106167818/170095125-b755f283-63b8-49ed-884a-e1a4553ca879.png)
### Pembahasan
Pada skenario 1 didapatkan bahwa konsentrasi yang ada pada grafik terus berkurang seiring bertambahnya periode. Selain itu, terlihat juga pergerakan polutan ke arah timur yang mana ini menandakan bahwa arus yang terjadi dilapangan juga bergerak kearah timur. Pada skenario 2, terlihat adanya penurunan konsentrasi yang terjadisecara drastic dalam kurun periode t 1-31. Arah arus pun terlihat bergerak ke arah tenggara yang mana hal ini diikuti oleh pergerakan konsentrasi polutan yang bergerak secara kontinu ke arah tenggara yang ditandai dengan adanya perpindahan grid pada grafik. Pada skenario 3 Arus bergerak ke arah selatan yang mana diikuti juga oleh pergerakan konsentrasi polutan ke arah yang sama. Konsentrasi polutan pada skenario 3 juga semakin menurun seiring bertambahnya periode t pada grafik yang mana pada titik akhir ini, konsentrasi polutan tertinggi memiliki nilai 4. Pada skenario 4, terlihat bahwa pada periode antara t 0,5-18 penurunan konsentrasi polutan terjadi secara drastis dimana hal ini diikuti oleh pergerakan konsentrasi polutan yang semakin bergerak ke arah timur laut seiring bertambahnya nilai t. Arah pergerakan dan penyebaran suatu zat yang ada pada suatu perairan biasanya dipengaruhi oleh konsentrasi zat itu sendiri (Firmansyah et al., 2021)
## MODUL 3 : HIDRODINAMIKA 1D
## MODUL 4 : HIDRODINAMIKA 2D
Secara umum, model hidrodinamika 2D yang dijalankan cukup mampu merepresentasikan kondisi hidrodinamika di lokasi penelitian. Hasil simulasi model menunjukkan bahwa arus cenderung memiliki arah bolak - balik sesuai dengan pasut yang terjadi. Pola sebaran konsentrasi Ammonia, BOD, Nitrat, dan TSS secara signifikan dipengaruhi oleh pola arus yang terjadi. Dalam menyelesaikan persamaan Hdirodinamika 2D, dapat menggunakan script yang telah diberikan. Pada saat menggunakan script, diperlukan coding dengan menggunakan Google Colab dan library seperti matplotlib dan numphy, selain itu juga diperlukan modul siphon. Dimana dalam mengerjakan script ini diperlukan beberapa data untuk memenuhi nilai parameter yang ada. Berdasarkan data yang telah didapatkan, maka dapat dibuat grafik perhitungan nilai proses awal dengan beberapa parameter. Setelah memasukkan parameter yang diperlukan, maka akan didapat juga  pressure, windspeed, dan water temperature. Untuk mendapatkan hasil dari perhitungan yang telah dilakukan. Selanjutnya klik run untuk mendapatkan hasil grafik yang diperlukan. Dimana nantinya akan didapat 3 grafik dalam menyelesai persamaan ini, yaitu terdapat grafik pressure, windspeed, dan water temperature.

### Script
#### Copyright (c) 2018 Siphon Contributors.
    Distributed under the terms of the BSD 3-Clause License.PDX-License-Identifier: BSD-3-Clause

    NDBC Bouy Meteorological Data Request
    =====================================
    The NDBC keeps a 45-day recent rolling file for each bouy. This examples shows how to access
    the basic meteorogical data from a buoy and make a simple plot.
    """

Import Library phyton
    import matplotlib.pyplot as plt

    from siphon.simplewebservice.ndbc import NDBC

Memasukkan data observasi
    
    # Get a pandas data frame of all of the observations, meteorogical data is the default
    # observation set to query
    df = NDBC.realtime_observations('41033') #Station ID
    df.head()

Menyederhanakan time series

    # Let's make a simple times series plot to checkout what the data look like.
    fig, (ax1, ax2, ax3) = plt.subplots(3, 1, figsize=(12, 10))
    ax2b = ax2.twinx()

Membuat grafik data tekanan terhadap waktu
    
    # Pressure
    ax1.plot(df['time'], df['pressure'], color='black')
    ax1.set_ylabel('Pressure [hPa]')
    fig.suptitle('Kelompok 14_Pemodelan', fontsize=18)

Membuat grafik data kecepatan , tekanan dan arah angin terhadap waktu
   
    # Wind Speed, gust, direction
    ax2.plot(df['time'], df['wind_speed'], color='tab:orange')
    ax2.plot(df['time'], df['wind_gust'], color='tab:olive', linestyle='--')
    ax2b.plot(df['time'], df['wind_direction'], color='tab:blue', linestyle='-')
    ax2.set_ylabel('Wind Speed [m/s]')
    ax2b.set_ylabel('Wind Direction')

Membuat grafik suhu perairan

    # Water temperature
    ax3.plot(df['time'], df['water_temperature'], color='tab:brown')
    ax3.set_ylabel('Water Temperature [degC]')

Show 

    plt.show()

### Hasil

![pemodelan md 4](https://user-images.githubusercontent.com/105905933/170138083-38248fdc-5868-4dc6-9282-340886a558b1.png)

### Pembahasan
Dapat dilihat pada hasil grafik bahwa nilai Pressure tertinggi menyentuh angka 1030 hPa yang mana itu terjadi pada akhir bulan April 2022. Sedangkan untuk nilai Wind Direction dan Wind Speed tidak konstant dan terus berubah-ubah dan mengalami penurunan di akhir bulan April 2022. Yang terakhir, untuk nilai Water Temperature terus mengalami kenaikan dalam rentang waktu 45 hari tersebut hingga akhirnya mencapai angka lebih dari 24 degC. Apabila dilihat dari nilai hasil grafiknya, Stasiun ini memiliki nilai gelombang dan arus yang cukup besar. Stasiun ini sendiri terletak pada koordinat 32.279 N 80.406 W (32°16'44" N 80°24'23" W) yang mana apabila dilihat pada Google Earth maka lokasi stasiun ini terletak di daerah dekat pesisir Pulau Pritchards dan Pulau Hunting di Negara Bagian Amerika Serikat yaitu Carolina Selatan. Artinya baik arus, gelombang, angin atau faktor oseanografi pada Station ini tidak begitu besar seperti yang ada di laut lepas atau di tengah samudera. Selain itu, Station ini juga terletak dekat dengan lokasi Blake Pateau. Lokasi Blake Plateau sendiri memiliki arus yang statis. Namun, daerah ini sendiri masih dapat terpengaruh oleh adanya arus teluk yang berbelok dan menghasilkan pusaran siklon yang disebut dengan Charlestone Gyre (Gula et al., 2015). Arus ini sendiri terjadi akibat dari adanya perbedaan temperatur yang ada disekitar wilayah tersebut yang terdiri dari Blake Plateau, Blake Ridge, Charlestone Bump dll. Adanya anomali ini dapat mengakibatkan nilai yang ada pada grafik menjadi lebih tinggi dari biasanya
