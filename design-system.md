# Design System — Jelajah Jawa Timur

Diekstrak dari implementasi hero banner (`index.html`). Jadikan kontrak tunggal untuk semua section berikutnya agar navigasi, tipografi, dan interaksinya terasa satu karya. Kalau ada kebutuhan yang tidak tercover di sini, turunkan pola yang ada (jangan mengimpor gaya baru) lalu tambahkan hasilnya ke file ini.

## 1. Karakter visual

- **Dark editorial**: base hitam pekat, foto besar full-bleed sebagai bintang utama, teks putih berkema.
- **Glassmorphism tipis**: layer kaca semi-transparan + blur kecil untuk semua elemen yang menimpa foto (badge, tombol, label kartu). Kaca selalu `blur-[2px]–[3px]`, jangan lebih — biar foto tetap tajam terbaca.
- **Aksen serif italic**: Playfair Display khusus judul display & nama destinasi; semua UI lain Poppins. Jangan memasang italic di UI biasa.
- **Bukan warna-warni**: satu-satunya "warna" adalah foto itu sendiri dan gradasi hitam. Tak ada accent color selain putih.

## 2. Fondasi

- Single file `index.html`, Tailwind v4 via CDN (`@tailwindcss/browser@4`), JS vanilla inline. Tanpa berkas CSS sendiri, style block, atau inline style — selain class Tailwind di atribut `class`.
- Font via Google Fonts (`Poppins` 300–600, `Playfair Display` italic 500/600 + upright 500/600, `Baloo 2` 600–700 untuk wordmark brand saja).
- Favicon: inline SVG data URI di `<head>` (kotak `#171717` rx 16 + mark putih) — sampai tetap berupa satu file HTML tanpa berkas asset tambahan.
- `alt` bahasa Indonesia deskriptif; sumber foto dari Wikimedia Commons (cantumkan kredit di komentar HTML).

## 3. Design tokens

### Warna

| Token | Kelas | Pakai untuk |
|---|---|---|
| Base | `bg-neutral-950` | background halaman |
| Teks utama | `text-white` | judul, label aktif |
| Teks rand-1 | `text-white/95` | deskripsi hero |
| Teks rand-2 | `text-white/90` | deskripsi, label |
| Teks rand-3 | `text-white/80` | nav item non-aktif, lokasi |
| Teks rand-4 | `text-white/70` | label non-aktif |
| Teks muted | `text-white/50·/40` | dot non-aktif |
| Kaca gelap | `bg-black/30` + `backdrop-blur-[3px]` | badge |
| Kaca gelap-2 | `bg-black/35` + `backdrop-blur-[2px]` | pill label card saat aktif |
| Kaca terang | `bg-white/15` + `backdrop-blur-[3px]` | tombol CTA |
| Border kaca | `border-white/25` (badge, label aktif) atau `border-white/20` (tombol) | outline elemen kaca |
| Chip solid | `bg-neutral-950 text-white` | chip "Baru" di dalam badge |
| Panel kartu | `bg-neutral-900` tanpa border | badan kartu paket & panel form di atas base |

### Tipografi (role → recipe)

| Role | Recipe |
|---|---|
| Logo wordmark | `text-[19px] font-semibold [font-family:'Baloo_2',sans-serif]` (Baloo 2, khusus brand) |
| Nav item | `text-[13px]` → `lg:text-[15px]`; aktif `font-medium text-white` + `aria-current="page"`; lain `text-white/80 hover:text-white transition-colors` |
| Display H1 | Poppins × Playfair: `font-medium italic [font-family:'Playfair_Display',serif] text-white`, `text-[28px] leading-[1.12] sm:text-[42px] sm:leading-[1.08]` |
| Body hero | `text-[14px] leading-[22px] sm:text-[17px] sm:leading-[27px] text-white/95` |
| Badge chip | `text-[14px] font-medium text-white` |
| Badge label | `text-[15px] text-white/95` |
| Nama destinasi | `text-[17px] italic [font-family:'Playfair_Display',serif]` |
| Lok destinasi | `text-[14px] text-white/70` (aktif `/90`) |
| Tombol CTA | `text-[15px] leading-[20px] font-medium text-white` |
| Metrik stat | nilai `text-[26px] font-medium tracking-[-0.01em] text-white` + label `text-[12px] text-white/70` |
| Label field form | `text-[12px] text-white/70`; input `text-[13px] leading-[18px]` + `placeholder:text-white/50` |

### Radius

- Pill penuh: badge, tombol, chip, dot, hover-target nav → `rounded-full`.
- Kartu label destinasi → `rounded-2xl`.
- Kartu media besar (bento tile) → `rounded-3xl` (24px).
- Foto destinasi hero-kluster → lingkaran (`rounded-full` pada `li` & `img`).

### Spasi & layout halaman

- Gutter halaman: `px-[29px]` mobile → `sm:px-[52px]`. Semua section dimulai dengan gutter sama; konten kiri hero duduk di `left-[29px] sm:left-[52px]`.
- Container konten section: `mx-auto w-full max-w-[1200px]`.
- Pola section: pad vertikal `pt-[84px] pb-[88px]` → `sm:pt-[104px] sm:pb-[112px]`; header section → gap ke grid `mt-[30px]`.
- Grid bento: `grid auto-rows-[220px] sm:auto-rows-[240px] lg:auto-rows-[280px] gap-[12px]`, `grid-cols-1 sm:grid-cols-2 lg:grid-cols-4`; tile unggulan `sm:col-span-2 lg:row-span-2`.
- Grid paket: `grid grid-cols-1 gap-[12px] sm:grid-cols-2 lg:grid-cols-3`; head foto kartu tinggi tetap `h-[300px]`.
- Split layout (Tentang/Kontak): `grid items-center(items-start) gap-[36px] lg:grid-cols-[1fr_440px|480px]`.
- Footer: `border-t border-white/10`, konten 3 kelompok di `sm:justify-between`, `py-[40px]`.
- Nav: pill mengambang `fixed inset-x-0 top-0 z-50` (margin `px/pt-[12px]`, konten `max-w-[1240px] mx-auto`); section target anchor memakai `scroll-mt-[100px]`. Jangan pakai `sticky`: header sticky menyisakan slot tinggi sendiri di alur → muncul band hitam di atas hero. Detail di §5.
- Unit `svh` boleh dipakai untuk elemen visual yang harus menskala mengikuti tinggi viewport (pola kluster destinasi: container `h-[85svh]`, item `size-[14.25svh]–[18svh]`).

### Shadow / elevation

- Resting tanpa shadow; kedalaman datang dari gradasi foto.
- Karen kaca: `backdrop-blur-[3px]`.
- Karen selected (lingkaran destinasi): `shadow-[0_0_28px_rgba(255,255,255,0.22),0_10px_18px_rgba(0,0,0,0.45)]` — glow putih + drop shadow gelap, selalu berpasangan.

### Motion

- Hover kecil (scale, color): `duration-200` ~ `duration-300`.
- Fade teks/pill/warna: `duration-300`.
- Crossfade foto background: `duration-500` + sambungkan dengan `img.decode()` supaya fade-in hanya jalan setelah file beneran termuat (lihat pola JS).
- Tidak ada animasi looping/dekoratif.

## 4. Overlay foto (kunci wajib di semua foto besar)

Kode hero selalu pakai dua lapisan ini di atas foto:

```html
<img src="..." alt="" class="absolute inset-0 -z-20 h-full w-full object-cover saturate-[0.8] transition-opacity duration-500">
<div class="absolute inset-0 -z-10 bg-gradient-to-r from-black/60 via-black/25 to-black/5"></div>
<div class="absolute inset-0 -z-10 bg-gradient-to-t from-black/35 via-black/5 to-black/30"></div>
```

- Foto hero: `object-cover saturate-[0.8]`, versi 1920px.
- Overlay membaca teks putih tetap keterbacaan di semua area; jangan pakai putih polos tanpa overlay.

## 5. Komponen

### Badge ("Baru" + konteks)

```html
<div data-ui="badge" class="inline-flex items-center gap-x-[10px] rounded-full border border-white/25 bg-black/30 py-[5px] pl-[5px] pr-[18px] backdrop-blur-[3px]">
  <span class="rounded-full bg-neutral-950 px-[14px] py-[8px] text-[14px] font-medium text-white">Baru</span>
  <span class="text-[15px] text-white/95">Jelajah Jawa Timur</span>
</div>
```

### Tombol CTA kaca (pill, ikon panah)

```html
<a href="#..." class="inline-flex items-center gap-x-[10px] rounded-full border border-white/20 bg-white/15 py-[12px] pl-[26px] pr-[22px] text-[15px] leading-[20px] font-medium text-white backdrop-blur-[3px] transition-colors hover:bg-white/25 focus-visible:ring-2 focus-visible:ring-white">
  Lihat 5 Destinasi
  <svg viewBox="0 0 24 24" class="size-[15px]" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true" focusable="false">
    <path d="M7 17 17 7M9 7h8v8"/>
  </svg>
</a>
```

Varian: label/ikon diganti sesuai aksi; jangan buat tuil tanpa ikon — semua CTA memakai pill kaca ini.

### Logo brand (mark + wordmark)

Mark = huruf "t" minimalis (batang + crossbar + titik titik awal rute) yang ujung atasnya berbelok kanan menjadi panah arah. Inline SVG `stroke-width="3.4"` dengan ujung/penyambung `round`, warna via `currentColor`. Wordmark: **Baloo 2** 600, putih.

```html
<a href="#beranda" class="inline-flex items-center gap-x-[10px] rounded text-white focus-visible:ring-2 focus-visible:ring-white">
  <svg viewBox="0 0 24 24" class="size-[26px]" aria-hidden="true" focusable="false">
    <g fill="none" stroke="currentColor" stroke-width="3.4" stroke-linecap="round" stroke-linejoin="round">
      <path d="M12.2 21V8.4C12.2 6.5 13.4 5.4 15.9 5.4"/>
      <path d="M5.6 10.2H16.6"/>
      <path d="M15.9 5.4L13.5 3.4M15.9 5.4L13.5 7.4"/>
    </g>
    <circle cx="12.2" cy="21" r="2.4" fill="currentColor"/>
  </svg>
  <span class="text-[19px] font-semibold [font-family:'Baloo_2',sans-serif]">Tur Palapa</span>
</a>
```

- Nama payung sama dipakai di hero badge ("Jelajah Jawa Timur" = deskriptor program/segmen, bukan brand).
- Jangan pakai Baloo untuk konten/heading selain wordmark brand.

### Navbar pill sticky

Satu permukaan kaca BARU: kaca navbar lebih tebal blur-nya dari kaca kartu (`bg-white/10 border-white/15 backdrop-blur-[12px]`) supaya konten di bawahnya benar-benar nembus halus saat scroll. Bentuk `rounded-full`, isi: logo kiri + menu tengah (`justify-between`, menu `hidden sm:flex`) + CTA solid kanan; header `fixed inset-x-0 top-0 z-50` di level `<main>` (bukan absolute dan bukan sticky: fixed keluar dari alur, hero tetap membentang dari tepi atas viewport di belakang pill), dengan `px/pt-[12px]` pengapit pill.

```html
<header data-ui="navbar" class="fixed inset-x-0 top-0 z-50 px-[12px] pt-[12px]">
  <div class="mx-auto flex w-full max-w-[1240px] items-center justify-between gap-x-[18px] rounded-full border border-white/15 bg-white/10 py-[10px] pl-[24px] pr-[10px] backdrop-blur-[12px]">
    <!-- logo brand (§ Logo brand) -->
    <!-- nav menu tengah: link recipe sama -->
    <a href="#destinasi" class="inline-flex shrink-0 items-center gap-x-[9px] rounded-full bg-neutral-950 py-[12px] pl-[24px] pr-[20px] text-[14px] leading-[18px] font-medium text-white transition-colors hover:bg-neutral-800 focus-visible:ring-2 focus-visible:ring-white">
      Mulai Jelajah
      <svg viewBox="0 0 24 24" class="size-[14px]" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true" focusable="false"><path d="M7 17 17 7M9 7h8v8"/></svg>
    </a>
  </div>
</header>
```

- CTA navbar = varian baru tombol: SOLID `bg-neutral-950` (bukan kaca) dengan `hover:bg-neutral-800`; ikon panah 14px serupa CTA lain.
- Padding-x tombol memindahkan tepi pill: pill punya `pr-[10px]` agar tombol "menempel" tepi kanan dengan ruang napas (seperti referensi).
- **Scrollspy**: link menu tidak memakai styling aktif yang di-hardcode; aktif dikontrol keadaan `aria-current="page"` dari JS (`syncNav()`, threshold `scrollY + 140`) dan diposisikan lewat varian utilitas `aria-[current=page]:font-medium aria-[current=page]:text-white` pada tiap link. Satu sumber kebenaran: CSS membaca atribut, JS hanya meng-toggle atribut — pola sama dengan `data-active`.

### Header section (pola untuk semua section)

Badge komponen §5 di atas judul italic level-2, lalu satu paragraf lede — dimensi turun dari display H1 (paling besar setelah hero):

```html
<header data-ui="section-header" class="max-w-[720px]">
  <div data-ui="badge" class="inline-flex items-center gap-x-[10px] rounded-full border border-white/25 bg-black/30 py-[5px] pl-[5px] pr-[18px] backdrop-blur-[3px]">
    <span class="rounded-full bg-neutral-950 px-[14px] py-[8px] text-[14px] font-medium text-white">5 Destinasi</span>
    <span class="text-[15px] text-white/95">Jelajah Jawa Timur</span>
  </div>
  <h2 id="..." class="mt-[18px] text-[30px] font-medium italic leading-[1.08] text-white sm:text-[38px] [font-family:'Playfair_Display',serif]">Judul Section</h2>
  <p class="mt-[12px] max-w-[560px] text-[14px] leading-[22px] text-white/90 sm:text-[16px] sm:leading-[26px]">…</p>
</header>
```

### Kartu media bento (destination tile)

Foto memenuhi tile `rounded-3xl` + overlay gradasi bawah tunggal (varian tile dari overlay §4) + kartu info kaca menempel di bawah. Hover = zoom foto pelan (`duration-500`); tanpa state terpilih.

```html
<figure data-ui="destination-tile" class="group relative overflow-hidden rounded-3xl">
  <img src="…" alt="…" class="absolute inset-0 h-full w-full object-cover saturate-[0.8] transition-transform duration-500 group-hover:scale-[1.03]">
  <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/25 to-black/5"></div>
  <figcaption class="absolute inset-x-0 bottom-0 p-[12px] sm:p-[14px]">
    <div class="rounded-2xl border border-white/25 bg-black/35 px-[14px] py-[11px] backdrop-blur-[2px]">
      <h3 class="text-[19px] font-medium italic leading-[1.15] text-white [font-family:'Playfair_Display',serif]">Nama</h3>
      <p class="mt-[2px] text-[13px] text-white/70">Lokasi</p>
      <p class="mt-[6px] text-[12px] leading-[17px] text-white/90 line-clamp-2">Deskripsi singkat…</p>
    </div>
  </figcaption>
</figure>
```

- Tile unggulan: `sm:col-span-2 lg:row-span-2`, kapsi `text-[22px]→[26px]`, deskripsi tanpa `line-clamp` + `max-w-[420px]`.
- Thumbnail: 1280px untuk tile kecil, 1920px untuk tile unggulan.

### Kartu paket

Satu-satunya panel solid: `rounded-3xl bg-neutral-900`. Head foto `h-[300px]` memuat chip (durasi/level) di pojok kiri atas; kartu kaca nama & lokasi di badan kartu, lalu ringkasan → daftar include → bar harga + CTA kecil (footer `mt-auto`).

```html
<article data-ui="package-card" class="group flex flex-col overflow-hidden rounded-3xl bg-neutral-900">
  <div class="relative h-[300px] w-full shrink-0 overflow-hidden">
    <!-- img object-cover saturate-[0.8] (hover scale[1.03] 500ms) + overlay tile -->
    <div class="absolute left-0 top-0 flex flex-wrap gap-x-[8px] gap-y-[6px] p-[12px]">
      <!-- chip kecil (+backdrop-blur-[2px] karena di atas foto): -->
      <span class="rounded-full border border-white/20 bg-white/10 px-[10px] py-[3px] text-[11px] leading-[15px] text-white/85 backdrop-blur-[2px]">1D1N</span>
    </div>
  </div>
  <div class="flex grow flex-col p-[18px] pt-[16px]">
    <div class="rounded-2xl border border-white/25 bg-black/35 px-[14px] py-[11px] backdrop-blur-[2px]">
      <h3 class="text-[19px] font-medium italic leading-[1.15] text-white [font-family:'Playfair_Display',serif]">Nama paket…</h3>
      <p class="mt-[2px] text-[13px] text-white/70">Lokasi, Jawa Timur</p>
    </div>
    <p class="mt-[12px] text-[13px] leading-[19px] text-white/90">Ringkasan tempo…</p>
    <ul class="mt-[12px] flex flex-col gap-y-[7px]">
      <li class="flex items-center gap-x-[8px]">
        <svg viewBox="0 0 24 24" class="size-[14px] shrink-0 text-white/85" fill="none" aria-hidden="true" focusable="false"><path d="M20 6 9 17l-5-5" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
        <span class="text-[13px] leading-[18px] text-white/85">Item include…</span>
      </li>
    </ul>
    <div class="mt-auto flex items-end justify-between gap-x-[12px] pt-[16px]">
      <p class="text-[11px] leading-[15px] text-white/60">mulai dari<br/><span class="text-[18px] font-medium text-white">Rp 850.000</span> <span class="text-[11px] text-white/60">/ pribadi</span></p>
      <!-- CTA kecil = varian pill kaca -->
      <a href="#kontak" class="inline-flex items-center gap-x-[8px] rounded-full border border-white/20 bg-white/15 py-[9px] pl-[18px] pr-[16px] text-[13px] leading-[17px] font-medium text-white backdrop-blur-[3px] transition-colors hover:bg-white/25 focus-visible:ring-2 focus-visible:ring-white">Tanya Itinerary …</a>
    </div>
  </div>
</article>
```

- Kartu kaca nama di badan pakai recipe kaca gelap-2 (`border-white/25 bg-black/35 blur-[2px]`) — sama dengan kapsi tile; blur di atas panel flat tidak tampak, tapi pas kalau foto/panel diganti.
- Wrapper foto kartu **wajib `overflow-hidden` sendiri**: zoom hover harus terkunci di dalam head — tanpa itu img yang membesar ke-luwer di bawah foto (bug "light bar").
- CTA kecil: 13px/17px, panah `size-[13px]` — varian ukuran dari komponen tombol CTA utama.
- Target CTA `#kontak` (section kontak menyusul); sampai ada, klik tidak menyebabkan apa-apa melainkan hash URL berganti.

### Form kontak (panel) & field

Panel form = `rounded-3xl bg-neutral-900 p-[20px] sm:p-[24px]` + H3 italic 19px. Field: `label > span label > input`; input recipe:

```html
<input type="text|email" required placeholder="…"
  class="mt-[6px] w-full rounded-2xl border border-white/20 bg-white/10 px-[14px] py-[10px] text-[13px] leading-[18px] text-white placeholder:text-white/50 outline-none focus:border-white/60 focus:ring-2 focus:ring-white/70 transition-colors">
```

- Textarea: sama + `rows="4" resize-none`.
- Submit: tombol CTA utama (`py-[11px] pl-[22px] pr-[19px] text-[14px]`, ikon 14px) dibungkus `div aria-live="polite"`; state kirim: label → "Terkirim. Kami balas via email", `disabled="true" + disabled:opacity-70`, reset setelah 4s. Simulasi — TODO backend.
- State sukses tetap pill kaca putih — jangan mengimpor warna status (hijau/merah) baru.

### Footer

```html
<footer data-ui="footer" class="border-t border-white/10">
  <div class="mx-auto flex w-full max-w-[1200px] flex-col gap-y-[26px] px-[29px] py-[40px] sm:flex-row sm:items-center sm:justify-between sm:px-[52px] sm:gap-x-[40px]">
    <!-- brand: logo (28px ikon + 17px wordmark) + tagline 12px white/60 -->
    <!-- nav footer: link 13px white/80 hover:white, gap-x-[22px] -->
    <!-- meta 11px white/50: © + kredit foto Wikimedia (CC BY / CC BY-SA) -->
  </div>
</footer>
```

### Destination card (lingkaran + label kaca)

Struktur minimal (state kontrol lewat `data-active` pada `li.group`):

```html
<li data-item role="button" tabindex="0" data-active="false" aria-pressed="false"
    data-bg="…1920px" data-title="…" data-desc="…"
    class="group absolute … cursor-pointer rounded-full focus-visible:ring-2 focus-visible:ring-white">
  <img src="…960px" alt="…" class="size-full rounded-full object-cover transition-[transform,box-shadow] duration-300 group-hover:scale-105 group-data-[active=true]:ring-2 group-data-[active=true]:ring-white/70 group-data-[active=true]:shadow-[0_0_28px_rgba(255,255,255,0.22),0_10px_18px_rgba(0,0,0,0.45)]">
  <span class="absolute right-[calc(100%+9px)] top-1/2 w-max -translate-y-1/2 rounded-2xl border border-white/0 bg-white/0 px-[12px] py-[7px] text-left transition-colors duration-300 group-data-[active=true]:border-white/25 group-data-[active=true]:bg-black/35 group-data-[active=true]:backdrop-blur-[2px]">
    <span class="flex items-center gap-x-[7px]">
      <svg viewBox="0 0 24 24" class="size-[15px] shrink-0 text-white opacity-0 scale-75 transition-all duration-200 group-data-[active=true]:opacity-100 group-data-[active=true]:scale-100" fill="none" aria-hidden="true" focusable="false"><path d="M20 6 9 17l-5-5" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
      <span class="block text-[17px] italic text-white/90 transition-colors duration-300 group-data-[active=true]:text-white [font-family:'Playfair_Display',serif]">Nama</span>
    </span>
    <span class="mt-[3px] block text-[14px] text-white/70 transition-colors duration-300 group-data-[active=true]:text-white/90">Lokasi</span>
  </span>
</li>
```

State:
- **Idle**: label tampil redup (nama white/90, lokasi white/70), tanpa pinggir/pill.
- **Selected**: pill kaca gelap + centang pop + teks menyala + ring putih/70 + glow. Label tidak pernah menghilang.
- Area klik = lingkaran + label (keduanya di dalam `li`). Keyboard: Enter/Space sama dengan klik.
- Ruang centang di-*reserve* (opacity/scale saja) supaya teks tidak pernah bergeser saat toggle.

### Pagination dots

- Baris vertikal `flex flex-col gap-y-[7px]` di sisi kanan konten pilihan; tiap tombol hit area `size-[15px]` + `cursor-pointer`.
- Titik: idle `size-[6px] bg-white/50`; aktif `size-[10px] bg-white` dengan `transition-all`.
- Sinkron dengan state yang sama (`data-active` + `aria-pressed`).

### Pola state (wa via data-attribute)

- Satu sumber kebenaran per komponen interaktif: `data-active="true|false"` dibaca CSS via `group-data-[active=true]:…`.
- `aria-pressed` selalu disinkronkan bersama `data-active`.
- Konten yang bertukar secara dinamis dibungkus region `aria-live="polite"` (hero copy).
- JS: satu fungsi `setActive(index)` yang men-sync semua member + men-crossfade konten hero (foto via `decode()` dulu). Handler: `click` + `keydown` (Enter/Space).

## 6. Aksesibilitas (checklist penerimaan)

- Semua elemen interaktif punya `focus-visible:ring-2 focus-visible:ring-white`.
- Kotak klik/card: `role="button"` + `tabindex="0"` + handler Enter/Space.
- `aria-pressed` untuk toggle, `aria-current="page"` untuk nav aktif, `aria-label` pada kontrol ikon (dot).
- `aria-live="polite"` pada region yang berubah tanpa reload.
- Kontras: teks putih di atas foto hanya dengan overlay gradasi §4.

## 7. Jangan (anti-pattern)

- Tambah warna aksen / gradien warna selain hitam-putih + foto.
- Judul display tanpa italic Playsair, atau memakai italic di UI/label/isi.
- Kalau tidak butuh highlight saat aktif pada teks di atas foto, tetap ayuh.
- Elemen kaca tanpa blur; gambar non-hero tanpa saturate treatment yang seragam.
- Shadow "elevasi standar" (skala besar blur gelap) tanpa pendampingnya per §3.
- Animasi lebih lambat dari 500ms.
- Pertama beruntun ui: jangan buat placeholder lorem — konten dulu beneran pakai bahasa Indonesia dengan copywriting yang mengajak.

## 8. Roadmap section selanjutnya (usulan urutan)

1. **Destinasi** — ✅ sudah dibangun (bento grid di `#destinasi`); anchor `id="destinasi"` sudah dipindah dari kluster hero, nav "Destinasi" + CTA "Lihat 5 Destinasi" sekarang menunjuk ke sini.
2. **Paket Wisata** — ✅ sudah dibangun (`#paket`, 3 kartu paket); nav "Paket Wisata" sudah aktif sebagai link sungguhan.
3. **Testimoni** — strip quote singkat sebagai pembangun kepercayaan (satu-satunya yang masih tersisa).
4. **Tentang Kami + Kontak/Footer** — ✅ sudah dibangun: split narasi + stats (`#tentang`), panel form kontak (`#kontak`, submit disimulasikan — TODO backend), footer nav mini + kredit foto.
