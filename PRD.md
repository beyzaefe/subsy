# PRD — Subsy: Abonelik Dedektifi (MVP)

**Version:** 1.0  
**Date:** 2026-06-16

---

## 1. Özet

Subsy, kullanıcıların aboneliklerini takip ettiği minimal bir web uygulamasıdır. MVP dört ana özellik içerir:

1. **Welcome** — Giriş noktası; kullanıcıyı Kayıt Ol veya Giriş Yap ekranına yönlendirir
2. **Supabase Auth** — E-posta ve şifre ile Giriş (Sign In) ve Kayıt Ol (Sign Up)
3. **Abonelik Yönetimi** — Abonelik ekleme, listeleme ve silme
4. **Dashboard** — Aylık toplam harcama (TRY) ve kategori pasta grafiği (Donut Chart)

**Stack:** React, Tailwind CSS, Supabase, Recharts  
**Tema:** Light Mode — `bg-slate-50` arka plan, beyaz kartlar, indigo mavi butonlar

---

## 2. MVP Kapsamı

### 2.1 Welcome (Giriş Noktası)

Uygulamanın kök rotası (`/`). Oturum açmamış kullanıcılar buradan auth akışına girer.

- **Subsy** markası, logo ve kısa tagline gösterilir
- **Ücretsiz Başla** (primary) → `/sign-up` (Kayıt Ol)
- **Giriş Yap** (secondary) → `/sign-in` (Giriş)
- MVP'de yalnızca mevcut özellikleri anlatan kısa bilgi maddeleri (abonelik takibi, aylık toplam, kategori görünümü)
- **MVP dışı:** hatırlatıcı, tasarruf önerisi veya arama/filtre vaatleri Welcome'da yer almaz

Oturum açmış kullanıcı `/` adresine gelirse doğrudan dashboard'a yönlendirilir.

### 2.2 Kimlik Doğrulama

**Sign Up (Kayıt Ol):**
- Kullanıcı **Ad Soyad**, **E-posta** ve **Şifre** bilgilerini girer
- Ad Soyad Supabase user metadata'sında saklanır
- Supabase Auth ile kayıt tamamlanır

**Sign In (Giriş):**
- E-posta + şifre ile giriş
- Başarılı girişte dashboard'a yönlendirme

**Ortak Kurallar:**
- Giriş yapmamış kullanıcı dashboard'a erişemez
- Auth sayfalarından **Geri** ile Welcome'a dönülebilir

### 2.3 Abonelik Yönetimi

Kullanıcı aşağıdaki alanlarla abonelik **ekler**, **listeler** ve **siler**.

| Alan | Açıklama |
|------|----------|
| Abonelik Adı | Metin (örn. Netflix) |
| Fiyat | Pozitif sayı |
| Döviz Cinsi | `TRY`, `USD` veya `EUR` |
| Kategori | Önceden tanımlı liste |
| Ödeme Tarihi | Tarih alanı |

**MVP dışı:** Abonelik düzenleme/güncelleme yoktur.

### 2.4 Dashboard

Tek ana ekran:

- Tüm abonelikler sabit veya anlık kurla **TRY**'ye çevrilir
- **Aylık Toplam Harcama** (TRY) gösterilir
- Kategorilere göre **Donut Chart** (Recharts) gösterilir
- Abonelik listesi aynı ekranda yer alır

### 2.5 Döviz Kuru

- Ana para birimi: **TRY**
- USD ve EUR tutarları uygulama açılışında sabit tanımlı veya basit bir API/mock ile TRY'ye çevrilir
- MVP'de gelişmiş kur yönetimi, cache veya geçmiş kur takibi yoktur

### 2.6 Kategoriler

`Eğlence` · `Müzik` · `Yazılım` · `Verimlilik` · `Eğitim` · `Diğer`

---

## 3. MVP Dışı (Şimdilik Eklenmeyecek)

- Abonelik düzenleme
- Profil yönetimi
- Çıkış yapma dışında gelişmiş oturum ekranları
- Banka entegrasyonu, bildirimler, dark mode
- Mobil uygulama, takım paylaşımı, otomatik abonelik tespiti
- Yaklaşan ödeme hatırlatıcıları, detaylı analitik, filtreleme/arama

---

## 4. Tasarım Sistemi

| Token | Değer | Kullanım |
|-------|-------|----------|
| Arka plan | `bg-slate-50` (#F8FAFC) | Sayfa |
| Kart | `bg-white shadow-sm rounded-xl` | Panel ve liste kartları |
| Primary | `bg-indigo-600` (#4F46E5) | Butonlar |
| Başlık | `text-slate-900` | Ana metin |
| İkincil | `text-slate-500` | Yardımcı metin |

---

## 5. Tech Stack

| Katman | Teknoloji |
|--------|-----------|
| Frontend | React + Vite |
| Styling | Tailwind CSS |
| Charts | Recharts |
| Auth & DB | Supabase |

---

## 6. Veritabanı Şeması

```sql
CREATE TABLE subscriptions (
  id            uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id       uuid NOT NULL REFERENCES auth.users(id) ON DELETE CASCADE,
  name          text NOT NULL,
  price         numeric(10,2) NOT NULL CHECK (price > 0),
  currency      text NOT NULL CHECK (currency IN ('TRY', 'USD', 'EUR')),
  category      text NOT NULL,
  payment_date  date NOT NULL,
  created_at    timestamptz NOT NULL DEFAULT now()
);

ALTER TABLE subscriptions ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users manage own subscriptions"
  ON subscriptions FOR ALL
  USING (auth.uid() = user_id)
  WITH CHECK (auth.uid() = user_id);
```

---

## 7. Kur Dönüşüm Mantığı

```js
function toTRY(price, currency, rates) {
  if (currency === 'TRY') return price;
  return price * rates[currency]; // örn. { USD: 34.5, EUR: 37.2 }
}
```

Dashboard toplamı: tüm aboneliklerin TRY karşılıklarının toplamı.
