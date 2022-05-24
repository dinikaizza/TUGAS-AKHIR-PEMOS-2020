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
Adveksi merupakan mekanisme perpindahan massa suatu materi dari titik ke titik lainnya. Persamaan adveksi sendiri merupakan suatu persamaan gelombang linier orde satu dan termasuk dalam persamaan hiperbolik yang menggambarkan mekanisme trasnportasi suatu gas atau zat cair dengan arah tertentu. Persamaan adveksi memiliki 2 tipe yaitu hitungan yaitu eksplisit dan implisit. Persamaan umum adveksi ∂F/∂t=-u(∂F/∂x)
### Metode Penurunan 
1. FTCS 
merupakan gabungan dari selisih maju terhadap waktu dan selisih pusat terhadap ruang.
3. Leapfrog
4. Upstream

### Adveksi-Difusi 2D
Adveksi Difusi 2 D dan menguji kekonvergenan\ud solusi numerik. Metode beda hingga merupakan suatu metode numerik yang sering digunakan dalam\ud penyelesaian masalah Persamaan Differensial Parsial karena metode ini dapat memberikan solusi yang\ud cukup akurat. Kemampuan metode beda hingga dalam memberikan hasil pendekatan tersebut karena\ud didukung oleh kemajuan yang pesat dalam bidang komputer.
### Metode 
Ada dua skema dasar yang dapat digunakan untuk menyelesaikan persamaan differensial dengan metode beda hingga (FDM), yaitu skema ekspisit dan implisit.
### Script
#!/usr/bin/env python
# coding: utf-8

import matplotlib.pyplot as plt
import numpy as np
import sys

def percentage(part, whole):
    percentage = 100 * float(part)/float(whole)
    return str(round(percentage,2)) + "x"

#Masukan Parameter Awal
C = 0.61
ad = 1.61

#Arah Arus
theta = 61
#theta = 121
#theta = 196
#theta = 376

#Parameter Lanjutan
q = 0.95 
x = 300 
y = 300 
dt = 0.5 
dx = 3 
dy = 3

#Lama Simulasi
Tend = 106
#Tend = 1
dt = 0.5

#Polutan
px = 150
py = 136
Ic = 516

#Perhitungan U dan V
u = C * np.sin(theta*np.pi/180)
v = C * np.cos(theta*np.pi/180)
dt_count = 1/((abs(u)/(q*dx))+(abs(v)/(q*dy))+(2*ad/(q*dx**2))+(2*ad/(q*dy*2)))

Nx = int(x/dx)  #number of mesh in x direction
Ny = int(y/dy)  #number of mesh in y direction
Nt = int(Tend/dt)

#perhitungan titik polutan di buang
px1 = int(px/dx)
py1 = int(py/dy)

#fungsi disederhanakan
lx = u*dt/dx
ly = v*dt/dy
ax = ad*dt/dx**2
ay = ad*dt/dy**2
cfl = (2*ax + 2*ay + abs(lx) + abs(ly))  #syarat kestabilan CFL

#perhitungan cfl
if cfl >= q:
    print('CFL Violated, please use dt :'+str(round(dt_count,4)))
    sys.exit ()
#%%

#pembuatan grid 
x_grid = np.linspace(0-dx, x+dx, Nx+2) #ghostnode boundary
y_grid = np.linspace(0-dx, y+dy, Ny+2) #ghostnode boundary
t = np.linspace(0, Tend, Nt+1)
x_mesh, y_mesh = np.meshgrid(x_grid,y_grid)
F = np.zeros((Nt+1, Ny+2, Nx+2))

#kondisi awal
F[0,py1,px1]=Ic
#%%

#Iterasi
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
    #syarat batas
    F[n+1,0,:] = 0 #bc bawah
    F[n+1,:,0] = 0 #bc kiri
    F[n+1,Ny+1,:] = 0 #bc atas
    F[n+1,:,Nx+1] = 0 #bc kanan
#%%

    #Output Gambar
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

### Hidrodinamika 2D
Secara umum, model hidrodinamika 2D yang dijalankan cukup mampu merepresentasikan kondisi hidrodinamika di lokasi penelitian. Hasil simulasi model menunjukkan bahwa arus cenderung memiliki arah bolak-balik sesuai dengan pasut yang terjadi. Pola sebaran konsentrasi Ammonia, BOD, Nitrat, dan TSS secara signifikan dipengaruhi oleh pola arus yang terjadi. Dalam menyelesaikan persamaan Hdirodinamika 2D, dapat menggunakan script yang telah diberikan. Pada saat menggunakan script, diperlukan coding dengan menggunakan Google Colab dan library seperti matplotlib dan numphy, selain itu juga di perlukan modul siphon. Dimana dalam mengerjakan script ini diperlukan beberapa data untuk memenuhi nilai parameter yang ada. Berdasarkan data yang telah didapatkan, maka dapat dibuat grafik perhitungan nilai proses awal dengan beberapa parameter. Setelah memasukkan parameter yang diperlukan, maka akan didapat juga  pressure, windspeed, dan water temperature. Untuk mendapatkan hasil dari perhitungan yang telah dilakukan. Selanjutnya klik run untuk mendapatkan hasil grafik yang diperlukan. Dimana nantinya akan didapat 3 grafik dalam menyelesai persamaan ini, yaitu terdapat grafik pressure, windspeed, dan water temperature.
