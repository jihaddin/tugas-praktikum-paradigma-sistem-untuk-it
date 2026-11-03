# Tugas 2 — Komunikasi Inter-service dengan gRPC (Polyglot)

## Deskripsi

Praktikum ini membahas komunikasi antar-service menggunakan **gRPC** dengan pendekatan *polyglot*, yaitu penggunaan beberapa bahasa pemrograman dalam satu sistem. 

Pada praktikum ini, **Go** digunakan sebagai *server*, sedangkan **Python** dan **Node.js** digunakan sebagai *client*. Kedua client tersebut dapat meminta data pengguna dari server yang sama melalui komunikasi gRPC.

---

## Tujuan Praktikum

* Memahami arsitektur *Schema-First Development* menggunakan Protocol Buffers.
* Mampu menginstalasi komponen *compiler* gRPC (`protoc`).
* Berhasil membuat gRPC Server menggunakan Go.
* Berhasil membuat gRPC Client menggunakan Python dan Node.js untuk berkomunikasi dengan server Go yang sama.

---

## Struktur Folder Proyek

```text
grpc-polyglot-lab/
├── go-server/          # Implementasi gRPC Server (Golang)
│   ├── pb/             # Generated Go Protobuf Code
│   ├── main.go
│   └── go.mod
├── python-client/      # Implementasi gRPC Client (Python)
│   ├── client.py
│   ├── user_pb2.py
│   └── user_pb2_grpc.py
├── node-client/        # Implementasi gRPC Client (Node.js)
│   ├── client.js
│   └── package.json
├── images/             # Folder dokumentasi tangkapan layar
├── .gitignore          # Pengaturan file yang diabaikan Git
└── user.proto          # Kontrak API Protocol Buffers (Single Source of Truth)
```

---

## Langkah Kerja & Implementasi

1. **Setup Workspace & Git:** Membuat direktori utama `grpc-polyglot-lab`, menginisialisasi Git, dan menambahkan konfigurasi `.gitignore`.
2. **Kontrak API (`user.proto`):** Membuat berkas kontrak layanan (*Single Source of Truth*) yang mendefinisikan `UserRequest`, `UserResponse` (termasuk penambahan field `phone`), dan layanan `UserService`.
3. **gRPC Server (Go):** Inisialisasi modul Go di dalam folder `go-server`, instalasi dependensi gRPC core, serta implementasi *logic* server pada porta TCP `:50051`.
4. **gRPC Client (Python):** Instalasi pustaka `grpcio` dan `grpcio-tools`, kompilasi skema `.proto`, serta pembuatan skrip `client.py`.
5. **gRPC Client (Node.js):** Instalasi `@grpc/grpc-js` dan `@grpc/proto-loader`, serta pembuatan skrip `client.js` dengan pembacaan skema secara dinamis (*runtime*).

---

## Cara Menjalankan Program

Buka 3 jendela terminal terpisah untuk menjalankan *server* dan masing-masing *client* secara bersamaan:

1. **Terminal 1 (Jalankan Server Go):**
   ```bash
   cd go-server
   go run main.go
   ```

2. **Terminal 2 (Jalankan Client Python):**
   ```bash
   cd python-client
   python client.py
   抓```

3. **Terminal 3 (Jalankan Client Node.js):**
   ```bash
   cd node-client
   node client.js
   ```

---

## Hasil Pengujian & Analisis

### 1. Hasil Pengujian Eksekusi Program
* **Server Go:** Berhasil berjalan pada porta `50051` dan mencatat log interaksi secara *real-time* setiap kali ada permintaan masuk (`Menerima request untuk User ID: 1`).
* **Client Python & Node.js:** Berhasil mengirimkan permintaan *RPC* dan menerima respons data profil ("Budi Santoso", "budi@universitas.ac.id") dari *server* Go tanpa kendala.

### 2. Tugas Evaluasi Mandiri
* **Modifikasi Kontrak Data:** Penambahan atribut properti baru berupa `string phone = 3;` pada struktur pesan `UserResponse` di `user.proto`, diikuti re-kompilasi dan penyesuaian logika di *server* Go serta *client*.
* **Analisis Komparasi Kompilasi:** 
  * *Static Code Generation* (Go & Python) menghasilkan file *stub* permanen untuk performa eksekusi optimal dan *type safety*.
  * *Dynamic Runtime Generation* (Node.js) membaca skema `.proto` saat *runtime* menggunakan `@grpc/proto-loader` untuk fleksibilitas tinggi tanpa proses *build* terpisah.

---

## Kesimpulan

Praktikum komunikasi *inter-service* dengan gRPC (*Polyglot*) berhasil diselesaikan dengan baik. Seluruh layanan heterogen (Go, Python, dan Node.js) dapat saling berkomunikasi secara stabil menggunakan satu kontrak skema tunggal (`user.proto`), dan seluruh kode sumber telah di-*push* ke repositori GitHub.

---

