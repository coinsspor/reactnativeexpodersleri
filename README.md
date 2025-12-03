# 📱 Modül 01: Başlangıç Hazırlıkları

Bu modülde bilgisayarını React Native geliştirmeye hazırlayacaksın.

---

## 1. Visual Studio Code Nedir?

Visual Studio Code (kısaca VS Code), kod yazmak için kullanılan bir programdır. Not defteri gibi düşün ama kodlama için özel olarak tasarlanmış. Kodlarını renklendiriyor, hataları gösteriyor ve işini kolaylaştırıyor.

### VS Code Kurulumu

1. Tarayıcıda şu adrese git: **https://code.visualstudio.com**
2. "Download for Windows" butonuna tıkla
3. İndirilen dosyayı çalıştır
4. "Next" diyerek kurulumu tamamla

### VS Code'u Türkçe Yapma (İsteğe Bağlı)

1. VS Code'u aç
2. Sol taraftaki kare simgeli "Extensions" bölümüne tıkla (veya Ctrl+Shift+X)
3. Arama kutusuna "Turkish" yaz
4. "Turkish Language Pack" yükle
5. VS Code'u yeniden başlat

---

## 2. Terminal Nedir?

Terminal, bilgisayara yazı yazarak komut verdiğin siyah bir ekrandır. Normalde fare ile klasör açar, program çalıştırırsın. Terminal'de bunları yazarak yaparsın.

### VS Code'da Terminal Açma

VS Code açıkken:
- Üst menüden: **Terminal → New Terminal**
- Veya klavyeden: **Ctrl + `** (Ctrl ve ters tırnak tuşu, Tab'ın üstündeki tuş)

Altta siyah bir ekran açılacak. Bu terminal.

### Temel Terminal Komutları

Terminale şunları yazıp Enter'a basarak dene:

```bash
cd Desktop
```
**Ne yapar?** Masaüstü klasörüne gider. "cd" = "change directory" (dizin değiştir) demek.

```bash
dir
```
**Ne yapar?** Bulunduğun klasördeki dosyaları listeler. (Mac/Linux'ta `ls` kullanılır)

```bash
cd ..
```
**Ne yapar?** Bir üst klasöre çıkar. İki nokta "üst klasör" demek.

```bash
mkdir projeler
```
**Ne yapar?** "projeler" adında yeni klasör oluşturur. "mkdir" = "make directory" (dizin oluştur)

### Terminal Neden Lazım?

React Native projesi oluşturmak ve çalıştırmak için terminal komutları kullanacağız. Fare ile yapılamıyor.

---

## 3. Node.js Nedir?

Node.js, JavaScript kodlarını bilgisayarında çalıştıran bir programdır. 

Normalde JavaScript sadece tarayıcıda (Chrome, Firefox) çalışır. Node.js sayesinde bilgisayarında da çalışır. React Native, Node.js kullanıyor.

### Node.js Kurulumu

1. Şu adrese git: **https://nodejs.org**
2. **LTS** yazan yeşil butona tıkla (kararlı sürüm)
3. İndirilen dosyayı çalıştır
4. "Next" diyerek kurulumu tamamla

### Kurulumu Kontrol Etme

VS Code'da terminal aç ve şunu yaz:

```bash
node --version
```

Şöyle bir şey görmelisin:
```
v20.10.0
```

Eğer "node tanınmıyor" gibi bir hata alıyorsan, VS Code'u kapat aç veya bilgisayarı yeniden başlat.

---

## 4. npm Nedir?

npm = Node Package Manager (Node Paket Yöneticisi)

Başka programcıların yazdığı hazır kodları (paket/kütüphane) indirmeni sağlar. Mesela birisi "tarih hesaplama" için kod yazmış, sen npm ile indirip kullanabilirsin. Tekerleği yeniden icat etmene gerek yok.

Node.js kurduğunda npm de otomatik kuruluyor.

### npm Kurulu mu Kontrol Et

Terminalde şunu yaz:

```bash
npm --version
```

Şöyle bir şey görmelisin:
```
10.2.3
```

---

## 5. Expo Nedir?

Expo, React Native geliştirmeyi kolaylaştıran bir araçtır.

Normal React Native'de Android Studio ve Xcode kurmak gerekiyor (çok karmaşık). Expo ile bunlara gerek yok. Telefonuna "Expo Go" uygulamasını yükle, bilgisayarındaki kodu telefonunda anında gör.

### Expo Go Uygulamasını Yükle

Telefonunda:
- **Android:** Play Store'da "Expo Go" ara ve yükle
- **iPhone:** App Store'da "Expo Go" ara ve yükle

---

## 6. VS Code Eklentileri (Önerilen)

VS Code'da sol taraftaki kare simgeye tıkla (Extensions). Şunları ara ve yükle:

1. **ES7+ React/Redux/React-Native/JS snippets**
   - Hızlı kod yazmanı sağlar
   
2. **Prettier - Code formatter**
   - Kodunu düzgün hizalar

3. **React Native Tools**
   - React Native için özel araçlar

---

## ✅ Kontrol Listesi

Devam etmeden önce bunların hepsinin tamam olduğundan emin ol:

- [ ] VS Code kuruldu
- [ ] Terminali açabiliyorum (Ctrl + `)
- [ ] `node --version` çalışıyor
- [ ] `npm --version` çalışıyor
- [ ] Telefonuma Expo Go yükledim

Hepsi tamam mı? O zaman sonraki modüle geç!

---

## 🔗 Sonraki Modül

[Modül 02: İlk Projeyi Oluşturma →](../02-ilk-proje/README.md)
