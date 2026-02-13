# Bedah Alur Serangan Lumma Stealer Menggunakan Cyber Kill Chain
Halo! Di sini saya mau sharing hasil analisis saya tentang salah satu malware yang lagi sering banget dibahas di forum cyber security, yaitu **Lumma Stealer**. Sebagai anak jurusan RPL, saya cukup penasaran gimana cara kerja 'Barang' ini sampai bisa menembus pertahanan sistem dengan cara yang cukup unik.

---

## Apa itu Lumma Stealer?
Singkatnya, Lumma Stealer (alias LummaC2 Stealer) adalah malware jenis *Information Stealer* yang pertama kali muncul pada tahun 2022 di sebuah forum berbahasa Rusia yang beroperasi dengan model Malware-as-a-Service (MaaS). Ditulis menggunakan bahasa C, oleh karena itu ukurannya cukup kecil tetapi sangat cepat saat mencuri informasi sensitif, seperti data login, informasi kartu kredit, data akun bank, dan bahkan informasi terkait kripto.

## Modus Baru: Fake CHAPTCHA (Social Engineering)
Yang membuat saya tertarik adalah cara penyebarannya. Mereka tidak menggunakan *exploit* yang rumit, tetapi menggunakan trik **Fake CHAPCTHA**.

Jadi, penyerang biasanya mengirim notifikasi atau komentar di repositori GitHub (bisa juga lewat media sosial, iklan atau software bajakan), seolah-olah memberitahu ada kerentanan keamanan (*security vulnerability*). Korban yang panik atau penasaran bakal didesak buat klik tautan mencurigakan yang sudah disiapkan.

Setelah klik link itu, korban nggak langsung kena malware. Mereka bakal dihadapkan sama halaman **CAPTCHA palsu** yang kelihatan sangat profesional. Di situ, korban diminta menyalin sebuah script dan menjalankannya lewat fungsi `WIN + R` (Run) dengan alasan "verifikasi keamanan".

Begitu script PowerShell tadi di *paste* dan dijalankan, saat itulah malware **Lumma Stealer** masuk dan menginfeksi sistem secara total.

---

## Analisis Menggunakan Framework Cyber Kill Chain

Saya mencoba melihat alur serangannnya dengan menggunakan framework **Cyber Kill Chain** agar lebih jelas step-by-step nya.


1. **Reconnaisance (Pengintaian):**
   Penyerang menyebar link lewat situs-situs download software bajakan atau iklan palsu (tidak hanya lewat repo github). Mereka mencari orang yang ingin dapat software/barang "gratisan" tapi nggak teliti.

2. **Weaponization (Persenjataan):**
   Penyerang sudah menyiapkan sebuah website dummy atau palsu yang tampilannya sangat mirip dengan website CloudFlare. Di balik tombol "Verify", mereka menyembunyikan script PowerShell yang sudah di acak (*obfuscated*) agar tidak terbaca oleh antivirus biasa.

<p align="center">
  <img src="https://cybelangel.com/wp-content/uploads/2025/10/lumma_stealer_infostealers.png" alt="Alur Serangan Lumma Stealer" width="600">
  <br>
  <em>Gambar: Fake CAPTCHA page in action. (Source: Microsoft)</em>
</p>

3. **Delivery (Pengiriman):**
   Begitu kita buka webnya, script tadi otomatis masuk ke *clipboard* kita. Lewat bantuan instruksi palsu di layar, penyerang "mengirim" perintah itu agar kita menjalankan sendiri secara manual.

4. **Exploitation (Eksploitasi):**
   Saat kita sudah menekan `Enter` di jendela Run Windows, pada saat itulah kita memberikan akses. PowerShell akan langsung menjalankan perintah untuk mendownload file `.exe` milik Lumma Stealer dari server penyerang.

5. **Installation (Instalasi):**
   Ketika sudah terinstall, malware tersebut akan langsung menyimpan file di folder yang jarang dicek atau digunakan oleh user, seperti `%AppData%`. Lalu dia akan membuat *startup* otomatis, jadi setiap kali laptop menyala, malwarenya akan langsung otomatis jalan.

6. **Command and Control (C2):**
   Lumma Stealer akan mengirim laporan ke server pusat (C2) penyerang lewat internet. Lumma akan menunggu perintah dari penyerang sambil mengirimkan info spesifik tentang spesifikasi device/sistem yang kita punya.

7. **Actions on Objectives (Eksekusi):**
   Tahap akhir. Semua password yang tersimpan di Chrome/Edge, cookies yang digunakan untuk login akun, sampai isi wallet crypto akan dikirim semua ke penyerang. Hasilnya, semua akun kita dapat dibajak dalam sekejap.

---

## kesimpulan & Cara Agar Tidak Terdampak
Dari analisis saya, sebagus apapun antivirus dan firewall kita, kalau kita masih asal "copy paste" perintah ke dalam terminal/Run (memberikan akses), ya tetap saja sistem kita akan bisa diakses oleh penyerang.

**Tips dari saya**
* Jangan mudah percaya dengan instruksi yang dirasa mencurigakan dari website apapun.
* Selalu cek URL dari websitenya.
* Selalu baca apapun yang diberikan dengan baik.

**Analisis Oleh:** Ahmad Ghifari
