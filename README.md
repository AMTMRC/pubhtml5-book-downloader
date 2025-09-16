# HAR Dosyasından Kitap İndirici (HAR-Based Book Downloader)

Bu PowerShell script'i, tarayıcınızın "Geliştirici Araçları" ile kaydettiğiniz bir **Ağ Trafiği (`.har`)** dosyasını analiz ederek, dijital kitap veya dergi sayfalarını `.webp` formatında indirir ve ardından `.jpg` formatına dönüştürür.

Bu yöntem, doğrudan URL tahmini yapmanın zor olduğu veya sayfa adreslerinin karmaşık olduğu sitelerde oldukça etkilidir.

## ✨ Temel Özellikler

-   **HAR Dosyası Analizi:** Ağ trafiği kaydını okuyarak resim URL'lerini otomatik olarak bulur.
-   **Akıllı Sıralama:** İndirilecek sayfaları, dosya adlarındaki sayíları analiz ederek doğru sıraya koyar.
-   **WEBP'den JPG'ye Dönüştürme:** İndirilen modern `.webp` formatındaki resimleri, daha yaygın olan `.jpg` formatına çevirir.
-   **Toplu İndirme:** Tespit edilen tüm sayfaları tek bir komutla indirir.

## ⚙️ Nasıl Çalışır?

Script'in çalışma mantığı adımlar halinde şöyledir:
1.  Kullanıcının sağladığı `network.har` dosyasını okur.
2.  Bu dosya içindeki tüm ağ isteklerini tarar ve kitap sayfalarına ait olan `.webp` uzantılı resim URL'lerini filtreler.
3.  URL'lerin veya dosya adlarının içindeki sayıları (`1.webp`, `sayfa-2.webp` vb.) bularak resimleri doğru sayfa sırasına göre dizer.
4.  Sıralanmış listedeki her bir resmi `donusturulmus_goruntuler` adında bir klasöre indirir.
5.  İndirme sonrası, harici bir araç olan **ImageMagick**'i kullanarak her bir `.webp` dosyasını `.jpg` dosyasına dönüştürür.

## ⚠️ Gereksinimler

Bu script'i çalıştırmadan önce sisteminizde şunların kurulu olması gerekir:

1.  **Windows ve PowerShell:** Script, Windows'un komut satırı aracı olan PowerShell'de çalışır.
2.  **ImageMagick:** `.webp` formatını `.jpg` formatına dönüştürmek için kullanılan güçlü bir resim işleme aracıdır.
    -   [ImageMagick'i buradan indirin](https.imagemagick.org/script/download.php).
    -   **ÖNEMLİ:** Kurulumdan sonra script'in içindeki `$imageMagickPath` değişkenini, kendi bilgisayarınızdaki `magick.exe`'nin konumuyla güncellemeniz gerekebilir.
        ```powershell
        # Örnek:
        $imageMagickPath = "C:\Program Files\ImageMagick-7.1.2-Q16-HDRI\magick.exe"
        ```

## 🚀 Kullanım Adımları

### Adım 1: `.har` Dosyasını Oluşturma

1.  İndirmek istediğiniz kitabın bulunduğu web sayfasını tarayıcınızda (Chrome, Edge, Firefox) açın.
2.  `F12` tuşuna basarak **Geliştirici Araçları**'nı açın.
3.  **Network (Ağ)** sekmesine gelin.
4.  **"Disable cache" (Önbelleği devre dışı bırak)** kutucuğunun işaretli olduğundan emin olun.
5.  Kitabın **tüm sayfalarını tek tek gezin**. Bu, her sayfanın resminin Network sekmesine kaydedilmesi için gereklidir.
6.  Tüm sayfaları gezdikten sonra, Network sekmesindeki listenin üzerinde bulunan **"Export HAR..."** (HAR'ı Dışa Aktar) ikonuna (genellikle aşağı ok şeklinde) tıklayın.
7.  Dosyayı `network.har` adıyla kaydedin.

### Adım 2: Script'i Çalıştırma

1.  Bu GitHub deposundaki PowerShell script'ini (`.ps1` uzantılı dosyayı) indirin.
2.  Oluşturduğunuz `network.har` dosyasını, indirdiğiniz script ile **aynı klasörün içine** koyun.
3.  O klasörün içinde bir PowerShell penceresi açın (Klasörde boş bir yere Shift + Sağ Tık yapıp "PowerShell penceresini burada aç" seçeneğini kullanabilirsiniz).
4.  Aşağıdaki komutu yazarak script'i çalıştırın:
    ```powershell
    .\dosya_adiniz.ps1
    ```
    *Not: Eğer bir çalıştırma ilkesi hatası alırsanız, önce `Set-ExecutionPolicy RemoteSigned -Scope Process` komutunu çalıştırıp tekrar deneyin.*

### Adım 3: Sonuç

Script, işlemleri tamamladığında `donusturulmus_goruntuler` adında bir klasör oluşturacak ve kitabınızın tüm sayfalarını `.jpg` formatında bu klasörün içine kaydedecektir. Artık bu resimleri bir PDF oluşturucu ile birleştirebilirsiniz.
