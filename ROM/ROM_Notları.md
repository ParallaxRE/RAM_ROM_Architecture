<div align="center">


<font color="#FF5733"> # ROM – Sadece Okunabilir Bellek

 **Tasarım Amacı:** Kalıcı verileri tutmak için tasarlanmıştır; veriler sık sık değiştirilmez.
 **Veri Girişi:** Veriler üretim süreci sırasında girilebilir.
 **Mikrobilgisayar Programları:** Uçucu (volatile) olmadığı için mikrobilgisayar programlarını (firmware) saklar.
 **Kullanım Alanları:** Hesap makineleri, beyaz eşyalar, güvenlik sistemleri vb. cihazlarda programlanmış halde bulunur.

---

## ROM Mimarisi (ROM Architecture)

Temel bir ROM mimarisi, donanım seviyesinde **4 ana parçadan** oluşur. Örnek bir mimaride, 16 farklı adresin her birinde 8-bit veri tutabilen **16 x 8 kapasiteli** bir yapı düşünülebilir.

### 1. Satır Çözücü (Row Decoder)
 **Görevi:** İşlemciden gelen adres bilgilerinin bir kısmını okuyarak hedef satırı aktif hale getirir.
 **Şema Detayı:** `A0` ve `A1` girişlerini alan **1-of-4 decoder** (4'ten 1'e çözücü) mantığı kullanılır. Bu çözücü, gelen 2 bitlik adrese göre 4 satırdan (Row 0, 1, 2, 3) sadece birini seçer.

### 2. Sütun Çözücü (Column Decoder)
 **Görevi:** Adres bilgisinin geri kalan kısmını okuyarak doğru sütunu seçer ve sadece o sütundaki verinin veri yoluna (Data bus) inmesine izin verir.
 **Şema Detayı:** `A2` ve `A3` girişlerini kullanarak yine **1-of-4 decoder** mantığıyla 4 sütundan (Column 0, 1, 2, 3) birini hedefler.

### 3. Register Dizisi (Register Array)
 **Görevi:** Verilerin asıl depolandığı hücre matrisidir. Satır ve sütun çizgilerinin kesiştiği her noktada bir "Register" bulunur.
 **Şema Detayı:** 4 satır ve 4 sütunun kesişimiyle toplam **16 adet Register** oluşur. Her bir register **8-bit** veri (word) saklar. Bir veriyi okumak için o register'ın hem satırından hem de sütunundan "aktif" (Enable - E) sinyali alması gerekir.

### 4. Çıkış Tamponları (Output Buffer)
 **Görevi:** Register'dan okunan verinin, çipin dışına (işlemciye veya veri yoluna) güvenle aktarılmasını sağlayan çıkış kapılarıdır.
 **Şema Detayı:** 8-bitlik veri yolu çıkış tamponlarına gelir. Ancak verinin pinlere (`D0` - `D7`) yansıması için **CS (Chip Select)** sinyalinin aktif edilmesi şarttır. Aksi takdirde pinler "High-Z" (Yüksek Empedans) durumunda kalır ve veri yolunu meşgul etmez.

---

## ROM (Read-Only Memory) Çeşitleri

 **Mask-Programmed ROM:** Üretim aşamasında içine veriler "maskelenerek" basılır. Kullanıcı tarafından asla değiştirilemez. Seri üretim çiplerde kullanılır.
 **PROM (Programmable ROM):** Üretildiğinde boştur. Özel bir cihazla bir kez programlanabilir. Bir hata yaparsan geri dönüşü yoktur, çip "çöp" olur.
 **EPROM (Erasable Programmable ROM):** İçindeki veriler ultraviyole (UV) ışık ile silinebilir. Silindikten sonra tekrar programlanabilir. Çipin üzerinde genellikle ışığın girmesi için küçük bir cam pencere olur.
 **EEPROM (Electrically Erasable PROM):** Veriler ışık yerine elektriksel sinyallerle silinebilir. Çipi yerinden çıkarmadan içindeki veriyi güncelleyebilirsin. Modern anakartlardaki BIOS çiplerinin atasıdır.
 **Flash Memory:** Günümüzde en çok kullandığımız türdür (USB bellekler, SSD'ler). EEPROM'un çok daha hızlı ve bloklar halinde silinebilen gelişmiş bir versiyonudur.

---

## Çeşitlerin Detaylı Özellikleri

### Mask-Programmed ROM Özellikleri
 **Üretici Tarafından Yazılır:** Programlar, özel isteklere veya teknik şartnamelere göre doğrudan üretici firma tarafından çipin içine yazılır.
 **Fotoğrafik Negatif (Maske) Kullanımı:** Elektriksel bağlantıları kontrol etmek için bir "maske" (fotoğrafik negatif) kullanılır. Bu, verinin fiziksel olarak çipin devresine işlenmesi demektir.
 **Ekonomik Olması İçin Seri Üretim Şart:** Bu maskeleri hazırlamak oldukça pahalıdır. Bu yüzden maliyeti düşürmek için ancak çok yüksek miktarlarda (binlerce, on binlerce) üretim yapıldığında ekonomik hale gelir.
 **Yeniden Programlanamaz:** Veri bir kez çipin fiziksel yapısına işlendiği için sonradan değiştirilmesi veya silinmesi imkansızdır.
 **Kullanım Alanı Örneği:** Eski tip monitörlerde (CRT) karakter üreticisi olarak kullanılması buna güzel bir örnektir.

### PROM (Programmable ROM) Özellikleri
 **Fusible-link MROM (Sigortalı Bağlantı):** Bu çiplerin içinde "sigorta" (fuse) adı verilen binlerce küçük iletken köprü vardır. Programlama sırasında bu sigortalar yüksek akımla yakılır veya sağlam bırakılır.
 **Kullanıcı Tarafından Programlanabilir:** Üretici bu çipi boş (tüm sigortalar sağlam) olarak satar. Kullanıcı, kendi ihtiyacına göre özel bir cihazla (PROM programmer) verisini içine işler.
 **OTP (One Time Programmable):** "Bir Kez Programlanabilir" özelliğine sahiptir. Bir sigorta yandığında artık geri dönüşü yoktur.
 **Silinemez:** Bir kez programlandığında içindeki veriler asla silinemez veya üzerine yazılamaz.

### EPROM (Silinebilir Programlanabilir ROM) Özellikleri
 **Programlama Şartları:**
   **Özel Voltaj:** Bu çipleri programlamak için normal çalışma voltajından daha yüksek, özel bir voltaja ihtiyaç duyulur (Genellikle 10-25V arası).
   **Zamanlama:** Veriyi hücrelere işlemek için belirli bir süre gerekir (Genellikle hücre başına 50 milisaniye civarı).
 **Silinebilirlik:** En büyük özelliği budur! İçindeki veriler Ultraviyole (UV) ışık kullanılarak silinebilir. 
   *Not: Bu çiplerin üzerinde genellikle veriyi silmek için UV ışığının içeri girmesini sağlayan şeffaf bir kuvars pencere bulunur.*
 **Örnekler:**
   **2732:** 4K x 8 NMOS yapısında bir EPROM örneğidir.
   **2764:** 8K x 8 kapasiteye sahip bir versiyondur.

### EEPROM (Elektriksel Olarak Silinebilir Programlanabilir ROM)
 **Elektriksel Silme:** Veriler elektriksel sinyallerle silinebilir. Bunun için yüksek bir voltaja (örneğin 21V) ihtiyaç duyulur. Bu yüksek voltaj, çipin kendi içindeki devreler sayesinde standart 5V'tan üretilir.
 **Hızlı Yazma:** PROM teknolojisine göre çok daha hızlıdır. Yazma süresi hücre başına yaklaşık 5 mikrosaniyedir (5usec).
 **Otomatik Silme:** Yazma işlemi sırasında, çipin içindeki devreler yazılacak olan hücreyi otomatik olarak siler. Yani "üzerine yazma" işlemini kendi halleder.
 **Tarihsel Örnekler:** Intel tarafından üretilen 2816 ve 2864 gibi modeller bu teknolojinin öncü örnekleridir.

### Flash Memory (Flash Hafıza) Özellikleri
 **EEPROM'dan Daha Yüksek Yoğunluk:** EEPROM'a göre fiziksel olarak daha küçük alanda çok daha fazla veri (GB’larca) saklayabilir.
 **Daha Hızlı Silme ve Yazma:** EEPROM’dan çok daha hızlı bir silme/yazma döngüsüne sahiptir.
 **İki Farklı Silme Modu:**
   **Bulk Erase (Toplu Silme):** Çipin içindeki tüm hücreleri tek seferde, tamamen temizler.
   **Sector Erase (Sektör Silme):** Belirli bir bölgeyi (örneğin 512 byte'lık bir bloğu) hedef alarak siler. Bu sayede tüm çipi sıfırlamadan sadece istediğin kısmı güncelleyebilirsin.
 **Yazma Süresi:** Tipik olarak hücre başına yaklaşık 10 mikrosaniye (10 usec) gibi çok kısa bir sürede yazma işlemini tamamlar.
 **Örnek:** 28F256A bu teknolojiye ait klasik bir modeldir.

---

## ROM Uygulama Alanları (ROM Applications)
 **Firmware (Donanım Yazılımı):** Bilgisayar veya elektronik cihaz açıldığı anda (power up) çalışan ilk verileri ve program kodlarını saklar. (Örneğin: BIOS).
 **Data Table (Veri Tablosu):** Sürekli ihtiyaç duyulan sabit verilerin hızlıca "bakılması" (look-up) için kullanılır.
   *Örnek:* İşlemcinin karmaşık hesaplamalarla uğraşmaması için trigonometrik tablolar (sinüs, kosinüs değerleri) doğrudan ROM'a önceden yazılır.
 **Data Converter (Veri Dönüştürücü):** Bir veri tipini başka bir tipe dönüştürmek için kullanılır.
   *Örnek:* Girdi olarak gelen BCD (Binary Coded Decimal) kodunu, ekranlarda sayıları gördüğümüz 7-segment koduna dönüştürerek rakamların ekranda belirmesini sağlar.

---

## ROM ve DAC ile Fonksiyon Jeneratörü

### 1. Adresleme Birimi (8-bit Counter)
 **Görevi:** Sistemin "zamanlayıcısıdır". Girişten gelen her saat darbesiyle (CLK) sayıyı 1 artırır (00→01→02⋯→FF).
 **Mühendislik Detayı:** Sayıcının hızı, çıkıştaki dalganın frekansını (hızını) belirler. Eğer sayıcıyı çok hızlı çalıştırırsan daha ince (yüksek frekanslı) bir ses veya sinyal alırsın.

### 2. Depolama Birimi (256 x 8 ROM)
 **Görevi:** Bir dalga formunun (Sinüs, Üçgen vb.) matematiksel örneklerini içinde barındıran bir "Look-up Table" (Bakma Tablosu) görevi görür.
 **Neden ROM?:** Çünkü sinüs fonksiyonu gibi karmaşık hesaplamaları anlık olarak işlemciye yaptırmak çok güç tüketir ve yavaştır. Önceden hesaplanmış veriyi ROM'dan okumak saniyede milyonlarca kez yapılabilir.
 **Kapasite:** 256×8 demek; dalganın 256 farklı noktasındaki yüksekliği, 8-bit (0-255 arası) hassasiyetle kaydedilmiş demektir.

### 3. Dönüştürme Birimi (8-bit DAC ve Filtre)
 **DAC (Digital-to-Analog Converter):** ROM'dan gelen dijital sayıları (biner kod) orantılı bir voltaj seviyesine dönüştürür.
 **RC Filtre (En sağdaki Direnç ve Kondansatör):** DAC çıkışı aslında merdiven basamağı gibidir (keskin köşelidir). Bu filtre, o köşeleri "yumuşatır" ve pürüzsüz bir analog dalga (Sinüs) elde edilmesini sağlar.

