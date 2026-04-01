# React Native Expo — APK ve IPA Oluşturma Rehberi

Bu rehber, React Native Expo ile geliştirdiğiniz uygulamayı **Android APK** ve **iOS IPA** dosyasına dönüştürme sürecini adım adım anlatmaktadır.

---

## İçindekiler

- [Gereksinimler](#gereksinimler)
- [Yöntem 1: Lokal Build (Hesap Gerektirmez)](#yöntem-1-lokal-build-hesap-gerektirmez)
  - [Android APK — Lokal Build](#android-apk--lokal-build)
  - [iOS IPA — Lokal Build](#ios-ipa--lokal-build)
- [Yöntem 2: EAS Build (Bulut Üzerinden)](#yöntem-2-eas-build-bulut-üzerinden)
  - [Android APK — EAS Build](#android-apk--eas-build)
  - [iOS IPA — EAS Build](#ios-ipa--eas-build)
- [Sık Karşılaşılan Hatalar ve Çözümleri](#sık-karşılaşılan-hatalar-ve-çözümleri)
- [Telefona Yükleme](#telefona-yükleme)
- [Faydalı Komutlar](#faydalı-komutlar)

---

## Gereksinimler

### Tüm Yöntemler İçin

| Araç | Açıklama | Kurulum |
|------|----------|---------|
| **Node.js** (v18+) | JavaScript çalışma ortamı | https://nodejs.org |
| **npm** veya **yarn** | Paket yöneticisi | Node.js ile birlikte gelir |
| **Expo CLI** | Expo komut satırı aracı | Projeyle birlikte gelir (`npx expo`) |
| **Git** | Sürüm kontrol | https://git-scm.com |

### Lokal Android Build İçin Ek Gereksinimler

| Araç | Açıklama | Kurulum |
|------|----------|---------|
| **Android Studio** | Android SDK ve JDK sağlar | https://developer.android.com/studio |
| **JDK 17** | Java Development Kit | Android Studio ile birlikte gelir |

> **Not:** Android Studio'yu sadece SDK ve JDK için kuruyoruz. Kurulumdan sonra Android Studio'yu açmanıza gerek yok, terminal üzerinden çalışacağız.

### Lokal iOS Build İçin Ek Gereksinimler

| Araç | Açıklama |
|------|----------|
| **macOS** bilgisayar | iOS build sadece Mac'te yapılabilir |
| **Xcode** (v15+) | App Store'dan ücretsiz indirilir |
| **Apple ID** | Ücretsiz Apple hesabı yeterli (test için) |
| **CocoaPods** | iOS bağımlılık yöneticisi |

---

## Yöntem 1: Lokal Build (Hesap Gerektirmez)

Bu yöntemde build işlemi kendi bilgisayarınızda gerçekleşir. Herhangi bir hesap açmanıza veya giriş yapmanıza **gerek yoktur**.

---

### Android APK — Lokal Build

#### Adım 1: Android Studio Kurulumu ve Ayarları

1. https://developer.android.com/studio adresinden Android Studio'yu indirin ve kurun.
2. Kurulum sırasında **Android SDK**, **Android SDK Platform-Tools** ve **Android SDK Build-Tools** bileşenlerini seçin.
3. Kurulum tamamlandıktan sonra Android Studio'yu bir kez açın ve SDK kurulumunun bitmesini bekleyin.

#### Adım 2: Ortam Değişkenlerini Ayarlayın

**Windows:**

Sistem ortam değişkenlerini ayarlayın. Başlat menüsünde "Ortam Değişkenleri" aratın veya `Denetim Masası → Sistem → Gelişmiş Sistem Ayarları → Ortam Değişkenleri` yolunu takip edin.

Yeni **Kullanıcı Değişkeni** ekleyin:

```
Değişken adı: ANDROID_HOME
Değişken değeri: C:\Users\KULLANICI_ADINIZ\AppData\Local\Android\Sdk
```

`Path` değişkenine şu satırları ekleyin:

```
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\tools
```

Değişkenleri ayarladıktan sonra **tüm terminal pencerelerini kapatıp yeniden açın.**

Doğrulama:

```bash
echo %ANDROID_HOME%
# Çıktı: C:\Users\KULLANICI_ADINIZ\AppData\Local\Android\Sdk
```

**macOS / Linux:**

`~/.bashrc`, `~/.zshrc` veya `~/.bash_profile` dosyasına şu satırları ekleyin:

```bash
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/tools
```

macOS'ta SDK yolu farklı olabilir:

```bash
export ANDROID_HOME=$HOME/Library/Android/sdk
```

Terminali yeniden başlatın veya:

```bash
source ~/.bashrc
# veya
source ~/.zshrc
```

Doğrulama:

```bash
echo $ANDROID_HOME
# Çıktı: /home/kullanici/Android/Sdk veya /Users/kullanici/Library/Android/sdk
```

#### Adım 3: Java (JDK) Kontrolü

Android Studio ile birlikte JDK gelir. Kontrol edin:

```bash
java -version
```

Beklenen çıktı (sürüm numarası farklı olabilir):

```
openjdk version "17.0.x" ...
```

Eğer Java bulunamazsa, `JAVA_HOME` ortam değişkenini ayarlayın:

**Windows:**

```
JAVA_HOME = C:\Program Files\Android\Android Studio\jbr
```

**macOS:**

```bash
export JAVA_HOME="/Applications/Android Studio.app/Contents/jbr/Contents/Home"
```

#### Adım 4: app.json Kontrolü

Proje klasöründeki `app.json` dosyasında `android.package` alanının tanımlı olduğundan emin olun:

```json
{
  "expo": {
    "name": "Uygulamam",
    "slug": "uygulamam",
    "version": "1.0.0",
    "android": {
      "package": "com.ogrenci.uygulamam"
    },
    "ios": {
      "bundleIdentifier": "com.ogrenci.uygulamam",
      "supportsTablet": true
    }
  }
}
```

> **Önemli:** `package` adı benzersiz olmalı. Formatı: `com.isim.uygulama` şeklindedir. Türkçe karakter kullanmayın.

#### Adım 5: Bağımlılıkları Yükleyin

```bash
cd proje-klasoru
npm install
```

#### Adım 6: Native Android Projesini Oluşturun

```bash
npx expo prebuild --platform android
```

Bu komut projenin kök dizininde `android/` klasörünü oluşturur. Bu klasör native Android proje dosyalarını içerir.

> **Not:** Eğer daha önce `prebuild` çalıştırdıysanız ve temiz başlamak istiyorsanız:
>
> ```bash
> npx expo prebuild --platform android --clean
> ```

#### Adım 7: APK Build Edin

**macOS / Linux:**

```bash
cd android
./gradlew assembleRelease
```

**Windows:**

```bash
cd android
gradlew.bat assembleRelease
```

Bu işlem ilk seferde **5-15 dakika** sürebilir (bağımlılıkları indirir). Sonraki build'ler daha hızlı olacaktır.

#### Adım 8: APK Dosyasını Bulun

Build başarılı olursa APK dosyası şu konumda oluşur:

```
android/app/build/outputs/apk/release/app-release.apk
```

Bu dosyayı kopyalayıp istediğiniz Android telefona yükleyebilirsiniz.

#### Adım 9: Doğrulama

Terminalde şu çıktıyı görmelisiniz:

```
BUILD SUCCESSFUL in Xm Xs
```

APK dosyasının var olduğunu doğrulayın:

**macOS / Linux:**

```bash
ls -la android/app/build/outputs/apk/release/
```

**Windows:**

```bash
dir android\app\build\outputs\apk\release\
```

---

### iOS IPA — Lokal Build

> **Uyarı:** iOS build sadece **macOS** bilgisayarda yapılabilir. Windows veya Linux'ta iOS build yapamazsınız.

#### Adım 1: Xcode Kurulumu

1. Mac App Store'dan **Xcode** uygulamasını indirin (ücretsiz, ~12 GB).
2. Kurulumdan sonra Xcode'u bir kez açın ve lisans sözleşmesini kabul edin.
3. Xcode Command Line Tools'u kurun:

```bash
xcode-select --install
```

Doğrulama:

```bash
xcode-select -p
# Çıktı: /Applications/Xcode.app/Contents/Developer
```

#### Adım 2: CocoaPods Kurulumu

```bash
sudo gem install cocoapods
```

Doğrulama:

```bash
pod --version
# Çıktı: 1.x.x
```

> **Not:** M1/M2/M3 Mac kullanıyorsanız ve hata alırsanız:
>
> ```bash
> sudo arch -x86_64 gem install cocoapods
> ```
>
> veya Homebrew ile:
>
> ```bash
> brew install cocoapods
> ```

#### Adım 3: app.json Kontrolü

`ios.bundleIdentifier` alanının tanımlı olduğundan emin olun (bkz. Android Adım 4).

#### Adım 4: Native iOS Projesini Oluşturun

```bash
cd proje-klasoru
npm install
npx expo prebuild --platform ios
```

Bu komut `ios/` klasörünü oluşturur.

#### Adım 5: Pod Bağımlılıklarını Yükleyin

```bash
cd ios
pod install
cd ..
```

> **Not:** Pod install sırasında hata alırsanız:
>
> ```bash
> cd ios
> pod deintegrate
> pod install
> cd ..
> ```

#### Adım 6: Xcode'da Projeyi Açın

```bash
open ios/uygulamam.xcworkspace
```

> **Dikkat:** `.xcodeproj` değil, `.xcworkspace` dosyasını açın. Workspace dosyası CocoaPods bağımlılıklarını içerir.

#### Adım 7: Xcode Ayarları

1. Xcode açıldıktan sonra sol panelden proje adınıza tıklayın.
2. **Signing & Capabilities** sekmesine gidin.
3. **Team** alanında Apple ID'nizi seçin. Eğer yoksa:
   - `Xcode → Settings → Accounts` menüsünden Apple ID'nizi ekleyin.
   - Ücretsiz Apple hesabı yeterlidir.
4. **Bundle Identifier** alanının `app.json`'daki ile aynı olduğunu kontrol edin.

#### Adım 8a: Kendi Telefonunuza Yükleme (Ücretsiz)

1. iPhone'unuzu USB kablosuyla Mac'e bağlayın.
2. iPhone'da "Bu Bilgisayara Güven" uyarısını onaylayın.
3. Xcode'un üst kısmındaki cihaz seçiciden iPhone'unuzu seçin.
4. ▶️ (Run) butonuna basın veya `Cmd + R` tuşlayın.
5. Uygulama doğrudan telefonunuza yüklenir.

> **Kısıtlamalar (ücretsiz Apple ID ile):**
> - Uygulama **7 gün** sonra çalışmayı durdurur. Tekrar Xcode'dan yüklemeniz gerekir.
> - Sadece **kendi cihazınıza** yükleyebilirsiniz.
> - İlk yüklemede iPhone'da: `Ayarlar → Genel → VPN ve Cihaz Yönetimi → Geliştirici Uygulaması` bölümünden sertifikaya güvenmeniz gerekir.

#### Adım 8b: IPA Dosyası Oluşturma (Apple Developer Hesabı Gerektirir — $99/yıl)

1. Xcode'da üst menüden: `Product → Archive`
2. Archive tamamlandığında **Organizer** penceresi açılır.
3. `Distribute App → Ad Hoc` seçin.
4. İmzalama ayarlarını yapın ve `Export` butonuna basın.
5. `.ipa` dosyası seçtiğiniz klasöre kaydedilir.

> **Not:** IPA dosyasını başka cihazlara yüklemek için cihaz UDID'lerinin Apple Developer Portal'a kayıtlı olması gerekir.

---

## Yöntem 2: EAS Build (Bulut Üzerinden)

Bu yöntemde build işlemi Expo'nun bulut sunucularında gerçekleşir. Bilgisayarınızda Android Studio veya Xcode kurulu olmasına **gerek yoktur**. Ancak ücretsiz bir **Expo hesabı** gerektirir.

### Ön Hazırlık

#### Adım 1: EAS CLI Kurulumu

```bash
npm install -g eas-cli
```

Doğrulama:

```bash
eas --version
# Çıktı: eas-cli/x.x.x ...
```

#### Adım 2: Expo Hesabına Giriş

```bash
eas login
```

Expo hesabınız yoksa https://expo.dev adresinden ücretsiz oluşturun. Terminalde e-posta ve şifrenizi girin.

Doğrulama:

```bash
eas whoami
# Çıktı: kullanici-adiniz
```

#### Adım 3: EAS Build Konfigürasyonu

Proje klasöründe çalıştırın:

```bash
eas build:configure
```

Bu komut proje kökünde `eas.json` dosyası oluşturur. Dosyayı şu şekilde düzenleyin:

```json
{
  "cli": {
    "version": ">= 3.0.0"
  },
  "build": {
    "preview": {
      "android": {
        "buildType": "apk"
      },
      "ios": {
        "distribution": "internal",
        "simulator": false
      },
      "distribution": "internal"
    },
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "production": {
      "android": {
        "buildType": "app-bundle"
      }
    }
  }
}
```

> **Profil Açıklamaları:**
> - `preview` → Test amaçlı. Android'de APK, iOS'ta Ad Hoc IPA üretir.
> - `development` → Geliştirme amaçlı. Development client üretir.
> - `production` → Mağazaya yükleme amaçlı. Android'de AAB üretir.

#### Adım 4: app.json Kontrolü

`app.json`'da şu alanların tanımlı olduğundan emin olun:

```json
{
  "expo": {
    "name": "Uygulamam",
    "slug": "uygulamam",
    "version": "1.0.0",
    "android": {
      "package": "com.ogrenci.uygulamam"
    },
    "ios": {
      "bundleIdentifier": "com.ogrenci.uygulamam",
      "supportsTablet": true
    }
  }
}
```

---

### Android APK — EAS Build

```bash
eas build --platform android --profile preview
```

Build süreci:

1. Proje dosyaları Expo sunucusuna yüklenir.
2. Bulutta build işlemi başlar (5-15 dakika, kuyruk durumuna göre değişir).
3. Build tamamlandığında terminalde bir **indirme linki** görüntülenir:

```
✔ Build finished
🔗 https://expo.dev/artifacts/eas/xxxxx.apk
```

Bu linki tarayıcıya yapıştırarak APK dosyasını indirin.

> **Not:** Ücretsiz Expo hesabında aylık **30 build** hakkınız vardır ve build sırası olabilir.

---

### iOS IPA — EAS Build

> **Gereksinim:** Apple Developer Program üyeliği ($99/yıl). Bu olmadan iOS build alamazsınız.

#### iOS Cihaz Kaydı (İlk Seferlik)

Test cihazlarının UDID'sini kaydetmeniz gerekir:

```bash
eas device:create
```

Bu komut bir kayıt linki üretir. Linki test yapılacak iPhone'dan Safari ile açın ve profili yükleyin. Bu işlem cihaz UDID'sini otomatik olarak kaydeder.

#### Build Başlatma

```bash
eas build --platform ios --profile preview
```

EAS size Apple ID ve şifrenizi soracaktır. Sertifika ve provisioning profile otomatik oluşturulur.

Build tamamlandığında indirme linki verilir:

```
✔ Build finished
🔗 https://expo.dev/artifacts/eas/xxxxx.ipa
```

#### Her İki Platform Aynı Anda

```bash
eas build --platform all --profile preview
```

---

## Sık Karşılaşılan Hatalar ve Çözümleri

### Android Hataları

#### `SDK location not found`

```
FAILURE: Build failed with an exception.
* What went wrong:
Could not determine the dependencies of task ':app:compileReleaseJavaWithJavac'.
> SDK location not found.
```

**Çözüm:** `ANDROID_HOME` ortam değişkeni ayarlanmamış. Adım 2'ye geri dönün.

Alternatif olarak, `android/` klasöründe `local.properties` dosyası oluşturun:

**Windows:**

```properties
sdk.dir=C:\\Users\\KULLANICI_ADINIZ\\AppData\\Local\\Android\\Sdk
```

**macOS:**

```properties
sdk.dir=/Users/KULLANICI_ADINIZ/Library/Android/sdk
```

**Linux:**

```properties
sdk.dir=/home/KULLANICI_ADINIZ/Android/Sdk
```

---

#### `Could not find or load main class org.gradle.wrapper.GradleWrapperMain`

**Çözüm:** Gradle wrapper dosyaları eksik.

```bash
cd android
gradle wrapper
cd ..
```

Eğer `gradle` komutu bulunamazsa Android Studio'dan Gradle'ı yoluna ekleyin veya:

```bash
cd android
npx expo prebuild --platform android --clean
cd android
./gradlew assembleRelease
```

---

#### `Execution failed for task ':app:mergeReleaseResources'`

**Çözüm:** Genellikle resim dosyalarındaki sorunlardan kaynaklanır. Asset dosya isimlerinde Türkçe karakter, boşluk veya büyük harf olmadığından emin olun.

---

#### `FAILURE: Build failed with an exception. Could not determine java version`

**Çözüm:** Java sürümü uyumsuz veya bulunamıyor.

```bash
java -version
```

JDK 17 kurulu olmalı. `JAVA_HOME` ayarlayın (Adım 3'e bakın).

---

#### `error Failed to install the app. Make sure you have the Android development environment set up`

**Çözüm:** Android SDK düzgün kurulmamış. Android Studio'yu açın → `SDK Manager` → aşağıdakilerin kurulu olduğundan emin olun:

- Android SDK Platform (en az API 33 veya 34)
- Android SDK Build-Tools
- Android SDK Platform-Tools

---

#### `Deprecated Gradle features were used in this build`

Bu bir **uyarıdır, hata değildir.** Build başarılı olmuştur. Güvenle devam edebilirsiniz.

---

#### `android/ klasörü zaten var` uyarısı

```bash
npx expo prebuild --platform android --clean
```

`--clean` parametresi mevcut `android/` klasörünü silip yeniden oluşturur.

---

### iOS Hataları

#### `xcode-select: error: tool 'xcodebuild' requires Xcode`

**Çözüm:**

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

---

#### `No signing certificate "iOS Development" found`

**Çözüm:** Xcode'da Apple ID eklenmemiş.

`Xcode → Settings → Accounts → + butonuyla Apple ID ekleyin`

---

#### `Unable to install "uygulamam"`

iPhone'da güven ayarı yapılmamış.

**Çözüm:** iPhone'da → `Ayarlar → Genel → VPN ve Cihaz Yönetimi` → geliştirici sertifikanıza dokunun → `Güven` butonuna basın.

---

#### `The sandbox is not in sync with the Podfile.lock`

**Çözüm:**

```bash
cd ios
pod deintegrate
pod install
cd ..
```

---

#### `error: Signing for "uygulamam" requires a development team`

**Çözüm:** Xcode'da Signing & Capabilities sekmesinde Team seçili değil. Apple ID'nizi ekleyip Team olarak seçin.

---

### EAS Build Hataları

#### `eas: command not found`

**Çözüm:**

```bash
npm install -g eas-cli
```

Eğer hâlâ bulunamıyorsa npm global path'i kontrol edin:

```bash
npm config get prefix
```

Çıkan yolu `PATH` ortam değişkenine ekleyin.

---

#### `This project is not configured to build with EAS`

**Çözüm:**

```bash
eas build:configure
```

---

#### `Invalid credentials`

**Çözüm:** Expo giriş bilgilerinizi kontrol edin:

```bash
eas logout
eas login
```

---

#### Build çok uzun sürüyor

Ücretsiz planda kuyruk oluşabilir. https://expo.dev adresinden build durumunu takip edebilirsiniz. Genellikle 15-30 dakika içinde tamamlanır.

---

## Telefona Yükleme

### Android APK Yükleme

1. APK dosyasını telefona aktarın (USB kablo, Google Drive, WeTransfer, WhatsApp vb.).
2. Telefonda `Ayarlar → Güvenlik → Bilinmeyen Kaynaklar` (veya `Bilinmeyen Uygulamalar Yükle`) seçeneğini açın.
   - Android 8+ sürümlerde bu izin uygulama bazlıdır: dosya yöneticisi veya Chrome için ayrı ayrı izin vermeniz gerekebilir.
3. APK dosyasına dokunun ve `Yükle` butonuna basın.
4. Yükleme tamamlandığında `Aç` butonuyla uygulamayı çalıştırın.

### iOS IPA Yükleme

- **Xcode ile (ücretsiz):** USB kablosu ile doğrudan yüklenir (bkz. Adım 8a).
- **Ad Hoc (ücretli):** EAS build sonrası verilen link Safari'den açılarak yüklenir.
- **TestFlight:** Production build alıp `eas submit --platform ios` komutuyla TestFlight'a yüklersiniz. Test kullanıcıları TestFlight uygulamasından indirir.

---

## Faydalı Komutlar

### Genel

```bash
# Expo proje bilgisi
npx expo config

# Bağımlılıkları kontrol et (uyumsuzluk varsa düzeltir)
npx expo install --check

# Expo doctor (sorunları tespit eder)
npx expo-doctor
```

### Lokal Build

```bash
# Native projeyi oluştur (Android)
npx expo prebuild --platform android

# Native projeyi oluştur (iOS)
npx expo prebuild --platform ios

# Native projeyi temizleyip yeniden oluştur
npx expo prebuild --platform android --clean
npx expo prebuild --platform ios --clean

# Android APK build (macOS/Linux)
cd android && ./gradlew assembleRelease

# Android APK build (Windows)
cd android && gradlew.bat assembleRelease

# Android — Önceki build çıktılarını temizle
cd android && ./gradlew clean

# iOS — Pod bağımlılıklarını yükle
cd ios && pod install

# iOS — Pod sorunlarını sıfırla
cd ios && pod deintegrate && pod install

# Xcode'da projeyi aç
open ios/PROJEADI.xcworkspace
```

### EAS Build

```bash
# EAS CLI kur
npm install -g eas-cli

# Giriş yap
eas login

# Giriş kontrolü
eas whoami

# Konfigürasyon oluştur
eas build:configure

# Android APK build
eas build --platform android --profile preview

# iOS IPA build
eas build --platform ios --profile preview

# Her iki platform birlikte
eas build --platform all --profile preview

# iOS cihaz kaydı
eas device:create

# Build durumunu kontrol et
eas build:list

# Çıkış yap
eas logout
```

---

## Yöntem Karşılaştırması

| Özellik | Lokal Build | EAS Build |
|---------|-------------|-----------|
| **Hesap gereksinimi** | Yok (Android) | Expo hesabı (ücretsiz) |
| **Yazılım gereksinimi** | Android Studio / Xcode | Sadece Node.js + EAS CLI |
| **Build süresi** | 5-15 dk | 10-30 dk (kuyruk dahil) |
| **İnternet gereksinimi** | İlk seferde (bağımlılıklar için) | Her build'de |
| **iOS build** | Sadece Mac'te | Bulutta (ama Apple Developer hesabı şart) |
| **Aylık limit** | Sınırsız | 30 build/ay (ücretsiz plan) |
| **Tavsiye** | Sınıf ortamında Android test | Hızlı dağıtım, CI/CD |

---

## Hızlı Başlangıç Özeti

Sadece Android APK üretmek istiyorsanız, en kısa yol:

```bash
# 1. Bağımlılıkları yükle
npm install

# 2. Native projeyi oluştur
npx expo prebuild --platform android

# 3. APK build et
cd android
./gradlew assembleRelease    # macOS/Linux
gradlew.bat assembleRelease  # Windows

# 4. APK burada:
# android/app/build/outputs/apk/release/app-release.apk
```

---

> **Hazırlayan:** Bilişim Teknolojileri Bölümü  
> **Amaç:** React Native Expo — Eğitim Amaçlı APK/IPA Oluşturma Rehberi
