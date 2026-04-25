# Laporan Praktikum 7: Upload File Gambar

Repositori ini berisi hasil praktikum mata kuliah **Pemrograman Web 2**. [cite_start]Fokus utama pada praktikum ini adalah implementasi fitur unggah (*upload*) file gambar pada sistem artikel menggunakan Framework CodeIgniter 4[cite: 3, 4, 5].

## Informasi Mahasiswa
* **Nama:** Fajar Fawwaz Atallah
* **NIM:** 312410357
* **Kelas:** TI.24.A4 / I241D
* **Dosen:** Agung Nugroho, S.Kom., M.Kom.

---

## Langkah-langkah Praktikum

### 1. Penyesuaian Controller Artikel
Melakukan modifikasi pada method `add()` di `Artikel.php` untuk menangani file yang diunggah dari form. [cite_start]Kode ini berfungsi untuk mengambil file menggunakan `getFile()`, memvalidasi data, dan memindahkan file ke direktori server[cite: 13, 14, 22, 23].

```php
public function add()
{
    // Validasi data
    $validation = \Config\Services::validation();
    $validation->setRules(['judul' => 'required']);
    $isDataValid = $validation->withRequest($this->request)->run();

    if ($isDataValid) {
        $file = $this->request->getFile('gambar');
        $file->move(ROOTPATH . 'public/gambar'); // Memindahkan file ke public/gambar

        $artikel = new ArtikelModel();
        $artikel->insert([
            'judul'  => $this->request->getPost('judul'),
            'isi'    => $this->request->getPost('isi'),
            'slug'   => url_title($this->request->getPost('judul')),
            'gambar' => $file->getName(), // Menyimpan nama file ke database
        ]);
        return redirect('admin/artikel');
    }
    $title = "Tambah Artikel";
    return view('artikel/form_add', compact('title'));
}
```

### 2. Modifikasi Form Tambah Artikel

Menambahkan atribut ``` enctype="multipart/form-data" ``` pada tag form agar form dapat mengirimkan data file. Selain itu, menambahkan input type ``` file ``` untuk memilih gambar.

```HTML
<form action="" method="post" enctype="multipart/form-data">
    <p>
        <input type="file" name="gambar">
    </p>
    <button type="submit" class="btn btn-primary">Kirim</button>
</form>
```

--- 

# HASIL PRAKTIKUM 

## A. Tampilan Form Tambah Artikel

### Menampilkan field "Telusuri" atau "Choose File" untuk mengunggah gambar ke dalam sistem.

<img width="1114" height="663" alt="image" src="https://github.com/user-attachments/assets/fb06b759-fe4c-42ec-99e0-95086051ed01" />

## B. Proses Upload Gambar

### Bukti bahwa file gambar berhasil dipilih dan dikirim melalui form.

<img width="1101" height="453" alt="image" src="https://github.com/user-attachments/assets/6ad6b53f-698b-41ae-8ba0-681b4cff3cfe" />
