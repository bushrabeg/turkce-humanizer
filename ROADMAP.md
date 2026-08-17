# ROADMAP — turkce-humanizer v3.0

**Tema:** Editör Diyaloğu ve Ölçülebilir Sonuç
**Hedef:** Skill sadece temizleyici değil, editör-yazar diyaloğu aracı olacak.
**Durum:** Tasarım aşaması. v2.1 canlı, v3.0 için karar alındı, kod yazımına başlanmadı.

---

## 1. Yazar Profili Sistemi

### Ne
Kullanıcının kendi yazım tarzını skill'e tanıtabildiği opsiyonel bir katman. Skill profil-uyumlu bir metin gördüğünde, kullanıcının imzasını (kavram üretme refleksi, ritim, tercih edilen kalıplar) tanıyıp korur.

### Neden
v2.1 register'ı biliyor ama yazar sesini bilmiyor. "Masada kazanılan egemenlik" gibi kişisel imzalar bazen AI-sinyali sanılıp temizlenebiliyor. Profil sistemi bu ayrımı yapar.

### Nasıl çalışır

**Kurulum:** Kullanıcı tek mesajda üç-beş örnek metnini paylaşır ("bu metinler benim profilim olsun" der). Claude profili oluşturup dosyaya yazar. Kullanıcı YAML görmez, dosya yolu bilmez, klasör açmaz.

**Format:** Profil dosyası ham metinden ibarettir — kullanıcının yapıştırdığı örnekler + Claude'un çıkarımları (nefes uzunluğu, sık kullanılan kalıplar, korunacak imzalar) yorum olarak eklenir. İnsan-okunabilir, elle düzenlenebilir.

**Konum:** `~/.claude/skills/turkce-humanizer/profiles/ben.md` (varsayılan). Kullanıcı isterse birden fazla profil oluşturabilir.

**Tetiklenme mantığı:**

Kullanıcı bir metin atar, "humanize et" der. Skill önce register'ı belirler.

- **Register profil-uyumsuz** (hukuki, kurumsal): Skill hiç sormadan işi yapar. Profil konusu açılmaz.

- **Register profil-uyumlu** (analiz, deneme, gazetecilik, edebi): Skill profil dosyasını kontrol eder.
  - Profil dosyası varsa: kullanır, işi yapar.
  - Profil dosyası yoksa: sorar → "Bu metin profil uygulamaya uygun bir register'da. Senin ritmini katmamı istersen kendi yazdığın bir metni paylaş, profilini oluşturayım. İstemiyorsan devam edebilirim."
    - Kullanıcı örnek yapıştırırsa: profil oluşur, aynı çalıştırmada humanize yapılır.
    - Kullanıcı "istemiyorum" derse: profilsiz humanize yapılır.

**Tercih hafızası:** "İstemiyorum" kararı sadece o sohbete özel. Yeni sohbette baştan sorulur. Aynı sohbette tekrar sorulmaz.

### Karar gerekçesi
Opt-in kullanıcı tarafında (dosya oluşturmak zorunda değil), opt-in skill tarafında (register uygun değilse sormaz), opt-in her sohbette (kullanıcı o günkü işine göre karar verir). Üç katmanlı bu yapı skill'i ısrarcı olmaktan çıkarır, teklif eden konuma alır.

---

## 2. Kullanıcıya Danışma Protokolü (Formalize)

### Ne
Skill kendi kararının sınırında olduğu her yerde kullanıcıya açık soru sorar. v2.1'de spontan yaptığı davranış v3.0'da SKILL.md'de tanımlı olacak.

### Neden
v2.1 sınırda kararlar için ("Ne var ki" bağlacı gibi) doğal olarak sormaya başladı. Bu davranış formalize edilmezse tesadüfe kalır, her sohbette güvenilir çalışmaz.

### Nasıl çalışır
Skill üç durumda kullanıcıya danışır:

- Bir sinyalin AI-imzası mı yoksa yazar tercihi mi olduğu belirsiz olduğunda.
- Bir bağlaç veya kalıp register'da sınırda olduğunda ("bu register'da [A] mı [B] mı tercih edersin?").
- Metonim ile eş anlamlı rotasyonu ayrımı belirsiz olduğunda.

Sorular kısa, açık, tek seçim. Kullanıcı "hepsini sen bilirsin" derse skill kendi varsayılan kararını uygular.

### Karar gerekçesi
Editörlük kararlarının bir kısmı algoritmik değil, bağlamsaldır. Skill bunu kabul edip kullanıcıya devretmeli, tahmin etmemeli.

---

## 3. Değişiklik Özeti ve Geri Alma

### Ne
Skill her müdahalesini işaretler. Sonda kullanıcıya değişiklik listesi sunar, kullanıcı tek tek onaylar veya reddeder.

### Neden
v2.1'de skill toplu değişiklik yapıyor, kullanıcı ya hepsini kabul ediyor ya baştan yazıyor. Ara yol yok. Bu editörün kontrolünü kısıtlıyor.

### Nasıl çalışır
Skill humanize sonrası çıktıyı iki bölüm halinde sunar:

- **Metin:** Değişiklikler işaretlenmiş halde (mesela satır numarasıyla).
- **Değişiklik listesi:** "1. [X] → [Y] (Sinyal 7, em dash overuse). 2. [A] → [B] (Sinyal 15, cümle patlaması)..."

Kullanıcı ya "hepsi tamam" der, ya "1, 3, 5'i uygula, gerisini bırak" der. Skill final metni ona göre üretir.

### Karar gerekçesi
Editör her kararı görmek ister. Toplu kabul/red editörü kör bırakır. Tek tek onay editörü tekrar merkeze koyar.

---

## 4. Kabul Oranı Metriği

### Ne
Kullanıcının onayladığı/reddettiği değişiklikler bir log dosyasına yazılır. Zamanla "hangi sinyal hangi register'da ne kadar kabul görüyor" verisi birikir.

### Neden
Sinyal/100 kelime metriği kaldırma metriği, kalite metriği değil. Kabul oranı skill'in gerçek başarısını ölçer. Ayrıca kalibrasyon için veri sağlar — hangi sinyal aşırı hassas, hangisi yetersiz görünür hale gelir.

### Nasıl çalışır
Her değişiklik özeti sonrası kullanıcının kararları `~/.claude/skills/turkce-humanizer/feedback.jsonl` dosyasına yazılır. Her satır bir değişiklik: sinyal, register, öneri, kullanıcı kararı (kabul/red).

Kullanıcı istediğinde `turkce-humanizer stats` benzeri bir komutla özet görür ("Sinyal 7 akademik-kurumsal'da %73 kabul, deneme-blog'da %31 kabul" gibi). İstemezse dosya arka planda birikir, ileride kalibrasyon için kullanılır.

### Karar gerekçesi
Şeffaflık. Skill'in kendi başarısını ölçebilmesi. Kullanıcının da skill'i eğitebildiğini hissetmesi.

---

## 5. Kalibrasyon Örnekleri Klasörü

### Ne
Repo'da `examples/` klasörü. Her sinyal için before/after çiftleri.

### Neden
İki fayda: (1) Skill kalibrasyonu için referans zemin — yeni sinyal eklenirken ya da mevcut sinyal düzenlenirken burası test korpüsü olur. (2) Topluluk katkısına açık zemin — başka kullanıcılar kendi before/after örneklerini pull request ile ekleyebilir.

### Nasıl çalışır
`examples/sinyal-07-em-dash.md` gibi dosyalar. Her dosya bir sinyal için 3-5 before/after çifti içerir, register etiketiyle. Skill kendi kararını verirken bunlara referans olarak bakabilir (opsiyonel, hız için varsayılan olarak kapalı).

### Karar gerekçesi
Skill kararları örneksiz kaldığında soyut kalır. Örnekler kararları somutlaştırır, hem kullanıcı için hem skill için.

---

## 6. Detektör Test Seti

### Ne
GPTZero, Originality.ai, Copyleaks gibi AI-detektörlerin Türkçe modu ile before/after karşılaştırma.

### Neden
Can Yılmaz'ın Twitter sorusuna cevap. Skill'in gerçek dünya detektör direncini ölçmek. Birincil başarı metriği değil, ikincil doğrulama.

### Nasıl çalışır
Belirli sayıda AI-Türkçesi paragrafı → detektör skoru → skill uygulaması → detektör skoru. Karşılaştırma tablosu repo'da tutulur, periyodik güncellenir.

### Karar gerekçesi
Skill'in birincil hedefi detektörü kandırmak değil, insan editör için okunabilir metin üretmek. Ama detektör skorlarını da göstermek şeffaflık için önemli — kullanıcı hem editör kalitesini hem detektör direncini bilerek karar verir.

### Uyarı
Detektör skorlarını *hedef* haline getirmemek gerek. Skill detektörü optimize eder hale gelirse editör kalitesi bozulabilir (Goodhart yasası). Detektör skoru rapor edilir, optimize edilmez.

---

## 7. Benchmark Corpus

### Ne
Standart bir Türkçe AI-metinleri korpüsü. Her sürümde aynı korpus üzerinde skill çalıştırılır, sonuçlar repo'da otomatik raporlanır.

### Neden
Sürümler arası ilerlemeyi ölçmek. v2.1 → v3.0 gerçekten iyileşme mi getirdi, hangi register'da ne kadar? Sayısal kanıt olmadan bu subjektif kalır.

### Nasıl çalışır
`benchmark/` klasöründe her register'dan 10-20 paragraflık AI-üretimi metin. Skill sürüm çıkışlarında bu korpus üzerinde çalıştırılır. Sonuç: sinyal/100 kelime, kabul oranı (elle etiketlenmiş "iyi/kötü" karşılaştırma), detektör skoru. `benchmark/results/vX.Y.md` altında raporlanır.

### Karar gerekçesi
Şeffaflık ve reproducibility. Preprint için de gerekli (bir sonraki adım, v3.1+).

---

## Mimari Kararlar (v2.1'den taşınan, değişmeyecek)

- İki fazlı mimari (Faz 1 koşulsuz temizlik, Faz 2 koşullu enjeksiyon)
- 5 register kategorisi (hukuki-idari, akademik-kurumsal, analitik-gazetecilik, deneme-blog, edebi-yaratıcı)
- Sinyal hiyerarşisi (baskın + ikincil)
- TDK-referanslı noktalama katmanı
- Atlanamaz çıktı formatı (rapor + metrik + versiyon + notlar)
- MIT lisansı
- Yapı koruma prensibi (madde listeleri liste kalır)

## v3.0 Kapsamı Dışında Bırakılanlar

Şunlar v3.0'a girmez, sonraki sürümlerde ya da paralel projeler olarak ele alınır:

- **Register sınıflandırıcı ayrı skill** (`turkce-register-detector`): v3.1 veya bağımsız repo. v3.0 kendi register mantığıyla çıkar.
- **Preprint (SSRN veya arXiv)**: v3.0 release + benchmark verisi hazır olduktan sonra ayrı iş.
- **Kullanım sırasında ilk sefer profil önerisi**: v3.1'de düşünülecek, kalibrasyon dikkat ister.

## Sürüm Sonrası Duyuru

v3.0 büyük duyuru olacak. Kimlik: "Editör Diyaloğu ve Ölçülebilir Sonuç." Twitter thread + LinkedIn post + Teknopolitika bültenine gidecek metin. Yalçın Soysal, Can Yılmaz, Alper Zorlu'nun v2.1 dönemi Twitter sorularına toplu cevap niteliği taşır.
