<div align="center">
    
# 💾 RAM – Random Access Memory (Rastgele Erişimli Bellek)

**Okunabilir ve Yazılabilir (Read/Writeable):** ROM'un aksine, RAM'in içindeki veriler her an değiştirilebilir, silinebilir veya üzerine yeni veri yazılabilir.
**Uçucu (Volatile):** Elektrik kesildiği anda içindeki tüm veriler silinir. Bu yüzden kalıcı depolama değil, geçici depolama (temporary storage) veya register olarak kullanılır.

</div>

---

> [!NOTE]
> ## ⚙️ Teknolojisine Göre 2 Tür
> * **Static RAM (SRAM):** Yarı iletken tabanlıdır. Çok hızlıdır ama pahalıdır. Genellikle işlemci içindeki "Cache" (Önbellek) olarak kullanılır.
> * **Dynamic RAM (DRAM):** Kondansatör (Capacitor) tabanlıdır. Daha yavaştır ama ucuzdur ve yüksek kapasitelerde (8GB, 16GB RAM'ler gibi) üretilebilir.
> * **Boyut Adımları:** Kapasiteleri byte bazından başlar ve 1K, 2K, 4K gibi 2^n katları şeklinde artarak ilerler.

---

## 🛠️ RAM OPERASYON ADIMLARI

> [!TIP]
> ### 📥 Ortak Adımlar (Başlangıç)
> Hangi işlemi yaparsan yap, önce şu iki adımı tamamlaman gerekir:
> * **Adres Kodunu Ayarla (Set Address):** İşlemci, hangi hücreye ulaşmak istediğini "Adres Yolu"na (Address Bus) yazar.
> * **Çipi Aktifleştir (/CS):** "Chip Select" sinyali gönderilerek ilgili RAM çipi uyandırılır.

---

> [!WARNING]
> ### ✍️ Yazma Döngüsü (Write Cycle) Adımları
> * **Adres Kurulumu:** İşlemci, veriyi nereye yazacağını belirlemek için adres yoluna (Address inputs) bilgiyi gönderir. Diyagramda görülen tAS (Address Setup Time), adresin RAM tarafından algılanması için gereken minimum süredir.
> * **Yazma Sinyali (W/R):** Yazma moduna geçmek için sinyal düşük seviyeye çekilir. Bu, RAM'e "kapıları aç, veri geliyor" komutudur.
> * **Data Setup (tDS):** En önemli kısımdır. Verinin, yazma sinyali sonlanmadan önce veri yolunda hazır ve kararlı hale gelmesi gerekir. Eğer veri bu süreden daha kısa sürede yola çıkarsa, hücreye yanlış veya bozuk bilgi yazılır.
> * **Data Hold (tDH):** Yazma sinyali kesildikten (yüksek seviyeye çıktıktan) sonra verinin hat üzerinde ne kadar daha tutulması gerektiğini ifade eder. Bir nevi "kuruma süresi" gibi düşünebilirsin; sinyal biter bitmez veriyi hattan çekersen hücre içeriği tam dolmadan işlem yarım kalabilir.

---

> [!TIP]
> ### 📖 Okuma Döngüsü (Read Cycle) Süreci
> * **Adres Geçerliliği (Address Valid):** İşlemci, okumak istediği hücrenin adresini adres yoluna bırakır. Diyagramın en üstünde gördüğün tRC (Read Cycle Time), tüm bu okuma işleminin toplam süresidir.
> * **Okuma Komutu (R/W):** Okuma yapılacağı için bu sinyal genellikle yüksek (1) seviyede tutulur.
> * **Chip Select (CS) Aktivasyonu:** RAM birimine "seninle konuşuyorum" sinyali gönderilir (0'a çekilir). Bu andan itibaren zaman sayacı işlemeye başlar.
> * **Adres Erişim Süresi (tACC):** Bu, okuma döngüsünün en kritik gecikmesidir. Adres verildikten sonra RAM'in içindeki dekoderlerin doğru hücreyi bulup, oradaki veriyi çıkış kapısına (Data Output) iletmesi için gereken süredir.
> * **Data Valid (Veri Kararlı):** tACC süresi dolduğunda, veri çıkış hattındaki o belirsiz (High-Z) durum sona erer ve "Data valid" kutusu oluşur. Artık işlemci bu veriyi güvenle alıp kendi register'larına kopyalayabilir.

---

## 🏗️ DRAM Mimari Özeti

Görseldeki yapı, 16K x 1 bitlik bir DRAM'i temsil ediyor. Yani toplamda 16.384 adet hücre var ve her hücre sadece 1 bit saklayabiliyor.

> [!NOTE]
> ### 1. Satır ve Sütun Düzeni (128x128 Matris)
> Tüm bellek hücreleri tek bir uzun sıra yerine, 128 satır ve 128 sütun şeklinde bir kare ızgaraya yerleştirilmiştir.
> * **Neden?** Eğer 16.384 hücreyi yan yana dizseydin, her birine ulaşmak için binlerce kablo (pin) gerekecekti. Matris yapısı sayesinde sadece 128+128 hatla tüm hücrelere erişilebiliyor.

> [!NOTE]
> ### 2. Adres Kod Çözücüler (Decoders)
> Görselin solunda ve üstünde gördüğün bloklar bellek biriminin "navigasyon" sistemidir:
> * **Row Address Inputs (A0 - A6):** 7 bitlik bu adres, "1-of-128 decoder" aracılığıyla 128 satırdan hangisinin aktif edileceğini seçer.
> * **Column Address Inputs (A7 - A13):** Diğer 7 bitlik adres ise yukarıdaki decoder aracılığıyla hangi sütundaki verinin okunacağını belirler.
> * **Toplam Adres:** 7 bit (satır) + 7 bit (sütun) = 14 bit. 2^14=16.384 adres noktasına tam olarak denk gelir.

---

### 🧱 1. Görseldeki Bileşenlerin Görevleri
* **Kondansatör (C):** Hücrenin kalbidir. Veriyi elektrik yükü olarak saklar. Şarjlıysa "1", boşsa "0" demektir.
* **S (Switch) Anahtarları:** Veri akışını kontrol eden kapılardır (Sembolik olarak transistörleri temsil ederler).
* **Sense Amplifier (Algılama Yükselteci):** Kondansatördeki çok zayıf voltajı okur, VREF (Referans Voltajı) ile karşılaştırır ve kararlı bir dijital sinyale (1 veya 0) dönüştürür.
* **Data In / Out:** Verinin hücreye girdiği ve çıktığı yollardır.

### 🕹️ 2. Operasyonların Adım Adım İşleyişi

> [!TIP]
> ### Yazma İşlemi (WRITE)
> Veriyi içeri "hapsettiğimiz" süreçtir.
> * **S1 ve S2 Kapatılır:** Kapılar açılır.
> * **Şarj Etme:** DATA IN hattından gelen voltaj kondansatöre (C) akar.
> * **Kilitlenme:** Anahtarlar tekrar açıldığında, elektrik kondansatörde hapsolur. Veri artık fiziksel olarak oradadır.

> [!IMPORTANT]
> ### Okuma İşlemi (READ)
> Veriyi kontrol ettiğimiz ama aynı zamanda "tükettiğimiz" süreçtir.
> * **S2, S3 ve S4 Kapatılır (S1 hariç):** Kondansatördeki yük Sense Amplifier'a süzülür.
> * **Karşılaştırma:** Yükseltici, gelen yükü referansla kıyaslar ve DATA OUT'a net bir "1" veya "0" basar.
> * **Geri Yazma (Refresh):** Okuma sırasında kondansatördeki yük dışarı boşaldığı için veri kaybolur (Destructive Read). Bu yüzden S4 üzerinden gelen sinyal, kondansatörü anında tekrar şarj eder.

---

## 🔀 Adres Multiplexing

Normalde bellekteki bir veriye ulaşmak için bir adres göndermen gerekir. Bellek kapasitesi arttıkça, bu adresi taşımak için gereken kablo (pin/line) sayısı da artar. Örneğin, çok büyük bir belleğin varsa yüzlerce pin gerekirdi; bu da anakartta yer kaplar ve maliyeti artırır. Multiplexing ise bu işi "sırayla" yaparak pin sayısını yarıya indirir. Adresi tek seferde değil, iki aşamada göndeririz.

> [!NOTE]
> ### (A) Mimari Şema: Multiplexing Burada Nerede?
> Şemanın sol tarafına bakarsan, giriş pinlerinin A0/A7, A1/A8... şeklinde isimlendirildiğini görürsün. İşte Multiplexing tam olarak budur:
> * **Pin Tasarrufu:** Bu bellek 128x128=16.384 (16K) hücreden oluşuyor. Normalde her hücreye ulaşmak için 14 adet adres pinine (2^14=16.384) ihtiyacın olurdu. Ama şemada sadece 7 adet adres giriş hattı var.
> * **İki Aşamalı Giriş:** 14 bitlik adres, 7'şer bitlik iki paket halinde aynı hatlardan sırayla içeri alınır.
>   * **1. Aşama (Satır):** İlk 7 bit gelir, RAS sinyaliyle "Row Address Register" (Satır Adres Kaydedicisi) içine kilitlenir.
>   * **2. Aşama (Sütun):** İkinci 7 bit gelir, CAS sinyaliyle "Column Address Register" (Sütun Adres Kaydedicisi) içine kilitlenir.
> * **Decoder (Kod Çözücü):** Kaydedicilerdeki bu 7'şer bitlik veriler, "Decoder" birimleri tarafından çözülerek 128 satır ve 128 sütundan oluşan matriste tam koordinatı belirler.

> [!NOTE]
> ### (B) Zamanlama Diyagramı: Multiplexing Nasıl Yönetilir?
> Multiplexing işleminin "sırasını" bu diyagramdaki sinyaller belirler:
> * **Address Inputs:** Hatlara önce "Row Address" (Satır Adresi) verilir.
> * **RAS (Row Address Strobe):** Bu sinyal aşağı düştüğü an (t1), yonga "Tamam, şu an hatlarda duran 7 biti satır adresi olarak hafızama aldım" der.
> * **CAS (Column Address Strobe):** Hatlardaki veri "Column Address" (Sütun Adresi) ile değiştirildikten sonra bu sinyal aşağı düşer (t3). Yonga şimdi de sütun adresini kabul eder.

> [!WARNING]
> ### (A) Şeması: Multiplexing Yok (ROM veya SRAM)
> Burada işlemci (CPU), belleğe (ROM veya statik RAM) direkt bağlıdır.
> * **Mantık:** İşlemcinin 14 adet adres pini (A0 - A13) vardır ve belleğin de 14 adet giriş pini vardır.
> * **İlişki:** Adres direkt olarak, tek seferde gönderilir. Multiplexing yoktur.
> * **Sorun:** Bellek kapasitesi arttıkça pin sayısı binlere çıkardı. Bu da fiziksel olarak imkansız bir anakart tasarımı demek olurdu.

> [!TIP]
> ### (B) Şeması: Multiplexing Var (DRAM)
> İşte burası senin "Address Multiplexing" dediğin yerin tam olarak gerçekleştiği nokta. İşlemci ile DRAM arasına bir Multiplexer (MUX) yerleştirilmiş.
> * **Multiplexer'ın Rol
