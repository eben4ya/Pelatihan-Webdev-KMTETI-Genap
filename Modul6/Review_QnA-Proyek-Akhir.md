# 📌 Modul 6: Review & Q\&A Proyek Akhir

## 🎯 Tujuan Pertemuan

Pada modul ke-6 ini, peserta akan:

- Mereview keseluruhan materi (Modul 1–5) dan capaian proyek akhir.
- Membahas kendala umum yang dihadapi saat mengerjakan proyek.
- Menyediakan sesi tanya jawab (Q\&A) untuk menemukan solusi bersama.

---

## Modul 1: Setup & Dasar Web Development

Pada modul ini peserta mempelajari fondasi pengembangan web, mulai dari instalasi alat hingga struktur dasar HTML/CSS.

**Poin-Poin Penting:**

- **Workflow Web**: Cara browser & server berkomunikasi (request → response → render).
- **Git & GitHub**: `git init` → `git add` → `git commit` → `git push`, konfigurasi user.name/user.email.
- **VS Code & Extensions**: Prettier, Live Server, HTML/CSS Support, Auto Close/Rename Tag, Color Highlight.
- **HTML Dasar**: Struktur dokumen (`<!DOCTYPE html>`, `<head>`, `<body>`), meta viewport, linking CSS.
- **CSS Dasar**: Reset global, styling body/header/textarea/button, hover transition.

---

## Modul 2: Tailwind CSS & Responsive UI

Modul ini mengenalkan “utility-first” Tailwind CSS, CDN, dan cara membangun antarmuka responsif tanpa menulis banyak CSS kustom.

**Poin-Poin Penting:**

- **Utility-First**: Kelas seperti `px-6`, `bg-blue-600`, `rounded` di-HTML menggantikan properti CSS.
- **CDN Tailwind**: `<script src="https://cdn.tailwindcss.com">` → file di-unduh via global CDN → cepat & bebas konfigurasi.
- **Kelebihan**:

  - Produktivitas tinggi (langsung pakai kelas utilitas)
  - Konsistensi desain
  - Responsiveness gampang (`sm:`, `md:`, `lg:`)
  - Minim CSS kustom → maintainable

- **Implementasi**:

  - Ganti `style.css` global + CDN
  - Komponen UI: header, textarea, tombol Ringkas/Reset, section Hasil & Riwayat
  - Flexbox responsif: `flex flex-col sm:flex-row gap-4`

---

## Modul 3: Dasar JavaScript untuk React

Peserta memahami JavaScript modern di browser & luar browser (Node.js), lalu menerapkannya untuk membuat interaktivitas ringkas/reset/delete history di versi vanilla.

**Poin-Poin Penting:**

- **Node.js**: Runtime V8 + npm → jalankan JS di server/laptop; instal via installer (Windows), apt/yarn, nvm, brew.
- **Tipe Data & Syntax**: string, number, boolean, `if/else`, `for` loop, `forEach`, arrow functions, destructuring.
- **DOM Manipulation**: `getElementById`, `createElement`, `innerText`, `innerHTML`.
- **Event Handling**: `onclick="ringkas()"` vs. external `script.js`.
- **Local Storage**: `localStorage.getItem()`/`setItem()` untuk simpan & load riwayat ringkasan.
- **Modularisasi**: Pisahkan `index.html` & `main.js` demi kebersihan kode.

---

## Modul 4: React Basic dengan Vite

Modul ini beralih ke React: instalasi via Vite, penggunaan JSX, komponen, state & effect, props, plus setup Tailwind di proyek React.

**Poin-Poin Penting:**

- **React & Vite**:

  - `npm create vite@latest my-app --template react`
  - `npm install && npm run dev`

- **JSX**: Ekstensi `.jsx` untuk tulis HTML-like di JS.
- **Komponen**: Header, Summarizer, History — reusable & terpisah.
- **State & Effect**:

  - `useState` untuk `inputText`, `summary`, `history`.
  - `useEffect` load riwayat dari `localStorage`.

- **Props**: Kirim data & handler dari `App` ke anak.
- **Virtual DOM**: React diffing → update minimal ke DOM nyata → performa optimal.
- **Tailwind via Vite**:

  - `npm install tailwindcss @tailwindcss/vite`
  - plugin di `vite.config.js`, `@import "tailwindcss"` di `index.css`.

- **VS Code Snippets**: ES7+ React/Redux/React-Native snippets by dsznajder.

---

## Modul 5: Integrasi API dengan Model AI & Deployment

Peserta belajar integrasi OpenRouter API untuk memanggil model AI (DeepSeek, Google, Meta), handling async/await, error handling, markdown parsing, dan deployment ke Vercel.

**Poin-Poin Penting:**

- **Async vs Sync**:

  - Fungsi JS default synchronous → API calls asynchronous via Promises & `async/await`.
  - Penanganan error dengan `try/catch/finally`.

- **.env**: Simpan `VITE_OPENROUTER_API_KEY` rahasia, akses via `import.meta.env`.
- **OpenRouter**: Aggregator banyak model AI → signup, generate API key, pilih model (`deepseek-chat`, `gemini-pro`, `meta-llama`).
- **Fetch Ringkasan**:

  - `fetch("https://openrouter.ai/api/v1/chat/completions", { … })`
  - Kirim `model`, `messages`; parsing response JSON → `data.choices[0].message.content`.

- **Loading & Markdown**:

  - UI loading spinner (`animate-spin`) selama menunggu.
  - `npm install react-markdown` → `<ReactMarkdown>{summary}</ReactMarkdown>`.

- **Deployment ke Vercel**:

  - Push ke GitHub → import project di Vercel → setting build (`npm run build`) & env vars → deploy.
  - Continuous Deployment otomatis tiap push.

---

## 2️⃣ Kendala Umum & Solusi

| Kendala                | Deskripsi Singkat                                     | Cara Mengatasi                                           |
| ---------------------- | ----------------------------------------------------- | -------------------------------------------------------- |
| Setting `.env` di Vite | Variabel `VITE_` tidak terbaca di runtime             | Pastikan nama file `.env` dan prefix `VITE_` tepat       |
| State tidak ter-update | Komponen tidak re-render setelah setState             | Pastikan menggunakan fungsi setter, hindari mutasi array |
| Layout responsif pecah | Tampilan berubah tak terduga di ukuran layar tertentu | Tambah breakpoint Tailwind (`sm:`, `md:`, `lg:`)         |
| Deploy build gagal     | Build Vite error saat deploy                          | Periksa `build` script, `dist` folder, dan config Vercel |

---

## 3️⃣ Sesi Q\&A

1. **Bagaimana menghubungkan custom hook di React?**
   _Diskusi:_ Membuat `useFetch` untuk reusability.

2. **Kenapa error "Hydration mismatch" di Vite/React?**
   _Diskusi:_ Pastikan rendering serupa antara server/client, hindari kode non-deterministik.

3. **Bagaimana pengecekan API Key valid di frontend?**
   _Diskusi:_ Simpan di `.env`, validasi di backend/proxy, tampilkan pesan error ramah pengguna.

4. **Strategi optimasi performa React?**
   _Diskusi:_ Lazy loading komponen, `React.memo`, splitting bundle.

5. **Cara undo localStorage history?**
   _Diskusi:_ Tambahkan tombol "Clear All", gunakan fallback.

6. **Skill lanjutan untuk portfolio?**
   _Diskusi:_ Integrasi Next.js, autentikasi, database, testing.

---

🚀 **Selesai!** Terus kembangkan proyek dan share pengalamanmu!
