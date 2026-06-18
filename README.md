# Subsy — Abonelik Dedektifi

Aboneliklerinizi TRY cinsinden takip etmenizi sağlayan minimal bir MVP.

---

## MVP Özellikleri

| Özellik | Açıklama |
|---------|----------|
| **Welcome** | Giriş noktası — Kayıt Ol veya Giriş Yap yönlendirmesi |
| **Auth** | Supabase ile e-posta/şifre Sign In ve Sign Up |
| **Abonelikler** | Ekle, listele, sil |
| **Dashboard** | Aylık toplam harcama (TRY) + kategori Donut Chart |

---

## Tech Stack

- React + Vite
- Tailwind CSS
- Supabase (Auth + PostgreSQL)
- Recharts

---

## Kurulum

### 1. Bağımlılıklar

```bash
npm install
```

### 2. Ortam değişkenleri

`.env.local` dosyası oluşturun:

```env
VITE_SUPABASE_URL=https://xxxx.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

Kur için sabit değerler kod içinde tanımlanabilir; isteğe bağlı olarak basit bir API entegrasyonu eklenebilir.

### 3. Veritabanı

Supabase SQL Editor'da [prd.md](prd.md) içindeki `subscriptions` şemasını çalıştırın.

### 4. Geliştirme

```bash
npm run dev
```

---

## Abonelik Alanları

| Alan | Tip |
|------|-----|
| Abonelik Adı | string |
| Fiyat | number |
| Döviz Cinsi | TRY / USD / EUR |
| Kategori | string (sabit liste) |
| Ödeme Tarihi | date |

---

## Proje Yapısı

```
src/
├── main.tsx                 # Entry — router provider, App mount
├── App.tsx                  # Route tanımları (yalnızca pages/ import eder)
├── pages/
│   ├── Welcome/
│   │   ├── WelcomePage.tsx
│   │   └── components/
│   │       └── WelcomeCard.tsx
│   ├── Auth/
│   │   ├── SignInPage.tsx
│   │   ├── SignUpPage.tsx
│   │   └── components/
│   │       ├── SignInForm.tsx
│   │       └── SignUpForm.tsx
│   └── Dashboard/
│       ├── DashboardPage.tsx
│       └── components/
│           ├── SubscriptionForm.tsx
│           ├── SubscriptionList.tsx
│           └── DonutChart.tsx
├── components/
│   └── common/              # birden fazla sayfada ortak bileşenler
├── lib/
│   ├── supabase.ts
│   └── currency.ts
└── types/
```

**Rotalar:**

| Rota | Sayfa | Erişim |
|------|-------|--------|
| `/` | Welcome | Herkese açık |
| `/sign-up` | Sign Up | Herkese açık |
| `/sign-in` | Sign In | Herkese açık |
| `/dashboard` | Dashboard | Oturum gerekli |

**Import akışı:** `main.tsx` → `App.tsx` → `pages/<PageName>/*Page.tsx` → `pages/<PageName>/components/`

---

## Döviz Mantığı

```js
export function toTRY(price, currency, rates) {
  if (currency === 'TRY') return price;
  return price * rates[currency];
}
```

Örnek sabit kurlar:

```js
const rates = { USD: 34.5, EUR: 37.2 };
```

---

## Tasarım

- Arka plan: `bg-slate-50`
- Kartlar: `bg-white shadow-sm rounded-xl`
- Butonlar: `bg-indigo-600 hover:bg-indigo-700 text-white`

---

## Dokümantasyon

- [prd.md](prd.md) — MVP gereksinimleri
- [userstory.md](userstory.md) — Kullanıcı hikayeleri
