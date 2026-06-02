# 🛡️ Dijital Garanti Sistemi — Kurulum Rehberi

## Sisteme Genel Bakış

Bu sistem **3 parçadan** oluşur:
- `verify.html` → Müşteri doğrulama sayfası (public, QR ile açılır)
- `admin.html` → Sizin kullandığınız yönetim paneli (şifre korumalı)
- `data/` → JSON veri dosyaları (GitHub'da saklanır, otomatik yedeklenir)

---

## ADIM 1 — GitHub Hesabı ve Repository

### 1.1 GitHub'a kayıt olun
Eğer hesabınız yoksa: https://github.com/signup

### 1.2 Yeni repository oluşturun
1. GitHub'da sağ üstteki `+` → **New repository**
2. Repository adı: `garanti-sistemi` (istediğiniz ismi verebilirsiniz)
3. **Public** seçin ← Bu önemli, yoksa verify.html çalışmaz
4. **Create repository**'e tıklayın

---

## ADIM 2 — Dosyaları Yükleyin

### 2.1 Klasör yapısını oluşturun

Repository ana sayfasında `Add file` → `Upload files`:

Şu dosyaları yükleyin:
```
verify.html
admin.html
data/warranties.json
data/products.json
```

> **İpucu:** `data/warranties.json` dosyasını yüklemek için önce bilgisayarınızda `data` klasörü oluşturup içine koyun, sonra klasörün tamamını sürükleyip bırakın.

### 2.2 Commit edin
Sayfanın altında "Commit changes" butonuna tıklayın.

---

## ADIM 3 — GitHub Pages'i Aktif Edin

1. Repository sayfasında **Settings** sekmesine gidin
2. Sol menüden **Pages**'e tıklayın
3. **Source** kısmında `Deploy from a branch` seçin
4. **Branch**: `main` seçin, klasör: `/ (root)`
5. **Save**'e tıklayın

Birkaç dakika bekleyin. Adresiniz şu şekilde olacak:
```
https://KULLANICIADI.github.io/garanti-sistemi/
```

---

## ADIM 4 — Kendi Domaininizi Bağlayın (İsteğe bağlı)

Eğer `garanti.firmaniz.com` gibi bir alt domain kullanmak istiyorsanız:

### 4.1 DNS Ayarı (Domain sağlayıcınızdan)
Domain panelinizde **CNAME kaydı** ekleyin:
```
Ad (Host):    garanti
Değer:        KULLANICIADI.github.io
TTL:          Auto
```

### 4.2 GitHub Pages Ayarı
Settings → Pages → **Custom domain** kutusuna yazın:
```
garanti.firmaniz.com
```
**Enforce HTTPS** kutusunu işaretleyin.

---

## ADIM 5 — GitHub Token Oluşturun (Admin paneli için)

Admin panelinden kayıt eklemek için GitHub'ın API'sine yazma izni gerekir.

1. GitHub → sağ üst profiliniz → **Settings**
2. Sol altta: **Developer settings**
3. **Personal access tokens** → **Fine-grained tokens**
4. **Generate new token**
5. Ayarlar:
   - Token name: `garanti-admin`
   - Expiration: `No expiration` (veya 1 yıl)
   - Repository access: `Only select repositories` → `garanti-sistemi`
   - Permissions → **Contents**: `Read and write`
6. **Generate token** → Çıkan kodu kopyalayın ve saklayın!

---

## ADIM 6 — Admin Panelini Yapılandırın

1. Tarayıcınızda `admin.html` dosyasını açın
   - Yerel test için: `file:///...admin.html`
   - Veya GitHub Pages üzerinden: `https://...github.io/garanti-sistemi/admin.html`
2. İlk şifre: `admin123`
3. Giriş yapın → **⚙️ Ayarlar** sayfasına gidin
4. Doldurun:
   - GitHub Kullanıcı Adı
   - Repository Adı: `garanti-sistemi`
   - GitHub Token: (5. adımda aldığınız)
   - Şirket Adı
   - Garanti Doğrulama URL: `https://garanti.firmaniz.com/verify.html`
5. Şifrenizi değiştirin
6. **💾 Ayarları Kaydet** → **🔗 Bağlantıyı Test Et**

---

## ADIM 7 — verify.html'deki Şirket Adını Güncelleyin

`verify.html` dosyasını açın ve şu satırı bulun:
```javascript
const CONFIG = {
  DATA_URL: 'data/warranties.json',
  COMPANY_NAME: 'Şirket Adı',   ← buraya şirket adınızı yazın
};
```

Dosyayı kaydedin ve GitHub'a yeniden yükleyin.

---

## ADIM 8 — İlk Garantiyi Oluşturun

1. Admin paneline girin
2. **📦 Yeni Ürün** sayfasından ürünlerinizi ekleyin
3. **➕ Yeni Garanti** sayfasından garanti kaydı oluşturun
4. Kayıt listesinden **QR** butonuna tıklayın
5. QR kodu indirin — etikete veya faturaya yapıştırın!

---

## Müşteriye Link Gönderme

Her garanti için iki yöntem:
- **QR Kod** → Ürüne/faturaya yapıştırın, müşteri tarayıcısıyla okur
- **Link** → `https://garanti.firmaniz.com/verify.html?id=GAR-2024-0001`
  Bu linki WhatsApp, SMS veya e-posta ile gönderin.

---

## Otomatik Yedekleme

**Git zaten yedektir!** Her kayıt GitHub'a bir "commit" olarak gider.
- Yanlışlıkla silinen veriyi kurtarmak için: GitHub → repository → **History**
- Ek güvenlik için: Admin paneli → **Ayarlar** → **JSON Olarak İndir**

---

## Sık Sorulan Sorular

**S: Veriler nerede saklanıyor?**
GitHub'daki `data/warranties.json` dosyasında. Admin panelinde de tarayıcı localStorage'ında yerel kopyası var.

**S: İnternet kesilse ne olur?**
Admin paneline kaydettiğiniz veriler localStorage'da kalır. İnternet gelince GitHub'a senkronize olur.

**S: QR kodu basan bir etiket nasıl yapabilirim?**
QR kodu PNG olarak indirin. Word, Canva veya herhangi bir tasarım uygulamasına ekleyip yazdırın.

**S: Token'ı kaybettim.**
GitHub'dan yeni token oluşturup Admin → Ayarlar'dan güncelleyin.

**S: Kaç garanti kaydı tutabilir?**
GitHub ücretsiz plan 500MB depo alanı verir. JSON olarak günde 1000 garanti ekleseniz yıllarca dolmaz.

---

## Destek

Herhangi bir sorun yaşarsanız:
1. `admin.html` → Ayarlar → **🔗 Bağlantıyı Test Et** ile bağlantıyı kontrol edin
2. GitHub → Repository → **Actions** sekmesinde hata var mı kontrol edin
3. Browser console'da (F12) hata mesajlarına bakın

