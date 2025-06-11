---
title: HUFFMAN CODING
date: 2025-05-20
categories: [GREEDY ALGORITHM]
tags: [daa]    
---

![Desktop View](/assets/K3.png){: width="500"}
_---_

Huffman Coding adalah algoritma kompresi data lossless yang efisien dan banyak digunakan sejak dikembangkan pada tahun 1952. Algoritma ini memberi kode biner yang lebih pendek untuk karakter yang sering muncul dan kode lebih panjang untuk karakter yang jarang muncul, sehingga ukuran data dapat dikompresi secara signifikan. Huffman Coding menjadi dasar berbagai format kompresi populer seperti ZIP, JPEG, dan MP3.

---

**Huffman coding** adalah algoritma kompresi data *lossless* yang dikembangkan oleh David A. Huffman pada tahun 1952.

* Digunakan untuk mengurangi ukuran data dengan cara mengganti simbol yang sering muncul dengan kode bit yang lebih pendek.
* Umumnya digunakan dalam format kompresi seperti ZIP, JPEG, dan MP3.

### Konsep Dasar

Prinsip kerja Huffman Coding didasarkan pada frekuensi karakter dalam data.

* Karakter dengan frekuensi tinggi akan diberikan kode yang lebih pendek.
* Karakter dengan frekuensi rendah akan diberikan kode yang lebih panjang.
* Representasi data menggunakan pohon biner.

## Proses Pembuatan Kode

Langkah-langkah dalam proses pembuatan kode Huffman Coding adalah sebagai berikut:
1.  Hitung frekuensi tiap karakter dalam data.
2.  Buat simpul (*node*) untuk setiap karakter.
3.  Gabungkan dua simpul dengan frekuensi terkecil.
4.  Ulangi langkah 3 hingga terbentuk satu pohon Huffman.
5.  Tentukan kode biner untuk tiap karakter (0 untuk sisi kiri, 1 untuk sisi kanan).

---


## Implementasi Huffman Coding dalam C++

Berikut adalah implementasi sederhana dari Huffman Coding dalam C++:

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
#include <vector>
#include <string>

using namespace std;

// Struktur data untuk merepresentasikan node pada pohon Huffman
struct Node {
    char data;
    int freq;
    Node *left, *right;

    Node(char data, int freq) {
        this->data = data;
        this->freq = freq;
        left = right = nullptr;
    }
};

// Membandingkan dua node berdasarkan frekuensi
struct compare {
    bool operator()(Node* left, Node* right) {
        return left->freq > right->freq;
    }
};

// Membangun pohon Huffman dari data input
Node* buildHuffmanTree(const unordered_map<char, int>& freq_map) {
    priority_queue<Node*, vector<Node*>, compare> minHeap;

    // Menambahkan semua karakter dan frekuensinya ke min-heap
    for (auto pair : freq_map) {
        minHeap.push(new Node(pair.first, pair.second));
    }

    // Membuat pohon Huffman
    while (minHeap.size() > 1) {
        // Mengambil dua node dengan frekuensi terkecil
        Node* left = minHeap.top(); minHeap.pop();
        Node* right = minHeap.top(); minHeap.pop();

        // Membuat node baru yang merupakan gabungan dari dua node tersebut
        Node* newNode = new Node('$', left->freq + right->freq);
        newNode->left = left;
        newNode->right = right;

        // Menambahkan node baru ke dalam min-heap
        minHeap.push(newNode);
    }

    // Mengembalikan root pohon Huffman
    return minHeap.top();
}

// Fungsi untuk menghasilkan kode Huffman
void generateHuffmanCodes(Node* root, string str, unordered_map<char, string>& huffmanCodes) {
    if (root == nullptr) {
        return;
    }

    // Jika kita sampai pada daun (simbol yang di-encode)
    if (root->data != '$') {
        huffmanCodes[root->data] = str;
    }

    // Rekursif untuk subtree kiri dan kanan
    generateHuffmanCodes(root->left, str + "0", huffmanCodes);
    generateHuffmanCodes(root->right, str + "1", huffmanCodes);
}

// Fungsi utama untuk mengompresi data menggunakan Huffman Coding
void huffmanCompress(const string& text) {
    unordered_map<char, int> freq_map;

    // Menghitung frekuensi kemunculan setiap karakter
    for (char ch : text) {
        freq_map[ch]++;
    }

    // Membangun pohon Huffman
    Node* root = buildHuffmanTree(freq_map);

    // Menghasilkan kode Huffman untuk setiap karakter
    unordered_map<char, string> huffmanCodes;
    generateHuffmanCodes(root, "", huffmanCodes);

    // Menampilkan hasil kode Huffman
    cout << "Kode Huffman untuk setiap karakter:" << endl;
    for (auto pair : huffmanCodes) {
        cout << pair.first << ": " << pair.second << endl;
    }

    // Mengompresi teks
    string compressedText = "";
    for (char ch : text) {
        compressedText += huffmanCodes[ch];
    }

    cout << "\nTeks setelah dikompresi: " << compressedText << endl;
}

int main() {
    string text;
    cout << "Masukkan teks untuk dikompresi: ";
    getline(cin, text);

    huffmanCompress(text);
    return 0;
}
```

## Penjelasan Kode

- **Node**: Struktur data untuk merepresentasikan setiap node dalam pohon Huffman. Setiap node memiliki data karakter (`data`), frekuensi (`freq`), dan dua pointer ke node anak kiri dan kanan.

- **Min-Heap**: Digunakan untuk membangun pohon Huffman, dengan memilih dua node dengan frekuensi terkecil untuk digabungkan. Node-node ini digabung menjadi satu node baru dengan frekuensi gabungan.

- **generateHuffmanCodes**: Fungsi rekursif yang menurunkan kode Huffman untuk setiap karakter berdasarkan jalur dari akar pohon. Setiap langkah kiri diwakili dengan `'0'`, dan langkah kanan dengan `'1'`.

- **huffmanCompress**: Fungsi utama yang mengompresi teks dengan menggunakan kode Huffman yang dihasilkan. Pertama, menghitung frekuensi kemunculan setiap karakter, membangun pohon Huffman, dan kemudian mengompresi teks.

---

## Hasil Output

### Contoh Input

Jika Anda memasukkan teks berikut:  `abcaacb`


### Output


### Penjelasan Output:

- **Kode Huffman**:
  - `a` diberi kode `01`,
  - `b` diberi kode `00`,
  - `c` diberi kode `10`.

- **Teks yang Dicompressi**:
  Teks `abcaacb` setelah dikompresi menjadi `010010011001000`, di mana setiap karakter telah diganti dengan kode Huffman yang sesuai.
