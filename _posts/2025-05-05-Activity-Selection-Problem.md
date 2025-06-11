---
title: ACTIVITY SELECTION PROBLEM
date: 2025-05-06
categories: [GREEDY ALGORITHM]
tags: [daa, greedy]
---

# Presentasi Kelompok 1: Activity Selection Problem

Hola!. Kali ini kita akan membahas sebuah konsep fundamental dalam bidang algoritma dan optimasi, yaitu **Activity Selection Problem** yang merupakan bagian presentasi Kelompok 1.

![Desktop View](/assets/K1.png){: width="500"}
_---_

Masalah **Pemilihan Aktivitas** (*Activity Selection Problem*) merupakan contoh klasik dari permasalahan optimisasi yang kerap ditemui baik dalam kehidupan nyata maupun dalam sistem komputasi, di mana kita memiliki sekumpulan tugas atau aktivitas yang masing-masing memiliki waktu mulai dan selesai. Tantangan muncul ketika sumber daya terbatas—seperti satu ruangan rapat, satu prosesor, atau satu orang—hanya dapat menangani satu aktivitas pada satu waktu, sehingga kita perlu memilih subset aktivitas yang tidak saling tumpang tindih untuk memaksimalkan jumlah aktivitas yang dapat diselesaikan. Solusinya dapat dicapai dengan algoritma *greedy*, yaitu memilih aktivitas yang selesai paling awal untuk memberi ruang bagi aktivitas berikutnya. Memahami prinsip di balik *Activity Selection Problem* penting tidak hanya sebagai dasar algoritma, tetapi juga untuk penerapannya dalam penjadwalan kegiatan, manajemen proyek, dan pengelolaan waktu yang lebih baik.


Mari kita selami lebih dalam untuk memahami bagaimana kita dapat secara efisien memilih aktivitas-aktivitas ini untuk memaksimalkan pemanfaatan sumber daya kita.

---

## Definisi Masalah

Diberikan sebuah set aktivitas yang memiliki waktu mulai dan waktu selesai. Tugasnya adalah memilih aktivitas sebanyak mungkin sehingga tidak ada dua aktivitas yang tumpang tindih (overlap).

### Contoh Problem dan Solusi

Diberikan tabel aktivitas berikut:

| Aktivitas | Mulai (s) | Selesai (f) |
| :-------- | :-------- | :---------- |
| A1        | 1         | 4           |
| A2        | 3         | 5           |
| A3        | 0         | 6           |
| A4        | 5         | 7           |
| A5        | 8         | 9           |
| A6        | 5         | 9           |

Tujuan kita adalah memilih aktivitas sebanyak mungkin sehingga tidak ada dua aktivitas yang tumpang tindih.
### Langkah-langkah:

1. **Urutkan aktivitas berdasarkan waktu selesai (finish time).** Aktivitas dengan waktu selesai yang lebih awal akan dipilih terlebih dahulu.
2. **Pilih aktivitas pertama** dari daftar yang sudah diurutkan, karena aktivitas ini selesai paling cepat.
3. **Untuk setiap aktivitas berikutnya, pilih aktivitas tersebut** jika waktu mulai aktivitas tersebut lebih besar atau sama dengan waktu selesai aktivitas yang telah dipilih sebelumnya.
4. **Ulangi** sampai semua aktivitas dipertimbangkan.


## Langkah Solusi:

1.  **Urutkan berdasarkan waktu selesai:** [A1, A2, A3, A4, A5, A6].
2.  **Pilih A1** (selesai paling awal).
3.  **Iterasi:**
    * A2, A3: Tumpang tindih (❌).
    * A4: Kompatibel ✅ → {A1, A4}.
    * A5: Kompatibel ✅ → {A1, A4, A5}.
    * A6: Tumpang tindih (❌).
4.  **Solusi Optimal:** {A1, A4, A5} (3 aktivitas).

## Algoritma Serakah (Greedy Algorithm)

Pendekatan **Greedy Algorithm** untuk menyelesaikan masalah pemilihan aktivitas berdasarkan prinsip memilih aktivitas yang memiliki waktu selesai paling awal. Dengan memilih aktivitas yang selesai lebih cepat, kita akan memiliki ruang lebih banyak untuk memilih aktivitas lainnya.

### Implementasi Greedy Algorithm dalam C++

Berikut adalah implementasi kode C++ untuk menyelesaikan masalah pemilihan aktivitas:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Struktur untuk menyimpan data aktivitas
struct Activity {
    int start;  // Waktu mulai
    int finish; // Waktu selesai
};

// Fungsi untuk mengurutkan aktivitas berdasarkan waktu selesai
bool compare(Activity a, Activity b) {
    return a.finish < b.finish;
}

vector<Activity> activitySelection(vector<Activity>& activities) {
    // Urutkan aktivitas berdasarkan waktu selesai
    sort(activities.begin(), activities.end(), compare);

    vector<Activity> selectedActivities;
    selectedActivities.push_back(activities[0]);  // Pilih aktivitas pertama

    int lastSelectedFinishTime = activities[0].finish;

    // Pilih aktivitas yang tidak tumpang tindih
    for (int i = 1; i < activities.size(); i++) {
        if (activities[i].start >= lastSelectedFinishTime) {
            selectedActivities.push_back(activities[i]);
            lastSelectedFinishTime = activities[i].finish;
        }
    }

    return selectedActivities;
}

int main() {
    vector<Activity> activities = {
        {1, 4}, {3, 5}, {0, 6}, {5, 7}, {3, 8}, {5, 9}, {6, 10}, {8, 11}
    };

    vector<Activity> selected = activitySelection(activities);

    cout << "Aktivitas yang dipilih:" << endl;
    for (const auto& activity : selected) {
        cout << "Start: " << activity.start << ", Finish: " << activity.finish << endl;
    }

    return 0;
}
```