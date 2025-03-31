# 📌 Modul 5: Integrasi React JS dengan Model AI untuk Ringkasan

## 🎯 Tujuan Pertemuan

Pada pertemuan ke-5 ini, peserta akan:

- Memahami konsep integrasi API dalam aplikasi React.
- Menghubungkan aplikasi React dengan model AI (DeepSeek, Google, Meta) menggunakan API OpenRouter.
- Menampilkan hasil ringkasan yang dihasilkan oleh model AI di aplikasi React.

---

## API Integration

### Apa itu API?

API (Application Programming Interface) adalah antarmuka yang memungkinkan komunikasi antara dua aplikasi berbeda, biasanya untuk mengakses data atau layanan eksternal.

---

## 📌 Konsep Asynchronous & Synchronous di React

Secara default, **fungsi di React bersifat synchronous**. Artinya, eksekusi kode akan berjalan baris demi baris secara berurutan. Namun, saat kita berinteraksi dengan **API**, prosesnya menjadi **asynchronous** karena membutuhkan waktu (misalnya: fetch data dari server), sehingga React menggunakan **Promises** dan **async/await** untuk menunggu hasilnya tanpa menghentikan eksekusi kode lainnya.

### Penjelasan Promises & Async/Await

- Promises adalah objek yang merepresentasikan hasil operasi asynchronous. Statusnya bisa:

  > **Pending:** sedang diproses
  > **Fulfilled:** berhasil
  > **Rejected:** gagal

  ```javascript
  const promise = new Promise((resolve, reject) => {
    // operasi asynchronous
  });
  ```

- **Async/Await**

  > `async` digunakan untuk mendefinisikan fungsi asynchronous.  
  > `await` digunakan untuk menunggu hasil Promise sebelum melanjutkan eksekusi kode berikutnya.

  ```javascript
  const getData = async () => {
    try {
      const response = await fetch("https://api.example.com/data");
      const data = await response.json();
      console.log(data);
    } catch (error) {
      console.error(error);
    }
  };
  ```

## 📌 Try-Catch-Finally

**Try-Catch-Finally** adalah blok untuk menangani error:

- **try:** blok utama untuk menjalankan kode.
- **catch:** menangkap error jika terjadi.
- **finally:** selalu dijalankan, baik sukses maupun gagal.

Contoh:

```javascript
const fetchData = async () => {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Terjadi error:", error);
  } finally {
    console.log("Request selesai");
  }
};
```

### Penggunaan file `.env`

File `.env` digunakan untuk menyimpan variabel lingkungan atau konfigurasi yang bersifat rahasia seperti API key. Variabel dalam file ini bisa diakses di aplikasi React dengan awalan `VITE_` (jika menggunakan Vite), misalnya:

```
VITE_OPENROUTER_API_KEY=your_api_key_here
```

Kemudian dapat diakses dalam kode JavaScript dengan cara:

```jsx
const apiKey = import.meta.env.VITE_OPENROUTER_API_KEY;
```

---

## Pengenalan OpenRouter

**OpenRouter** adalah platform aggregator yang menyediakan akses ke berbagai model AI dari banyak vendor (OpenAI, Google, Meta, Mistral, DeepSeek, dsb.) melalui satu API. OpenRouter memungkinkan pengguna menggunakan model-model AI secara gratis (untuk limit tertentu) atau berbayar.

Keuntungan bagi OpenRouter:

- Mendapatkan komisi transaksi saat pengguna membayar model tertentu.
- Memberikan layanan API terpadu agar pengguna tidak perlu mengatur API masing-masing vendor.

---

## Mendapatkan API Key dari OpenRouter

Ikuti langkah berikut untuk mendapatkan API Key dari OpenRouter:

1. Kunjungi situs [OpenRouter](https://openrouter.ai/) dan daftar atau masuk ke akun Anda.
2. Setelah login, buka halaman Dashboard API Keys.
3. Klik tombol **Create new API Key**.
4. Salin dan simpan API key tersebut, yang akan digunakan untuk mengakses model AI.

---

## Referensi Model AI OpenRouter

Kamu bisa melihat daftar lengkap model AI yang tersedia di OpenRouter di:

🔗 [https://openrouter.ai/models](https://openrouter.ai/models)

Beberapa model AI populer dan gratis untuk summarization:

| Vendor   | Model                        | Deskripsi Singkat                                          |
| -------- | ---------------------------- | ---------------------------------------------------------- |
| DeepSeek | `deepseek-chat`              | Model chat general-purpose dari DeepSeek                   |
| Google   | `gemini-pro`                 | Model dari Google Gemini untuk reasoning dan summarization |
| Meta     | `meta-llama/llama-2-7b-chat` | Model open-source dari Meta (Llama2)                       |

---

## Quickstart API OpenRouter

Panduan resmi untuk penggunaan API OpenRouter dapat dibaca di:

🔗 [https://openrouter.ai/docs/quickstart](https://openrouter.ai/docs/quickstart)

---

## 📌 Instalasi React Markdown

Hasil ringkasan dari model AI umumnya berbentuk **Markdown**. Untuk menampilkannya secara rapi di React, kita gunakan paket `react-markdown`.

### Instalasi

```bash
npm install react-markdown
```

### Penggunaan

Pada file komponen:

```jsx
import ReactMarkdown from "react-markdown";

<ReactMarkdown>{summary}</ReactMarkdown>;
```

---

## Tambah State untuk Menyimpan List Model AI dan Efek Loading

```jsx
const [model, setModel] = useState("deepseek/deepseek-chat-v3-0324:free");
const [loading, setLoading] = useState(false);
```

## Modifikasi Komponen Summarizer.jsx dengan Parameter Model AI

Tambahkan UI dropdown untuk memilih model AI gratis dari vendor berbeda:

```jsx
import ReactMarkdown from "react-markdown";

const Summarizer = ({
  inputText,
  setInputText,
  summary,
  handleSummarize,
  handleReset,
  model,
  setModel,
  loading,
}) => {
  return (
    <>
      <p className="mb-4 text-lg">Masukkan teks untuk diringkas:</p>
      <select
        value={model}
        onChange={(e) => setModel(e.target.value)}
        className="mb-4 p-2 border border-gray-300 rounded"
      >
        <option value="deepseek/deepseek-chat-v3-0324:free">DeepSeek V3</option>
        <option value="meta-llama/llama-3.3-70b-instruct:free">
          Llama 3.3 70B Instruct (Meta)
        </option>
        <option value="google/gemini-2.0-flash-exp:free">
          Gemini Flash 2.0 Experimental (Google)
        </option>
      </select>
      // other tag
    </>
  );
};

export default Summarizer;
```

## Koneksi ke Model AI

```jsx
const handleSummarize = async () => {
  if (inputText.trim() === "") return;

  setSummary("");
  setLoading(true);
  // Kirim teks ke API untuk diringkas
  try {
    const response = await fetch(
      "https://openrouter.ai/api/v1/chat/completions",
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${import.meta.env.VITE_OPENROUTER_API_KEY}`,
        },
        body: JSON.stringify({
          model: model,
          messages: [
            {
              role: "user",
              content: `Summarize the following text without any addition answer. Answer in the language the user speaks:\n${inputText}`,
            },
          ],
        }),
      }
    );

    const data = await response.json();
    setSummary(data.choices[0].message.content);
    const newHistory = [...history, data.choices[0].message.content];
    setHistory(newHistory);
    localStorage.setItem("summaryHistory", JSON.stringify(newHistory));
  } catch (error) {
    console.error("Gagal mengambil data ringkasan:", error);
  } finally {
    setLoading(false);
  }
};
```

---

## Kesimpulan Modul 5

- Peserta memahami integrasi API dengan React dan layanan OpenRouter.
- Peserta mampu membuat aplikasi React yang terhubung ke model AI untuk menghasilkan ringkasan teks otomatis.

🚀 **Selanjutnya**, kita akan mendeploy aplikasi agar dapat diakses semua orang.
