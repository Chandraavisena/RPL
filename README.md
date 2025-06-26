import csv
import os
from datetime import datetime
import pandas as pd
import matplotlib.pyplot as plt

DATA_FILE = "data/waste_data.csv"

# Buat folder data jika belum ada
os.makedirs("data", exist_ok=True)

# Inisialisasi file CSV jika belum ada
if not os.path.exists(DATA_FILE):
    with open(DATA_FILE, mode='w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(["Tanggal", "Produk", "Jenis Plastik", "Jumlah (gram)"])

# Fungsi untuk input data
def input_data():
    print("=== Input Limbah Plastik Baru ===")
    produk = input("Nama Produk (contoh: Air Mineral, Snack, dll): ")
    jenis = input("Jenis Plastik (PET, HDPE, LDPE, dll): ")
    jumlah = float(input("Jumlah limbah (gram): "))
    tanggal = datetime.now().strftime("%Y-%m-%d")

    with open(DATA_FILE, mode='a', newline='') as file:
        writer = csv.writer(file)
        writer.writerow([tanggal, produk, jenis, jumlah])
    
    print("Data berhasil ditambahkan!")

# Fungsi untuk melihat ringkasan
def tampilkan_ringkasan():
    df = pd.read_csv(DATA_FILE)
    if df.empty:
        print("Belum ada data!")
        return

    print("\n=== Ringkasan Data ===")
    print(df.groupby("Jenis Plastik")["Jumlah (gram)"].sum())
    print("\nTotal Limbah: {:.2f} gram".format(df["Jumlah (gram)"].sum()))

# Fungsi untuk membuat grafik
def buat_grafik():
    df = pd.read_csv(DATA_FILE)
    if df.empty:
        print("Tidak ada data untuk divisualisasikan.")
        return
    
    df_grouped = df.groupby("Jenis Plastik")["Jumlah (gram)"].sum()

    plt.figure(figsize=(8,5))
    df_grouped.plot(kind="bar", color="green")
    plt.title("Total Limbah Plastik per Jenis")
    plt.xlabel("Jenis Plastik")
    plt.ylabel("Jumlah (gram)")
    plt.grid(True)
    plt.tight_layout()
    plt.savefig("data/grafik_limbah.png")
    plt.show()
    print("Grafik disimpan sebagai 'data/grafik_limbah.png'")

# Menu utama
def main():
    while True:
        print("\n=== Sistem Tracking Limbah Plastik ===")
        print("1. Input Data Limbah")
        print("2. Tampilkan Ringkasan")
        print("3. Tampilkan Grafik")
        print("4. Keluar")
        pilihan = input("Pilih menu (1/2/3/4): ")

        if pilihan == "1":
            input_data()
        elif pilihan == "2":
            tampilkan_ringkasan()
        elif pilihan == "3":
            buat_grafik()
        elif pilihan == "4":
            print("Keluar dari program. Terima kasih.")
            break
        else:
            print("Pilihan tidak valid!")

if __name__ == "__main__":
    main()
