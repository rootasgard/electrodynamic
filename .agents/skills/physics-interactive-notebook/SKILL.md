---
name: physics-interactive-notebook
description: "Pedoman dan template standar untuk membuat notebook Jupyter komputasi fisika interaktif (elektrodinamika, mekanika, dsb.) dengan penurunan matematis analitis, verifikasi simbolik SymPy, visualisasi numerik 2D publikasi, laboratorium 3D interaktif Three.js WebGL (light-mode, collapsible toolbar, responsif mobile antislop, dan estetika premium-frontend-ui), serta tabel rangkuman fisis."
---

# Physics Interactive Notebook Skill

Standar arsitektur dan gaya penulisan untuk menghasilkan notebook komputasi fisika interaktif tingkat pascasarjana/akademik premium, dimodelkan langsung dari arsitektur standar emas [course6.ipynb](file:///c:/Users/asgard/OneDrive/Laptop/MAGISTER%20FISIKA%20UNPAD/ELEKTRODINAMIKA/electrodynamic/course6.ipynb).

---

## 1. Filosofi & Anatomi Notebook

Setiap notebook fisika komputasi harus memiliki **6 pilar struktur terpadu**:

```
[Cell 0: Markdown] ──▶ 1. Metadata Akademik & Perumusan Masalah Fisis
[Cell 1: Code]     ──▶ Inisialisasi Environment, Konstanta Fisis, & Dependensi
[Cell 2: Markdown] ──▶ 2. Penurunan Matematis Analitis Langkah-demi-Langkah
[Cell 3: Code]     ──▶ Verifikasi Simbolik Formal Menggunakan SymPy
[Cell 4: Code]     ──▶ Visualisasi Numerik 2D Multi-Panel Kualitas Publikasi (Matplotlib)
[Cell 5: Markdown] ──▶ 3. Pengantar Proyeksi Ruang & Simulasi 3D
[Cell 6: Code]     ──▶ Laboratorium 3D Interaktif WebGL (Three.js + Collapsible Toolbar + Premium UI)
[Cell 7: Markdown] ──▶ 4. Rangkuman Formula, Tabel Karakteristik, & Pembahasan Fisis
```

---

## 2. Rincian Implementasi Tiap Bagian

### 2.1 Bagian 1: Header Akademik & Geometri Sistem (Cell 0)
- **Judul**: Spesifik dan komprehensif, mencakup fenomena utama dan metode aproksimasi.
- **Metadata**: Nama penulis, afiliasi akademik (misal Magister Fisika Universitas Padjadjaran), buku rujukan standar (misal David J. Griffiths, *Introduction to Electrodynamics* edisi ke-4).
- **Deskripsi Geometri**:
  - Definisi sistem koordinat kartesian/sferis/silinder yang dipakai.
  - Asumsi fisis dan batas limit validitas (misal aproksimasi jarak jauh $r \gg a, b$).
  - Perincian variabel dan parameter sumber vs medan.

---

### 2.2 Bagian 2: Environment & Konstanta Fisis (Cell 1)
Wajib memuat pustaka komputasi numerik, simbolik, dan visualisasi:
```python
import json
import numpy as np
import matplotlib.pyplot as plt
import sympy as sp
from IPython.display import display, HTML
from html import escape

# Konfigurasi konstanta fisis fundamental (SI)
mu_0 = 4 * np.pi * 1e-7      # Permeabilitas vakum (H/m atau T.m/A)
eps_0 = 8.8541878128e-12    # Permitivitas vakum (F/m)
c = 299792458               # Kecepatan cahaya (m/s)

# Konfigurasi gaya visual Matplotlib
plt.rcParams['font.family'] = 'sans-serif'
plt.rcParams['font.size'] = 10
plt.rcParams['axes.linewidth'] = 1.0
plt.rcParams['mathtext.fontset'] = 'cm'
```

---

### 2.3 Bagian 3: Penurunan Matematis Analitis (Cell 2)
- Penurunan formal langkah-demi-langkah dengan sitasi bab/persamaan buku acuan.
- Gunakan blok persamaan LaTeX rapi:
  $$\mathbf{B}(\mathbf{r}) = \frac{\mu_0}{4\pi r^3}\left[ 3(\mathbf{m}\cdot\hat{\mathbf{r}})\hat{\mathbf{r}} - \mathbf{m} \right]$$
- Berikan justifikasi fisis di setiap transformasi matematika (misal ekuatorial $\theta = \pi/2$, ortogonalitas, ekspansi multipol).

---

### 2.4 Bagian 4: Verifikasi Simbolik Formal (Cell 3)
Gunakan `sympy` untuk membuktikan hasil analitis secara independen:
```python
# Definisi simbol positif
a, b, r_sym, I_1, I_2, mu0 = sp.symbols('a b r I_1 I_2 mu_0', positive=True)

# 1. Definisi kuantitas sumber
m_1 = I_1 * sp.pi * a**2

# 2. Perhitungan medan dan fluks
B_1 = - (mu0 * m_1) / (4 * sp.pi * r_sym**3)
Phi_21 = sp.simplify(B_1 * (sp.pi * b**2))
M_21 = sp.simplify(sp.Abs(Phi_21) / I_1)

# 3. Tampilkan hasil simbolik
display(sp.Eq(sp.Symbol(r'B_{1,z}'), B_1))
display(sp.Eq(sp.Symbol(r'\Phi_{21}'), Phi_21))
display(sp.Eq(sp.Symbol('M'), M_21))
```

---

### 2.5 Bagian 5: Visualisasi Numerik 2D Publikasi (Cell 4)
- Gunakan subplot ganda atau komparatif (`plt.subplots(1, 2, figsize=(13, 5), dpi=120)`).
- Terapkan palet warna harmonis (`#0284c7` Azure, `#059669` Zamrud, `#d97706` Amber, `#dc2626` Crimson).
- Label sumbu lengkap dengan notasi matematika LaTeX dan satuan SI.
- Grid garis putus-putus halus (`alpha=0.4, linestyle='--'`).
- Anotasi titik kerja atau batas fisis penting.

---

### 2.6 Bagian 6: Laboratorium 3D Interaktif Three.js (Cell 6)

Bagian ini adalah mahakarya interaktif. Wajib mematuhi aturan berikut:

#### A. Desain Latar & Visual (Light Theme Standard)
- `scene.background = new THREE.Color(0xffffff);` (Latar putih bersih).
- `grid = new THREE.GridHelper(size, divisions, 0x94a3b8, 0xe2e8f0);` (Kisi abu-abu terang).
- Pencahayaan ganda: `AmbientLight(0xffffff, 0.95)` + `DirectionalLight(0xffffff, 1.15)` dari sudut atas.
- Warna objek 3D kontras tinggi:
  - Loop/Kawat 1: `#ea580c` (Oranye tembaga metalik).
  - Loop/Kawat 2: `#0284c7` (Biru tajam).
  - Garis Medan $B$: `#d97706` (Amber dengan opasitas garis target lebih pekat).
  - Vektor Dipol $\mathbf{m}$: `#059669` (Hijau zamrud).
  - Vektor Gaya/Interaksi: `#dc2626` (Merah).

#### B. Toolbar Buka/Tutup (Collapsible Toolbar)
Sediakan floating toolbar atas dengan:
- **Badge judul status WebGL**.
- **Tombol Toggle Konfigurasi Variabel**: Membuka/menutup panel slider parameter dengan dot indikator aktif.
- **Tombol Toggle Grafik & Metrik**: Membuka/menutup panel mini-chart dan pembacaan angka real-time.
- **Tombol Reset Kamera**: Mengembalikan orbit ke orientasi optimal.
- **Tombol Close (✕)** pada header setiap drawer.

#### C. Mobile Layout (`/antislop-layoutmobile`)
- Pada breakpoint mobile/tablet (`max-width: 768px`):
  - Panel konfigurasi berubah menjadi **bottom-sheet modal** dengan pill *sheet handle* untuk gestur jempol.
  - Ukuran target sentuh (touch targets) minimum **44 x 44 px**.
  - Jarak aman antar-tombol minimal 6-8 px.
  - Slider dengan thumb tebal (18px) dan track sentuh lebar.
  - `overflow-x: hidden` (tanpa kebocoran scroll horizontal).

#### D. Pembungkus Kartu Laboratorium (`/premium-frontend-ui`)
- Output dibungkus dalam kartu `.simulation-card-wrapper`:
  - **Header**: Ikon instrumen, judul laboratorium, subjudul teori, dan pill status hijau pulsing.
  - **Kontainer Iframe**: `height: clamp(560px, 75vh, 680px)` dengan border halus dan bayangan elevasi `box-shadow: 0 20px 45px -12px rgba(15, 23, 42, 0.08)`.
  - **Footer**: Pill hukum fisis (misal ketergantungan $r^{-3}, r^{-4}$) dan panduan kontrol mouse/touch (Orbit, Zoom, Pan).

#### E. Bebas SyntaxWarning (Kritikal Python 3.12+)
> [!IMPORTANT]
> Jangan pernah menulis simbol LaTeX dengan *single backslash* di dalam f-string `f"""..."""`!
> Gunakan double backslash:
> - Tulis `\\gg` untuk $\gg$
> - Tulis `\\propto` untuk $\propto$
> - Tulis `\\Phi` untuk $\Phi$
> - Tulis `\\theta` untuk $\theta$

---

### 2.7 Bagian 7: Rangkuman & Pembahasan Fisis (Cell 7)
Tutup notebook dengan:
1. **Tabel Ringkasan Parameter Fisis**: Kolom Parameter, Formula Analitis, Arah Vektor, dan Pangkat Skala Jarak ($r^{-n}$).
2. **Uji Batas Limit (Limiting Cases)**: Pembuktian bahwa saat $r \to \infty$, medan/fluks/gaya menuju nol.
3. **Resiprositas & Konsistensi Hukum Kekekalan**: Bukti kesimetrian $M_{12} = M_{21}$, hukum aksi-reaksi Newton $\mathbf{F}_{12} = -\mathbf{F}_{21}$, dan ketiadaan torsi netto jika sebidang.

---

## 3. Boilerplate Template Sel Three.js (Cell 6)

Gunakan template Python berikut untuk menghasilkan Cell 6:

```python
# Render Simulasi Tiga Dimensi Interaktif Three.js
threejs_code = r"""<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Simulasi Fisika 3D Interaktif</title>
  <style>
    :root {
      --bg: #ffffff;
      --surface: rgba(255, 255, 255, 0.90);
      --border: rgba(226, 232, 240, 0.85);
      --text: #0f172a;
      --text-secondary: #475569;
      --text-muted: #64748b;
      --primary: #0284c7;
      --shadow-lg: 0 20px 40px -12px rgba(15, 23, 42, 0.12);
      --ease-spring: cubic-bezier(0.16, 1, 0.3, 1);
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { width: 100%; height: 100%; overflow: hidden; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background: var(--bg); color: var(--text); }
    #canvas-container { position: relative; width: 100%; height: 100%; overflow: hidden; display: flex; }
    #viewport { width: 100%; height: 100%; display: block; outline: none; }

    /* Top Toolbar */
    .app-toolbar { position: absolute; top: 14px; left: 14px; right: 14px; display: flex; justify-content: space-between; pointer-events: none; z-index: 40; }
    .toolbar-brand { pointer-events: auto; display: flex; align-items: center; gap: 8px; padding: 7px 12px; background: var(--surface); backdrop-filter: blur(18px); border: 1px solid var(--border); border-radius: 9999px; }
    .brand-dot { width: 8px; height: 8px; border-radius: 50%; background: #10b981; }
    .toolbar-actions { pointer-events: auto; display: flex; gap: 6px; background: var(--surface); backdrop-filter: blur(18px); padding: 4px; border: 1px solid var(--border); border-radius: 9999px; }
    .toolbar-btn { display: inline-flex; align-items: center; gap: 6px; height: 36px; padding: 0 12px; border-radius: 9999px; font-size: 12px; font-weight: 600; color: var(--text-secondary); background: transparent; border: none; cursor: pointer; transition: all 0.2s; }
    .toolbar-btn.active { background: var(--primary); color: #fff; }
    .toolbar-btn.icon-only { width: 36px; padding: 0; justify-content: center; }

    /* Collapsible Drawers */
    .drawer { position: absolute; top: 66px; left: 14px; width: 330px; max-height: calc(100% - 80px); background: var(--surface); backdrop-filter: blur(20px); border: 1px solid var(--border); border-radius: 16px; box-shadow: var(--shadow-lg); z-index: 35; overflow: hidden; transition: transform 0.38s var(--ease-spring), opacity 0.25s; display: flex; flex-direction: column; }
    .drawer.is-collapsed { transform: translateX(calc(-100% - 24px)); opacity: 0; pointer-events: none; }
    .drawer-header { display: flex; justify-content: space-between; align-items: center; padding: 12px 16px; border-bottom: 1px solid var(--border); }
    .drawer-close { width: 32px; height: 32px; border: none; background: transparent; cursor: pointer; color: var(--text-muted); border-radius: 8px; }
    .drawer-body { padding: 14px 16px; overflow-y: auto; flex: 1; }

    /* Touch-Friendly Controls */
    .control-row { margin-bottom: 10px; }
    .control-label { display: flex; justify-content: space-between; font-size: 11.5px; color: var(--text-secondary); font-weight: 500; margin-bottom: 4px; }
    input[type=range] { width: 100%; height: 6px; accent-color: var(--primary); margin: 6px 0; cursor: pointer; }

    /* Mobile Bottom Sheet */
    @media (max-width: 768px) {
      .toolbar-btn .btn-text { display: none; }
      .drawer { top: auto; bottom: 0; left: 0; right: 0; width: 100%; max-height: 68vh; border-radius: 20px 20px 0 0; }
      .drawer.is-collapsed { transform: translateY(105%); }
    }
  </style>
  <script type="importmap">
    {
      "imports": {
        "three": "https://cdn.jsdelivr.net/npm/three@0.170.0/build/three.module.js",
        "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.170.0/examples/jsm/"
      }
    }
  </script>
</head>
<body>
  <div id="canvas-container">
    <canvas id="viewport"></canvas>
    
    <header class="app-toolbar">
      <div class="toolbar-brand">
        <div class="brand-dot"></div>
        <span style="font-size: 12px; font-weight: 700;">Simulasi 3D</span>
      </div>
      <nav class="toolbar-actions">
        <button id="btn-toggle-config" class="toolbar-btn active" type="button">
          <span>⚙️</span><span class="btn-text">Parameter</span>
        </button>
        <button id="btn-reset-view" class="toolbar-btn icon-only" type="button" title="Reset Kamera">🔄</button>
      </nav>
    </header>

    <aside id="config-drawer" class="drawer">
      <div class="drawer-header">
        <h3 style="font-size: 13px; font-weight: 700;">Konfigurasi Parameter</h3>
        <button id="btn-close-drawer" class="drawer-close" type="button">✕</button>
      </div>
      <div class="drawer-body">
        <!-- Input Sliders Di Sini -->
      </div>
    </aside>
  </div>

  <script type="module">
    import * as THREE from 'three';
    import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

    const canvas = document.querySelector('#viewport');
    const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.setSize(canvas.clientWidth, canvas.clientHeight, false);

    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xffffff);

    const camera = new THREE.PerspectiveCamera(45, canvas.clientWidth / canvas.clientHeight, 0.1, 100);
    camera.position.set(2.5, 7.5, 7.0);

    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;

    const ambientLight = new THREE.AmbientLight(0xffffff, 0.95);
    scene.add(ambientLight);
    const dirLight = new THREE.DirectionalLight(0xffffff, 1.15);
    dirLight.position.set(6, 12, 10);
    scene.add(dirLight);

    const grid = new THREE.GridHelper(16, 32, 0x94a3b8, 0xe2e8f0);
    scene.add(grid);

    // Toggle Drawer Wiring
    const configDrawer = document.querySelector('#config-drawer');
    const btnToggle = document.querySelector('#btn-toggle-config');
    let isOpen = true;
    function setDrawer(open) {
      isOpen = open;
      configDrawer.classList.toggle('is-collapsed', !open);
      btnToggle.classList.toggle('active', open);
    }
    btnToggle.addEventListener('click', () => setDrawer(!isOpen));
    document.querySelector('#btn-close-drawer').addEventListener('click', () => setDrawer(false));
    document.querySelector('#btn-reset-view').addEventListener('click', () => {
      camera.position.set(2.5, 7.5, 7.0);
      controls.target.set(0, 0, 0);
      controls.update();
    });

    window.addEventListener('resize', () => {
      camera.aspect = canvas.parentElement.clientWidth / canvas.parentElement.clientHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(canvas.parentElement.clientWidth, canvas.parentElement.clientHeight, false);
    });

    renderer.setAnimationLoop(() => {
      controls.update();
      renderer.render(scene, camera);
    });
  </script>
</body>
</html>
"""

# Bungkus output HTML dengan pembungkus standar antislop & premium-frontend-ui
rendered_card = f"""
<div class="simulation-card-wrapper" style="
  margin: 1.5rem 0;
  border-radius: 18px;
  background: #ffffff;
  border: 1px solid #e2e8f0;
  box-shadow: 0 20px 45px -12px rgba(15, 23, 42, 0.08), 0 2px 8px -1px rgba(15, 23, 42, 0.04);
  overflow: hidden;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
">
  <div style="display: flex; align-items: center; justify-content: space-between; padding: 12px 18px; background: #f8fafc; border-bottom: 1px solid #e2e8f0;">
    <div style="display: flex; align-items: center; gap: 10px;">
      <div style="width: 32px; height: 32px; border-radius: 8px; background: linear-gradient(135deg, #0284c7, #0ea5e9); display: flex; align-items: center; justify-content: center; color: #ffffff; font-size: 15px;">🔬</div>
      <div>
        <h3 style="margin: 0; font-size: 13.5px; font-weight: 700; color: #0f172a;">Simulasi 3D Interaktif</h3>
        <p style="margin: 2px 0 0; font-size: 11px; color: #64748b;">Three.js WebGL Engine • Real-Time Field Calculation</p>
      </div>
    </div>
    <span style="padding: 4px 10px; border-radius: 9999px; background: #ecfdf5; border: 1px solid #a7f3d0; color: #047857; font-size: 11px; font-weight: 600;">Aktif</span>
  </div>

  <div style="position: relative; width: 100%; height: clamp(560px, 75vh, 680px); background: #ffffff;">
    <iframe srcdoc="{escape(threejs_code, quote=True)}" style="width: 100%; height: 100%; border: none; display: block;" loading="lazy"></iframe>
  </div>

  <div style="padding: 10px 18px; background: #f8fafc; border-top: 1px solid #e2e8f0; font-size: 11px; color: #64748b;">
    Gunakan toolbar di atas kanvas untuk membuka/menutup panel konfigurasi parameter
  </div>
</div>
"""

display(HTML(rendered_card))
```

---

## 4. Checklist Kualitas Pra-Rilis

Sebelum menyelesaikan notebook baru, pastikan seluruh kriteria ini terpenuhi:

- [ ] **Alur Narasi Utuh**: Rumusan $\to$ Teori $\to$ SymPy $\to$ Matplotlib $\to$ Three.js 3D $\to$ Tabel Diskusi.
- [ ] **Kompilasi Python Bersih**: `compile(code)` pada semua sel menghasilkan **0 warning** (bebas `SyntaxWarning: invalid escape sequence`).
- [ ] **Latar Belakang Putih Konsisten**: Matplotlib dan Three.js keduanya berlatar putih bersih dengan kontras tinggi.
- [ ] **Toolbar Collapsible Berfungsi**: Tombol buka/tutup dapat menyembunyikan drawer sehingga kanvas 3D leluasa dieksplorasi.
- [ ] **Ergonomi Sentuh Mobile**: Slider dan tombol dapat dioperasikan dengan mudah pada layar sentuh ponsel tanpa overflow horizontal.
- [ ] **Validasi Ekspor HTML**: Perintah `python -m nbconvert --to html --no-input notebook.ipynb` berhasil menghasilkan file `.html` yang mandiri dan interaktif.

---

## 5. Pembaruan `index.html` Saat Notebook Baru Diterbitkan

Setiap kali notebook baru (`courseN.ipynb`) dibuat atau diterbitkan, `index.html` **wajib diperbarui** agar daftar modul tetap akurat. Ikuti alur ini secara berurutan.

> [!IMPORTANT]
> Jangan pernah mengarang judul, topik, atau deskripsi kursus. Semua konten kartu harus diambil langsung dari notebook. Ini adalah aturan **antislop R-38 (Real Content or Honest Placeholder)**.

---

### 5.1 Langkah Wajib: Ekstrak Judul dan Topik dari Notebook

Jalankan skrip Python berikut untuk membaca data asli dari semua notebook:

```python
import json, pathlib

base = pathlib.Path(r"c:\Users\asgard\OneDrive\Laptop\MAGISTER FISIKA UNPAD\ELEKTRODINAMIKA\electrodynamic")

for nb_path in sorted(base.glob("course*.ipynb")):
    with open(nb_path, encoding="utf-8") as f:
        nb = json.load(f)
    for cell in nb["cells"]:
        if cell["cell_type"] == "markdown":
            src = "".join(cell["source"])
            for line in src.splitlines():
                if line.startswith("# "):
                    print(f"{nb_path.stem}: {line.strip()}")
                    break
            else:
                continue
            break
```

Gunakan output skrip ini sebagai sumber kebenaran judul kartu. **Jangan tulis judul dari ingatan.**

---

### 5.2 Aturan Antislop yang Berlaku pada `index.html`

Seluruh revisi `index.html` harus mematuhi aturan berikut (referensi ke antislop core):

| Aturan | Penerapan pada `index.html` |
|---|---|
| **R-24 (Navigation)** | Setiap `<a href="...">` hanya boleh mengarah ke file `.html` yang benar-benar ada di direktori. |
| **R-26 (Interactive Elements)** | Tidak ada link mati. Kursus yang `.html`-nya belum diekspor harus memakai `<div class="card card-soon">` bukan `<a>`. |
| **R-38 (Real Content)** | Judul, topik, dan referensi bab Griffiths diambil persis dari notebook. Tidak boleh dikarang. |
| **R-03 (Mobile Responsiveness)** | Grid wajib tiga state reflow: 1 kolom (<560px), 2 kolom (560–900px), 3 kolom (>900px). |
| **R-17 (Data & Numbers)** | Tidak boleh ada angka statistik palsu (jumlah mahasiswa, rating, dsb.). |
| **R-07 (Background)** | Tidak ada grid background CSS (`background-image: linear-gradient` berulang sebagai latar repeating). |
| **R-05 (Layout)** | Tidak ada layout template AI generik. Struktur: header, banner konteks, grid kartu, footer. |

---

### 5.3 Pola Kartu untuk Notebook Baru (Tersedia)

Salin template kartu berikut dan ganti placeholder `{{...}}` dengan data dari notebook:

```html
<!-- Course N — file courseN.html SUDAH ADA -->
<a class="card rise dN" href="courseN.html"
   aria-label="Buka modul N: {{JUDUL_LENGKAP_DARI_NOTEBOOK}}">
  <div class="card-header">
    <div class="card-num">0N</div>
  </div>
  <div class="card-body">
    <p class="card-title">{{JUDUL_SINGKAT}}</p>
    <ul class="topic-list">
      <li>{{TOPIK_1_DARI_NOTEBOOK}}</li>
      <li>{{TOPIK_2_DARI_NOTEBOOK}}</li>
      <li>{{TOPIK_3_DARI_NOTEBOOK}}</li>
      <li>{{TOPIK_4_DARI_NOTEBOOK}}</li>
      <li>{{TOPIK_5_DARI_NOTEBOOK}}</li>
    </ul>
  </div>
  <div class="card-footer">
    <span class="ref-chip">Griffiths Ch. {{BAB}}</span>
    <span class="card-cta">Buka modul</span>
  </div>
</a>
```

---

### 5.4 Pola Kartu untuk Notebook yang Belum Diekspor ke HTML

Jika `courseN.ipynb` sudah ada tetapi `courseN.html` **belum** diekspor, gunakan `<div>` bukan `<a>` (antislop R-26):

```html
<!-- Course N — courseN.html BELUM ADA, gunakan div bukan <a> -->
<div class="card card-soon rise dN"
     aria-label="Modul N belum tersedia: sedang disiapkan"
     id="card-courseN">
  <div class="card-header">
    <div class="card-num">0N</div>
    <span class="soon-pill">Segera</span>
  </div>
  <div class="card-body">
    <p class="card-title">{{JUDUL_SINGKAT_DARI_NOTEBOOK}}</p>
    <ul class="topic-list">
      <li>{{TOPIK_1}}</li>
      <li>{{TOPIK_2}}</li>
      <li>{{TOPIK_3}}</li>
    </ul>
  </div>
  <div class="card-footer">
    <span class="ref-chip">Griffiths Ch. {{BAB}}</span>
    <span style="font-size:0.76rem;color:var(--muted-2);">HTML dalam persiapan</span>
  </div>
</div>
```

Setelah `nbconvert` berhasil, ganti `<div class="card card-soon ...">` menjadi `<a class="card ...">` dengan `href="courseN.html"`.

---

### 5.5 Cara Mengekspor Notebook ke HTML

```powershell
# Dari direktori proyek:
python -m nbconvert --to html --no-input courseN.ipynb
```

Setelah berhasil, perbarui kartu dari pola `card-soon` (`<div>`) menjadi pola aktif (`<a>`).

---

### 5.6 Checklist Update `index.html`

Sebelum menyimpan hasil revisi `index.html`, pastikan semua poin ini terpenuhi:

- [ ] **Judul kartu** diambil persis dari `# Heading` pertama di notebook (jalankan skrip §5.1).
- [ ] **Topik-topik** mencerminkan subjudul (`##`) utama notebook, bukan karangan.
- [ ] **Referensi bab Griffiths** sesuai dengan yang tertulis di metadata notebook (sel header).
- [ ] **Link `href`** hanya ada jika file `.html` sudah ada di direktori (cek dengan `dir course*.html`).
- [ ] **Kartu notebook-belum-html** menggunakan `<div class="card card-soon">`, bukan `<a>`.
- [ ] **Grid reflow** tetap tiga state: 1 col / 2 col / 3 col (jangan hapus media queries).
- [ ] **Tidak ada link mati** di navbar, footer, atau kartu (antislop R-24, R-26).
- [ ] **Tidak ada konten fabrikasi**: tidak ada statistik palsu, testimonial, atau klaim tanpa bukti.
- [ ] **Animasi stagger** kelas `.d1`–`.dN` diperbarui agar urutan delay sesuai jumlah kartu baru.
