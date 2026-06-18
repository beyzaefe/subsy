# User Stories — Subsy MVP

**Format:** As a [role], I want [goal], so that [benefit].

---

## Epic 1 — Kimlik Doğrulama

### US-000: Welcome (Giriş Noktası)

**As a** yeni veya kayıtlı ziyaretçi,  
**I want to** uygulamaya girdiğimde Kayıt Ol veya Giriş Yap seçeneğini net görmek,  
**so that** doğru auth ekranına gidebileyim.

**Kabul Kriterleri:**

- [ ] Kök rota (`/`) Welcome ekranını gösterir
- [ ] Subsy logosu ve tagline görünür
- [ ] **Ücretsiz Başla** butonu `/sign-up` sayfasına yönlendirir
- [ ] **Giriş Yap** butonu `/sign-in` sayfasına yönlendirir
- [ ] Oturum açmış kullanıcı `/` adresine gelirse dashboard'a yönlendirilir
- [ ] Figma Light Mode tokenlarına uygun tasarım (`bg-slate-50`, beyaz kart, `bg-indigo-600` primary)

---

### US-001: Kayıt Ol (Sign Up)

**As a** yeni kullanıcı,  
**I want to** ad soyad, e-posta ve şifre ile kayıt olmak,  
**so that** aboneliklerimi saklayabileyim.

**Kabul Kriterleri:**

- [ ] Ad Soyad, E-posta ve Şifre alanları bulunur
- [ ] Ad Soyad user metadata'sına kaydedilir
- [ ] Supabase Auth ile kayıt tamamlanır
- [ ] Başarılı kayıt sonrası dashboard'a yönlendirilir
- [ ] Hatalı girişte kullanıcıya mesaj gösterilir
- [ ] **Geri** ile Welcome (`/`) ekranına dönülebilir

---

### US-002: Giriş Yap (Sign In)

**As a** kayıtlı kullanıcı,  
**I want to** e-posta ve şifre ile giriş yapmak,  
**so that** aboneliklerime erişebileyim.

**Kabul Kriterleri:**

- [ ] E-posta ve şifre alanları bulunur
- [ ] Supabase Auth ile oturum açılır
- [ ] Başarılı girişte dashboard gösterilir
- [ ] Giriş yapmadan dashboard erişilemez
- [ ] **Geri** ile Welcome (`/`) ekranına dönülebilir

---

## Epic 2 — Abonelik Yönetimi

### US-003: Abonelik Ekleme

**As a** giriş yapmış kullanıcı,  
**I want to** yeni abonelik eklemek,  
**so that** harcamalarımı takip edebileyim.

**Kabul Kriterleri:**

- [ ] Form alanları: Abonelik Adı, Fiyat, Döviz Cinsi, Kategori, Ödeme Tarihi
- [ ] Döviz seçenekleri: TRY, USD, EUR
- [ ] Kayıt Supabase `subscriptions` tablosuna yazılır
- [ ] Ekleme sonrası liste güncellenir

---

### US-004: Abonelik Listeleme

**As a** giriş yapmış kullanıcı,  
**I want to** aboneliklerimi listelemek,  
**so that** tüm aboneliklerimi tek ekranda görebileyim.

**Kabul Kriterleri:**

- [ ] Kullanıcı yalnızca kendi aboneliklerini görür
- [ ] Her satır/kart: ad, fiyat, döviz, kategori, ödeme tarihi
- [ ] Liste boşsa bilgilendirici mesaj gösterilir

---

### US-005: Abonelik Silme

**As a** giriş yapmış kullanıcı,  
**I want to** bir aboneliği silmek,  
**so that** iptal ettiğim servisleri listeden çıkarabileyim.

**Kabul Kriterleri:**

- [ ] Her abonelikte silme aksiyonu bulunur
- [ ] Silme sonrası kayıt veritabanından kaldırılır
- [ ] Liste ve dashboard anında güncellenir

---

## Epic 3 — Dashboard

### US-006: Aylık Toplam Harcama

**As a** giriş yapmış kullanıcı,  
**I want to** tüm aboneliklerimin TRY cinsinden aylık toplamını görmek,  
**so that** ne kadar harcadığımı anlayayım.

**Kabul Kriterleri:**

- [ ] USD ve EUR abonelikler TRY'ye çevrilir
- [ ] Ekranda "Aylık Toplam Harcama" TRY olarak gösterilir
- [ ] Abonelik yoksa toplam `₺0,00` gösterilir

---

### US-007: Kategori Donut Chart

**As a** giriş yapmış kullanıcı,  
**I want to** harcamalarımı kategorilere göre pasta grafikte görmek,  
**so that** hangi kategoriye ne harcadığımı anlayayım.

**Kabul Kriterleri:**

- [ ] Recharts Donut Chart kullanılır
- [ ] Dilimler kategori bazlı TRY tutarlarını gösterir
- [ ] Abonelik yoksa grafik yerine boş durum mesajı gösterilir

---

## Epic 4 — Arayüz

### US-008: Light Mode Tasarım

**As a** kullanıcı,  
**I want to** sade ve okunaklı bir arayüz görmek,  
**so that** uygulamayı kolayca kullanabileyim.

**Kabul Kriterleri:**

- [ ] Sayfa arka planı `bg-slate-50`
- [ ] İçerik alanları beyaz kartlarda (`bg-white shadow-sm rounded-xl`)
- [ ] Birincil butonlar `bg-indigo-600`
