---
title: FRACTIONAL KNAPSACK
date: 2025-05-05
categories: [ GREEDY ALGORITHM]
tags: [daa]
---
![Desktop View](/assets/K2.png){: width="500"}

## Definisi

Masalah Fractional Knapsack adalah masalah optimasi yang memungkinkan pengambilan sebagian dari barang untuk dimasukkan ke dalam tas dengan kapasitas terbatas, dengan tujuan memaksimalkan nilai total. Strategi yang digunakan adalah memilih barang berdasarkan nilai per satuan bobot tertinggi agar kapasitas digunakan seefisien mungkin.


## Algoritma Fractional Knapsack

Langkah-langkah untuk menyelesaikan masalah fractional knapsack adalah sebagai berikut:

1. Hitung nilai per satuan bobot untuk setiap barang.
2. Urutkan barang berdasarkan nilai per satuan bobot secara menurun.
3. Masukkan barang ke dalam knapsack dengan kapasitas terbatas sampai penuh.

#### Contoh 2:

Anda memiliki knapsack dengan kapasitas 10 kg. Item-item:

| Item | Berat (kg) | Nilai (*) |
| :--- | :--------- | :-------- |
| X    | 6          | 30        |
| Y    | 4          | 20        |
| Z    | 3          | 15        |

**Solusi:**

1.  **Hitung Rasio Nilai per Berat:**
    * Item X: *30 / 6 = 5*
    * Item Y: *20 / 4 = 5*
    * Item Z: *15 / 3 = 5*

2.  **Urutkan Item:** Karena semua rasio sama, urutan relatif tidak masalah. Mari kita asumsikan urutan X, Y, Z.

3.  **Pilih Item:**
    * **Kapasitas Awal:** 10 kg
    * **Pilih Item X:** Berat 6 kg, Nilai 30.
        * Sisa kapasitas: *10 - 6 = 4* kg
        * Total nilai: *30
    * **Pilih Item Y:** Berat 4 kg, Nilai 20.
        * Sisa kapasitas: *4 - 4 = 0* kg
        * Total nilai: *30 + 20 = 50

    Knapsack sudah penuh.

**Total Nilai Maksimum:** *50

## Implementasi dalam C++

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Struktur untuk merepresentasikan barang
struct Item {
    int weight;  // Bobot barang
    int value;   // Nilai barang
    double valuePerWeight;  // Nilai per satuan bobot
    
    // Konstruktor untuk menyimpan nilai barang
    Item(int w, int v) {
        weight = w;
        value = v;
        valuePerWeight = (double)v / w;  // Menghitung nilai per satuan bobot
    }
};

// Fungsi untuk membandingkan nilai per bobot dari dua barang
bool compare(Item a, Item b) {
    return a.valuePerWeight > b.valuePerWeight;  // Urutkan dari yang terbesar
}

// Fungsi untuk menyelesaikan masalah Fractional Knapsack
double fractionalKnapsack(int capacity, vector<Item>& items) {
    // Urutkan barang berdasarkan nilai per bobot secara menurun
    sort(items.begin(), items.end(), compare);
    
    double totalValue = 0.0; // Nilai total yang dimasukkan ke dalam knapsack
    for (const Item& item : items) {
        if (capacity == 0) break; // Jika kapasitas knapsack sudah habis
        
        // Ambil barang sebanyak mungkin
        if (item.weight <= capacity) {
            // Ambil seluruh barang
            totalValue += item.value;
            capacity -= item.weight;
        } else {
            // Ambil sebagian barang
            totalValue += item.valuePerWeight * capacity;
            break;
        }
    }
    
    return totalValue;
}

int main() {
    int n, capacity;
    
    // Input jumlah barang dan kapasitas knapsack
    cout << "Masukkan jumlah barang: ";
    cin >> n;
    cout << "Masukkan kapasitas knapsack: ";
    cin >> capacity;
    
    vector<Item> items;
    
    // Input nilai dan bobot barang
    for (int i = 0; i < n; i++) {
        int weight, value;
        cout << "Masukkan bobot dan nilai barang ke-" << i + 1 << ": ";
        cin >> weight >> value;
        items.push_back(Item(weight, value));  // Membuat objek barang dan menambahkannya ke vector
    }
    
    // Menghitung nilai maksimal yang dapat dimasukkan ke dalam knapsack
    double maxValue = fractionalKnapsack(capacity, items);
    cout << "Nilai maksimal yang dapat dimasukkan ke dalam knapsack adalah: " << maxValue << endl;
    
    return 0;
}
```

## Output dari Implementasi C++

Misalkan kita menginput data sebagai berikut:

- Jumlah barang: `4`
- Kapasitas knapsack: `50`
- Barang dengan nilai dan bobot:
  - Barang 1: Nilai = `60`, Bobot = `10`
  - Barang 2: Nilai = `100`, Bobot = `20`
  - Barang 3: Nilai = `120`, Bobot = `30`
  - Barang 4: Nilai = `200`, Bobot = `40`

#### Input:

```
Masukkan jumlah barang: 4
Masukkan kapasitas knapsack: 50
Masukkan bobot dan nilai barang ke-1: 10 60
Masukkan bobot dan nilai barang ke-2: 20 100
Masukkan bobot dan nilai barang ke-3: 30 120
Masukkan bobot dan nilai barang ke-4: 40 200
```

#### Output:

```
Nilai maksimal yang dapat dimasukkan ke dalam knapsack adalah: 240
```

## Penjelasan Output

1. **Barang yang diambil:**
   - **Barang 4 (Nilai 200, Bobot 40)**: Dimasukkan sepenuhnya karena kapasitas knapsack mencukupi.
   - **Barang 3 (Nilai 120, Bobot 30)**: Hanya sebagian yang diambil karena kapasitas knapsack sudah terpakai. Kapasitas yang tersisa adalah `10`, dan kita ambil nilai per bobot yang proporsional.

2. **Perhitungan nilai:**
   - Nilai dari Barang 4 (sepenuhnya): `200`
   - Nilai dari Barang 3 (sebagian, hanya 10 unit dari 30 unit bobot): `(120 / 30) * 10 = 40`
   - Total nilai yang dimasukkan ke dalam knapsack = `200 + 40 = 240`

## Kesimpulan

Masalah **Fractional Knapsack** menggunakan pendekatan **greedy** untuk memaksimalkan nilai total barang yang dimasukkan ke dalam knapsack dengan kapasitas terbatas. Dalam implementasi ini, kita:
- Menghitung **nilai per satuan bobot** untuk setiap barang.
- Mengurutkan barang berdasarkan nilai per bobot dari yang terbesar ke terkecil.
- Memasukkan barang ke dalam knapsack, baik sepenuhnya atau sebagian (jika tidak dapat dimasukkan sepenuhnya), dengan memanfaatkan kapasitas yang ada secara maksimal.

Pendekatan ini memastikan kita mendapatkan solusi yang optimal untuk masalah fractional knapsack, dengan kompleksitas waktu yang efisien yaitu O(n log n), karena kita mengurutkan barang berdasarkan nilai per bobot.

### Catatan:
- Jika kapasitas knapsack lebih besar dari jumlah total bobot barang, kita akan memilih barang-barang secara penuh.
- Jika kapasitas knapsack terbatas, barang-barang dengan nilai per satuan bobot tertinggi akan diprioritaskan untuk dimasukkan terlebih dahulu, dan sebagian barang akan diambil jika kapasitasnya tidak mencukupi untuk seluruh barang.