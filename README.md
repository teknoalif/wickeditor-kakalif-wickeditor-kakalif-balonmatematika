# wickeditor-kakalif-wickeditor-kakalif-balonmatematika

## 🎈 Game Balon Matematika - Wick Editor

Sebuah game edukatif interaktif berbasis matematika yang dibuat menggunakan **Wick Editor**. Dalam permainan ini, pemain harus menjawab soal matematika sebelum balon mencapai batas atas layar. Jika jawabannya benar, balonnya akan meledak — jika tidak, permainan berakhir!

---

### 🎮 Fitur Permainan

* Soal matematika acak: penjumlahan dan perkalian.
* Input jawaban menggunakan keyboard.
* Skor bertambah untuk jawaban benar, dan berkurang untuk jawaban salah.
* Balon meledak jika jawaban benar.
* Permainan berakhir (game over) jika balon mencapai batas atas.

---

### 🧠 Tujuan Edukasi

Permainan ini dirancang untuk melatih:

* Kecepatan berpikir dalam operasi matematika dasar.
* Kemampuan mengetik angka dengan akurat.
* Konsentrasi dan refleks dalam pembelajaran yang menyenangkan.

---

### 🛠️ Teknologi yang Digunakan

* **Wick Editor** – platform open-source sebagai alternatif Macromedia Flash.
* Bahasa: JavaScript (internal Wick scripting).
* Aset: Bentuk vektor dan teks.

---

### 🚀 Coba Permainannya

**🔗 GitHub Pages (Online Demo)**
👉 [Mainkan di Sini](https://teknoalif.github.io/wickeditor-kakalif-balonmatematika)

---

### 📂 Struktur Proyek

```
📁 wickeditor-kakalif-balonmatematika
├── index.html                <- Versi ekspor HTML dari Wick Editor
├── Kak Alif-Balon Matematika-7-24-2025_23-35-36.wick     <- File asli proyek Wick Editor
└── README.md                <- Dokumentasi proyek
```

---

### 📦 Cara Menjalankan Secara Lokal

1. Unduh dan buka file `.wick` di [Wick Editor Offline](https://www.wickeditor.com/#download).
2. Jalankan proyek langsung dari Wick, atau ekspor ke HTML.
3. Buka `index.html` di browser untuk mencoba.

---

### 📚 Referensi dan Inspirasi

🎥 Tutorial asli dan sumber skrip diadaptasi dari:
**Channel YouTube**: [Jovanny Game Dev](https://www.youtube.com/@JovannyGameDev)
📺 Video referensi: [How To Make A Math Game | Wick Editor Tutorial](https://www.youtube.com/watch?v=8Yr-d5gxtyM)

Proyek ini dimodifikasi dan dikembangkan ulang oleh [@teknoalif](https://github.com/teknoalif) sebagai latihan awal dalam transisi dari Macromedia Flash MX ke Wick Editor.

---

### 📝 Lisensi

Proyek ini bebas digunakan untuk tujuan edukasi dan pengembangan pribadi.
Mohon cantumkan sumber jika digunakan ulang atau dimodifikasi untuk publik.

---

### 🙌 Kontribusi

Suka proyek ini?
Silakan **fork**, tambahkan fitur baru seperti:

* Operator pengurangan atau pembagian
* Level permainan
* Suara dan efek animasi
* Sistem waktu atau nyawa

---


### 🧠 Script untuk Frame UI

#### ✅ Default Script

```javascript
// Inisialisasi waktu dan batas waktu antar balon muncul
time = 0;
maxTime = 3 * project.framerate; // Waktu maksimum dalam frame (misalnya 3 detik)

// Kosongkan input jawaban di awal permainan
numTxt.setText("");

// Daftar angka yang bisa diketik oleh pemain
numberList = ['1','2','3','4','5','6','7','8','9','0'];

// Daftar operator matematika yang akan dipakai
operatorList = ['+', '*'];

// Skor awal
score = 0;
scoreTxt.setText(score);

// Soal yang sedang aktif
currentProblem = "";

// Teks Game Over disembunyikan dulu
gameOverTxt.setText("");

// Status permainan: belum selesai
gameOver = false;
```

---

#### 🔁 Update Script

```javascript
// Hanya berjalan jika permainan belum selesai
if (!gameOver) {

    // Periksa apakah tombol angka ditekan
    for (let num of numberList) {
        if (isKeyJustPressed(num)) {
            // Batasi panjang input maksimal 3 karakter
            if (numTxt.textContent.length < 3) {
                // Tambahkan angka yang diketik ke layar input
                numTxt.setText(numTxt.textContent + num);
            }
        }
    }

    // Saat pemain menekan Enter
    if (isKeyJustPressed("enter")) {
        let userAnswer = numTxt.textContent; // Ambil jawaban pemain

        if (userAnswer != "") {
            numTxt.setText(""); // Kosongkan input setelah menjawab
            let correct = false;

            // Periksa semua balon yang sedang aktif
            for (let bal of ballon.clones) {
                // Hanya periksa balon pada frame ke-1 (soal aktif)
                if (bal.currentFrameNumber == 1) {
                    // Jika jawaban benar
                    if (userAnswer == bal.ans) {
                        bal.gotoAndPlay(2); // Jalankan animasi ledakan
                        correct = true;
                        score += 100; // Tambah skor
                    }
                }
            }

            // Jika jawaban salah, kurangi skor
            if (!correct) {
                score -= 25;
            }

            // Perbarui tampilan skor
            scoreTxt.setText(score);
        }
    }

    // Hapus input jika pemain menekan Backspace
    if (isKeyJustPressed("backspace")) {
        numTxt.setText("");
    }

    // Waktu habis → munculkan balon baru
    if (time++ >= maxTime) {
        time = 0;

        // Buat soal baru secara acak
        let operator = random.choice(operatorList);
        let num1 = random.integer(0, 9);
        let num2 = random.integer(0, 9);
        currentProblem = num1 + ' ' + operator + ' ' + num2;

        // Kloning balon baru dan atur posisinya
        let bal = ballon.clone();
        bal.x = random.integer(30, 470); // Posisi X acak
        bal.y = 800; // Posisi Y di bawah layar

        // Kurangi waktu jeda antar balon seiring waktu → makin sulit
        if (maxTime > 20) {
            maxTime -= 1;
        }
    }
}
```

---

### 🎈 Script untuk Clip **Ballon**

#### ✅ Default Script

```javascript
// Script ini hanya berjalan jika objek ini adalah kloningan
if (this.isClone) {
    // Jika soal mengandung simbol '+', tampilkan tampilan khusus (opsional)
    if (currentProblem.includes('+')) {
        this.body.gotoAndStop(2); // Ganti tampilan visual
    }

    // Tampilkan soal matematika di atas balon
    this.equ.setText(currentProblem);

    // Simpan jawaban soal di properti balon
    this.ans = eval(currentProblem); // Menghitung hasil dari string soal
}
```

---

#### 🔁 Update Script

```javascript
// Script ini hanya berjalan jika ini adalah klon balon
if (this.isClone) {

    // Jika permainan belum selesai, balon terus naik
    if (!gameOver) {
        this.y -= 0.5; // Balon naik perlahan

        // Jika balon sudah terlalu atas, permainan selesai
        if (this.y < 190) {
            gameOver = true;
            gameOverTxt.setText("GAME OVER"); // Tampilkan pesan
        }

    } else {
        // Jika sudah meledak tapi belum ganti frame, paksa ledakannya muncul
        if (this.currentFrameNumber == 1) {
            this.gotoAndPlay(2); // Animasi ledakan
        }
    }
}
```

---



 
