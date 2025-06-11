---
title: ACTIVITY SELECTION PROBLEM
date: 2025-05-06
categories: [GREEDY ALGORITHM]
tags: [daa]
---
![Desktop View](/assets/K1.png){: width="500"}
_---_

Masalah Pemilihan Aktivitas (Activity Selection Problem) adalah masalah optimasi yang melibatkan pemilihan sejumlah aktivitas dengan waktu mulai dan selesai tertentu, agar tidak saling tumpang tindih, dan jumlah aktivitas yang dipilih maksimal. Tantangan ini muncul saat sumber daya terbatas, seperti satu ruangan atau prosesor, hanya dapat menangani satu aktivitas dalam satu waktu. Solusinya menggunakan algoritma greedy dengan memilih aktivitas yang selesai paling awal untuk memberi ruang bagi aktivitas lain. Masalah ini sering diterapkan dalam penjadwalan, manajemen proyek, dan pengelolaan waktu.

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