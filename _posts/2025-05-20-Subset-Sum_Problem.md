---
title: SUBSET SUM PROBLEM
date: 2025-05-20
categories: [BACKTRACKING ALGORITHM]
tags: [daa]
---

![SUBSET SUM PROBLEM](/assets/K5.png){: width="500"}

_---_

# Penjelasan tentang Subset Sum Problem

**Subset Sum Problem** adalah masalah pencarian subset dari sebuah himpunan yang jumlah elemennya sama dengan nilai target yang telah ditentukan. Misalnya, kita memiliki himpunan angka {3, 34, 4, 12, 5, 2} dan kita ingin mengetahui apakah ada subset dari himpunan tersebut yang jumlahnya sama dengan angka 9. Dalam hal ini, jawabannya adalah "ya", karena subset {4, 5} memiliki jumlah 9.

## Prinsip Dasar Subset Sum Problem

Proses utama untuk menyelesaikan masalah Subset Sum adalah sebagai berikut:

1. **Pencarian Subset**  
   Kita mencoba berbagai subset dari himpunan angka untuk memeriksa apakah jumlahnya sama dengan nilai target.

2. **Pemeriksaan Jumlah**  
   Setelah memilih sebuah subset, kita memeriksa apakah jumlah elemen-elemen dalam subset tersebut sama dengan nilai target.

3. **Pencarian Solusi yang Efisien**  
   Dalam banyak kasus, kita tidak perlu mencoba semua subset secara brute force. Algoritma yang efisien dapat mengurangi jumlah percobaan yang diperlukan.

---

## Langkah-Langkah Implementasi Subset Sum Problem

### 1. Menyiapkan Himpunan Angka dan Target
Tentukan himpunan angka dan target yang ingin dicapai.

### 2. Pemrograman Dinamis
Gunakan pemrograman dinamis untuk menyimpan hasil sub-masalah (apakah subset dengan jumlah tertentu dapat dibentuk atau tidak).

### 3. Pencarian Subset
Gunakan tabel untuk menyimpan hasil dari sub-masalah yang telah dihitung sebelumnya, kemudian tentukan apakah target dapat dicapai dengan menggunakan subset yang ada.

---

## Implementasi Subset Sum Problem dalam C++

Berikut adalah implementasi Subset Sum Problem menggunakan pendekatan pemrograman dinamis dalam bahasa C++:

```cpp
#include <iostream>
#include <vector>

using namespace std;

// Fungsi untuk memeriksa apakah ada subset dengan jumlah yang sama dengan target
bool isSubsetSum(const vector<int>& set, int n, int target) {
    // Membuat tabel untuk menyimpan hasil sub-masalah
    vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));

    // Jika target = 0, maka selalu ada subset kosong yang jumlahnya 0
    for (int i = 0; i <= n; i++) {
        dp[i][0] = true;
    }

    // Mengisi tabel dp untuk setiap elemen dalam set
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= target; j++) {
            if (set[i - 1] <= j) {
                dp[i][j] = dp[i - 1][j] || dp[i - 1][j - set[i - 1]];
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }

    // Mengembalikan hasil apakah ada subset yang jumlahnya sama dengan target
    return dp[n][target];
}

int main() {
    vector<int> set = {3, 34, 4, 12, 5, 2};
    int target = 9;
    int n = set.size();

    if (isSubsetSum(set, n, target)) {
        cout << "Ada subset yang jumlahnya sama dengan " << target << endl;
    } else {
        cout << "Tidak ada subset yang jumlahnya sama dengan " << target << endl;
    }

    return 0;
}
```

## Penjelasan Kode

- **isSubsetSum**: Fungsi untuk memeriksa apakah ada subset dari himpunan yang jumlahnya sama dengan target yang diberikan. Fungsi ini menggunakan pemrograman dinamis dengan tabel `dp[i][j]` yang menunjukkan apakah mungkin untuk mendapatkan jumlah `j` menggunakan elemen pertama hingga ke-`i` dari himpunan. Fungsi ini memeriksa apakah elemen saat ini dapat dimasukkan dalam subset atau tidak.

- **isSubsetSum** (lanjutan): Tabel `dp` diisi berdasarkan dua kemungkinan: apakah elemen saat ini dimasukkan dalam subset atau tidak. Jika tidak dimasukkan, maka nilai `dp[i][j]` disalin dari nilai `dp[i-1][j]` (menunjukkan apakah kita bisa mendapatkan jumlah `j` tanpa elemen tersebut). Jika elemen dimasukkan, nilai `dp[i][j]` diturunkan dari `dp[i-1][j - set[i-1]]` (apakah kita bisa mendapatkan jumlah yang tersisa setelah memasukkan elemen tersebut).

- **main**: Fungsi utama yang memulai program. Di dalam fungsi ini, kita mendefinisikan himpunan angka dan target yang ingin dicapai. Fungsi ini memanggil `isSubsetSum` untuk memeriksa apakah ada subset yang jumlahnya sama dengan target dan mencetak hasilnya.

---

## Hasil Output

### Contoh Input

Jika Anda memasukkan himpunan angka `{3, 34, 4, 12, 5, 2}` dan target `9`:

### Output

`Ada subset yang jumlahnya sama dengan 9`


### Penjelasan Output

- **Himpunan Subset**: Pada himpunan `{3, 34, 4, 12, 5, 2}`, subset `{4, 5}` memiliki jumlah 9, sehingga hasilnya adalah "Ada subset yang jumlahnya sama dengan 9".

---

## Kesimpulan

Masalah Subset Sum adalah masalah klasik yang sering digunakan untuk mengilustrasikan teknik pemrograman dinamis dalam pencarian subset. Dengan memanfaatkan tabel dp, kita dapat menentukan apakah terdapat subset dengan jumlah tertentu secara lebih efisien dibandingkan pendekatan brute force. Pendekatan ini berguna dalam berbagai bidang seperti kriptografi, manajemen sumber daya, dan analisis data, karena mampu menangani ruang solusi besar dengan waktu komputasi yang lebih singkat. Subset Sum menunjukkan bagaimana pemrograman dinamis dapat menyelesaikan masalah optimisasi secara sistematis dan efisien.