---
title: DIJKSTRA'S ALGORITHM
date: 2025-06-11
categories: [GRAPH]
tags: [daa]
---




![Dijkstra's Algorithm Illustration](/assets/K10.jpeg){: width="500"}

## Prinsip Dasar Dijkstra's Algorithm

Proses utama dari Dijkstra's Algorithm adalah sebagai berikut:

1. **Inisialisasi**  
   Tentukan node sumber dan tetapkan jarak ke node sumber sebagai 0 dan jarak ke semua node lainnya sebagai tak terhingga (∞). Tandai semua node sebagai belum dikunjungi.

2. **Pemilihan Node Terdekat**  
   Pilih node yang memiliki jarak terkecil yang belum dikunjungi. Jika ada beberapa node dengan jarak yang sama, pilih salah satu. Node ini akan diproses terlebih dahulu.

3. **Perbarui Jarak ke Node Tetangga**  
   Untuk setiap tetangga dari node yang dipilih, hitung jarak sementara dari sumber melalui node saat ini. Jika jarak ini lebih kecil dari jarak yang sudah tercatat, perbarui jarak tersebut.

4. **Tandai Node sebagai Dikunjungi**  
   Setelah memproses node dan memperbarui jarak ke tetangganya, tandai node ini sebagai telah dikunjungi dan tidak perlu diproses lagi.

5. **Mengulang Proses**  
   Proses ini diulang hingga semua node telah dikunjungi atau tidak ada jalur yang dapat diperbarui.

### Struktur Data yang Digunakan

- **Min-Heap**: Untuk efisiensi, Dijkstra sering kali menggunakan struktur data heap (atau priority queue) untuk memilih node dengan jarak terkecil secara cepat.
  
- **Visited Set**: Untuk menandai node yang sudah diproses agar tidak diproses lagi.

- **Distance Array**: Array atau tabel yang menyimpan jarak terpendek dari node sumber ke setiap node lainnya.

## Langkah-Langkah Implementasi Dijkstra's Algorithm

1. **Menyiapkan Struktur Data**  
   Tentukan jarak awal untuk semua node (0 untuk sumber, ∞ untuk lainnya). Tentukan set visited dan priority queue.

2. **Pemilihan Node Terdekat**  
   Pilih node dengan jarak terkecil yang belum dikunjungi. Proses node ini dan perbarui jarak ke tetangganya.

3. **Mengulangi Proses**  
   Proses ini dilakukan hingga semua node telah dikunjungi atau jarak ke semua node yang dapat dicapai telah terhitung.

---

## Implementasi Dijkstra's Algorithm dalam C++

Berikut adalah implementasi Dijkstra's Algorithm dalam bahasa C++:

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
using namespace std;

// Struktur untuk menyimpan graf
struct Edge {
    int destination, weight;
};

// Fungsi untuk melakukan Dijkstra
void dijkstra(int start, int n, const vector<vector<Edge>>& graph) {
    // Inisialisasi jarak dari start ke semua node sebagai tak terhingga
    vector<int> distance(n, INT_MAX);
    distance[start] = 0;

    // Priority Queue untuk memilih node dengan jarak terkecil
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    pq.push({0, start});  // Memulai dari node sumber

    while (!pq.empty()) {
        int dist = pq.top().first;  // Jarak dari sumber
        int node = pq.top().second; // Node yang sedang diproses
        pq.pop();

        // Jika jarak node yang sedang diproses lebih besar daripada jarak yang tercatat, lewati
        if (dist > distance[node]) continue;

        // Proses semua tetangga node
        for (const Edge& edge : graph[node]) {
            int nextNode = edge.destination;
            int weight = edge.weight;

            // Jika jalur yang lebih pendek ditemukan
            if (distance[node] + weight < distance[nextNode]) {
                distance[nextNode] = distance[node] + weight;
                pq.push({distance[nextNode], nextNode});
            }
        }
    }

    // Output jarak terpendek dari sumber ke semua node
    cout << "Jarak terpendek dari node " << start << " ke semua node lainnya:" << endl;
    for (int i = 0; i < n; i++) {
        if (distance[i] == INT_MAX) {
            cout << "Node " << i << ": Tidak dapat dijangkau" << endl;
        } else {
            cout << "Node " << i << ": " << distance[i] << endl;
        }
    }
}

int main() {
    int n = 6; // Jumlah node
    vector<vector<Edge>> graph(n);

    // Menambahkan sisi ke dalam graf
    graph[0].push_back({1, 4});
    graph[0].push_back({2, 1});
    graph[1].push_back({3, 1});
    graph[2].push_back({1, 2});
    graph[2].push_back({3, 5});
    graph[3].push_back({4, 3});
    graph[4].push_back({5, 1});
    graph[5].push_back({0, 3});

    int startNode = 0;
    dijkstra(startNode, n, graph);

    return 0;
}
```

# Penjelasan Kode

- **Graph Representation**: Graf direpresentasikan dengan **adjacency list**, di mana setiap node memiliki daftar sisi yang terhubung, dengan masing-masing sisi menyimpan tujuan dan bobotnya.

- **Distance Array**: Array `distance` digunakan untuk menyimpan jarak terpendek dari node sumber ke setiap node lainnya. Inisialisasi jarak sumber dengan 0 dan jarak lainnya dengan tak terhingga (∞).

- **Priority Queue**: **Priority queue** atau **min-heap** digunakan untuk selalu memilih node dengan jarak terpendek yang belum diproses. Heap memastikan bahwa operasi pemilihan node dengan jarak terkecil dilakukan dengan efisien.

- **Dijkstra Function**: Fungsi utama yang mengimplementasikan Dijkstra's Algorithm. Fungsi ini memproses node satu per satu, memperbarui jarak ke tetangga node, dan melanjutkan hingga semua node telah diproses.

---

## Hasil Output

### Contoh Input

Jika kita menggunakan graf berikut dengan node 6 dan sisi yang terhubung:

```
Graf:
0 → 1, 2
1 → 3
2 → 1, 3
3 → 4
4 → 5
5 → 0
```

Maka output dari Dijkstra's Algorithm adalah:

```
Jarak terpendek dari node 0 ke semua node lainnya:
Node 0: 0
Node 1: 3
Node 2: 1
Node 3: 4
Node 4: 7
Node 5: 8
```


### Penjelasan Output

- **Node 0** adalah node sumber, jadi jaraknya adalah 0.
- **Node 1** memiliki jarak terpendek 3, yang didapatkan melalui jalur 0 → 1.
- **Node 2** memiliki jarak terpendek 1, yang didapatkan melalui jalur 0 → 2.
- **Node 3** dapat dicapai dengan jarak terpendek 4 melalui jalur 0 → 2 → 3.
- **Node 4** memiliki jarak terpendek 7 melalui jalur 0 → 2 → 3 → 4.
- **Node 5** dapat dicapai dengan jarak terpendek 8 melalui jalur 0 → 2 → 3 → 4 → 5.

---

## Kesimpulan

Dijkstra's Algorithm adalah algoritma pencarian jalur terpendek pada graf berbobot tanpa sisi negatif, yang bekerja dengan memilih node terdekat secara bertahap menggunakan min-heap dan distance array. Algoritma ini sangat efisien dan digunakan luas dalam pemetaan rute, navigasi, dan analisis jaringan. Namun, Dijkstra tidak cocok untuk graf dengan bobot negatif—untuk itu, algoritma Bellman-Ford lebih sesuai.