<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "affcf199a44f60283a289dcb69dc144e",
  "translation_date": "2025-07-17T09:12:54+00:00",
  "source_file": "03-GettingStarted/02-client/complete_examples.md",
  "language_code": "ms"
}
-->
# Contoh Lengkap Klien MCP

Direktori ini mengandungi contoh lengkap dan berfungsi bagi klien MCP dalam pelbagai bahasa pengaturcaraan. Setiap klien menunjukkan fungsi penuh yang diterangkan dalam tutorial utama README.md.

## Klien Tersedia

### 1. Klien Java (`client_example_java.java`)
- **Pengangkutan**: SSE (Server-Sent Events) melalui HTTP
- **Pelayan Sasaran**: `http://localhost:8080`
- **Ciri-ciri**: 
  - Penubuhan sambungan dan ping
  - Senarai alat
  - Operasi kalkulator (tambah, tolak, darab, bahagi, bantuan)
  - Pengendalian ralat dan pengekstrakan keputusan

**Untuk menjalankan:**
```bash
# Ensure your MCP server is running on localhost:8080
javac client_example_java.java
java client_example_java
```

### 2. Klien C# (`client_example_csharp.cs`)
- **Pengangkutan**: Stdio (Input/Output Standard)
- **Pelayan Sasaran**: Pelayan MCP .NET tempatan melalui dotnet run
- **Ciri-ciri**:
  - Permulaan pelayan automatik melalui pengangkutan stdio
  - Senarai alat dan sumber
  - Operasi kalkulator
  - Penguraian keputusan JSON
  - Pengendalian ralat menyeluruh

**Untuk menjalankan:**
```bash
dotnet run
```

### 3. Klien TypeScript (`client_example_typescript.ts`)
- **Pengangkutan**: Stdio (Input/Output Standard)
- **Pelayan Sasaran**: Pelayan MCP Node.js tempatan
- **Ciri-ciri**:
  - Sokongan penuh protokol MCP
  - Operasi alat, sumber, dan prompt
  - Operasi kalkulator
  - Pembacaan sumber dan pelaksanaan prompt
  - Pengendalian ralat yang kukuh

**Untuk menjalankan:**
```bash
# First compile TypeScript (if needed)
npm run build

# Then run the client
npm run client
# or
node client_example_typescript.js
```

### 4. Klien Python (`client_example_python.py`)
- **Pengangkutan**: Stdio (Input/Output Standard)  
- **Pelayan Sasaran**: Pelayan MCP Python tempatan
- **Ciri-ciri**:
  - Corak async/await untuk operasi
  - Penemuan alat dan sumber
  - Ujian operasi kalkulator
  - Pembacaan kandungan sumber
  - Organisasi berasaskan kelas

**Untuk menjalankan:**
```bash
python client_example_python.py
```

## Ciri-ciri Umum Di Semua Klien

Setiap pelaksanaan klien menunjukkan:

1. **Pengurusan Sambungan**
   - Menubuhkan sambungan ke pelayan MCP
   - Mengendalikan ralat sambungan
   - Pembersihan dan pengurusan sumber yang betul

2. **Penemuan Pelayan**
   - Menyenaraikan alat yang tersedia
   - Menyenaraikan sumber yang tersedia (jika disokong)
   - Menyenaraikan prompt yang tersedia (jika disokong)

3. **Panggilan Alat**
   - Operasi kalkulator asas (tambah, tolak, darab, bahagi)
   - Arahan bantuan untuk maklumat pelayan
   - Pemindahan argumen dan pengendalian keputusan yang betul

4. **Pengendalian Ralat**
   - Ralat sambungan
   - Ralat pelaksanaan alat
   - Kegagalan yang teratur dan maklum balas kepada pengguna

5. **Pemprosesan Keputusan**
   - Mengekstrak kandungan teks dari respons
   - Memformat output untuk kebolehbacaan
   - Mengendalikan pelbagai format respons

## Prasyarat

Sebelum menjalankan klien ini, pastikan anda:

1. **Pelayan MCP yang sepadan sedang berjalan** (dari `../01-first-server/`)
2. **Kebergantungan yang diperlukan telah dipasang** untuk bahasa pilihan anda
3. **Sambungan rangkaian yang betul** (untuk pengangkutan berasaskan HTTP)

## Perbezaan Utama Antara Pelaksanaan

| Bahasa     | Pengangkutan | Permulaan Pelayan | Model Async | Perpustakaan Utama |
|------------|--------------|-------------------|-------------|--------------------|
| Java       | SSE/HTTP     | Luaran            | Sinkron     | WebFlux, MCP SDK   |
| C#         | Stdio        | Automatik         | Async/Await | .NET MCP SDK       |
| TypeScript | Stdio        | Automatik         | Async/Await | Node MCP SDK       |
| Python     | Stdio        | Automatik         | AsyncIO     | Python MCP SDK     |

## Langkah Seterusnya

Selepas meneroka contoh klien ini:

1. **Ubah suai klien** untuk menambah ciri atau operasi baru
2. **Bina pelayan anda sendiri** dan uji dengan klien ini
3. **Cuba pengangkutan berbeza** (SSE vs. Stdio)
4. **Bina aplikasi yang lebih kompleks** yang mengintegrasikan fungsi MCP

## Penyelesaian Masalah

### Isu Biasa

1. **Sambungan ditolak**: Pastikan pelayan MCP berjalan pada port/laluan yang dijangka
2. **Modul tidak ditemui**: Pasang MCP SDK yang diperlukan untuk bahasa anda
3. **Kebenaran ditolak**: Semak kebenaran fail untuk pengangkutan stdio
4. **Alat tidak ditemui**: Sahkan pelayan melaksanakan alat yang dijangka

### Petua Debug

1. **Aktifkan log terperinci** dalam MCP SDK anda
2. **Semak log pelayan** untuk mesej ralat
3. **Sahkan nama dan tandatangan alat** sepadan antara klien dan pelayan
4. **Uji dengan MCP Inspector** terlebih dahulu untuk mengesahkan fungsi pelayan

## Dokumentasi Berkaitan

- [Tutorial Klien Utama](./README.md)
- [Contoh Pelayan MCP](../../../../03-GettingStarted/01-first-server)
- [MCP dengan Integrasi LLM](../../../../03-GettingStarted/03-llm-client)
- [Dokumentasi Rasmi MCP](https://modelcontextprotocol.io/)

**Penafian**:  
Dokumen ini telah diterjemahkan menggunakan perkhidmatan terjemahan AI [Co-op Translator](https://github.com/Azure/co-op-translator). Walaupun kami berusaha untuk ketepatan, sila ambil perhatian bahawa terjemahan automatik mungkin mengandungi kesilapan atau ketidaktepatan. Dokumen asal dalam bahasa asalnya harus dianggap sebagai sumber yang sahih. Untuk maklumat penting, terjemahan profesional oleh manusia adalah disyorkan. Kami tidak bertanggungjawab atas sebarang salah faham atau salah tafsir yang timbul daripada penggunaan terjemahan ini.