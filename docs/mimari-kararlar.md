# Mimari Kararlar

Bu dosya turkce-humanizer skill'inin temel tasarım kararlarının gerekçelerini açıklar. SKILL.md skill'in ne yaptığını söyler; bu dosya neden öyle yapıldığını söyler.

## Karar 1 — İki Fazlı Mimari

### Ne

Skill iki ayrı fazda çalışır. Faz 1 metinden AI-üretimi imzaları çıkarır (koşulsuz, her metne uygulanır). Faz 2 metnin registeri uygunsa insan Türkçesinin ritim imzalarını yerleştirir (koşullu).

### Alternatif ne olabilirdi

Tek fazlı bir mimari düşünülebilirdi: her sinyalin hem "çıkar" hem "yerleştir" davranışı olabilir, ikisi bir arada çalıştırılabilirdi. Ya da tek yönlü bir mimari: sadece çıkarma, hiç enjeksiyon yok.

### Neden iki fazlı seçildi

Ayrılığın üç faydası var:

**Birincisi**, "çıkarma" evrenseldir, "enjekte etme" bağlama bağlıdır. Bir KVKK metnine konuşma bağlacı enjekte edilmez ama noktalı virgül yine de çıkarılır. Bu iki iş farklı mantık taşıyor — birleştirmek register kontrolünü karmaşıklaştırıyor.

**İkincisi**, iki fazlı mimari LLM tarafında maker/checker deseni kurmayı kolaylaştırıyor. Faz 1'in çıktısı Faz 2'nin girdisidir. Skill kendi çıktısına ikinci bir gözle bakabiliyor.

**Üçüncüsü**, kullanıcı isterse sadece Faz 1'i çalıştırabilir ("AI kokusunu at, ama fazla enjekte etme"). Bu esneklik tek fazlı mimaride imkansızdı.

### Değişebilir mi

Bu karar v3.0'da değişmez. Skill'in temel iskeleti bu.

## Karar 2 — Beş Register Kategorisi

### Ne

Skill metinleri beş register kategorisine ayırır: hukuki-idari, akademik-kurumsal, analitik-gazetecilik, deneme-blog, edebi-yaratıcı.

### Alternatif ne olabilirdi

Daha az kategori (üç: resmi, yarı-resmi, gayri-resmi) düşünülebilirdi. Ya da daha fazla (yedi-sekiz: her tür için ayrı). Ya da spektrum yaklaşımı: register bir eksende sürekli değer, ayrık kategori değil.

### Neden beş seçildi

**Az kategori** (üç) hukuki metinle akademik metni aynı kutuya koyar. Ama bu ikisinin Faz 2 uygulaması farklıdır — akademikte birinci çoğul geçer, hukuki metinde geçmez. Ayrılık gerekli.

**Çok kategori** (yedi-sekiz) skill'i pratikte kullanılmaz hale getirir. Kullanıcı "bu neyin registerinde" diye kararsız kalır, skill teşhiste yanılır.

**Spektrum yaklaşımı** teorik olarak daha zarif ama uygulamada Faz 2 sinyallerinin devreye girme mantığını flulaştırır. Her sinyalin "hangi eşikte devreye girer" kararı sürekli hesaplama gerektirir. Ayrık kategori net karar üretir.

Beş kategori Türkçe yazı geleneğinin doğal ayrımlarına da denk düşüyor: kanun / tez / analiz / deneme / roman. Türk okur bu ayrımı zaten sezgisel yapıyor.

### Değişebilir mi

Kategori sayısı değişmez ama sınır durumlar zamanla netleşebilir. `docs/register-detayli.md` yaşayan bir belge.

## Karar 3 — Mutlak Yasak Katmanı

### Ne

Beş yasak (uzun çizgi, noktalı virgül, yarım cümle, bağlaçla açılan izole cümle, karşıtlık bağlaçları öncesi virgül) register-bağımsız uygulanır. Faz 1'den önce, Faz 2'den önce, her şeyden önce.

### Alternatif ne olabilirdi

Bu yasakları da sinyal olarak koyabilirdik — alarm eşiği ve register kısıtı ile. Örneğin: "noktalı virgül akademik-kurumsalda kabul edilebilir, deneme-blogda alarm."

### Neden ayrı katman

**Birincisi**, bu yasaklar müzakere kabul etmez. Skill her koşulda uygular. Sinyal olarak koymak "belki bu registerde tolerans var" tartışması yaratıyordu. Ayrı katmana çıkarmak bu tartışmayı bitirdi.

**İkincisi**, mutlak yasaklar skill'in kimliğini belirliyor. Bu skill "Türkçe olsun" değil, "AI Türkçesi olmasın" der. Beş yasak bu kimliğin somut ifadesi.

**Üçüncüsü**, tasarım netliği açısından okuyucuya (kullanıcı, katkı yapan) "bu skill'in çıktısında asla bulunmayacak şeyler" listesi tek bir yerde görünür. Karışıklık yok.

### Değişebilir mi

Yeni bir yasak eklenebilir (mesela v3.1'de altıncı yasak ortaya çıkabilir). Ama mevcut beş yasaktan biri kaldırılmaz — bu skill'in temel kimliği.

## Karar 4 — Sinyal Hiyerarşisi (Baskın + İkincil)

### Ne

Skill sinyalleri iki grupta topluyor: baskın sinyaller (5 tane, Sinyal 1-5) ve ikincil sinyaller (Sinyal 6-14). Baskın sinyaller tek başına AI-imzası kanıtı. İkincil sinyaller destekleyici — baskınlarla birlikte görüldüğünde şüpheyi güçlendirir.

### Alternatif ne olabilirdi

Bütün sinyaller eşit ağırlıkta olabilirdi. Ya da her sinyal için ayrı bir güven skoru olabilirdi (0-100 arası).

### Neden hiyerarşi

**Birincisi**, gerçek AI çıktısında bazı sinyaller çok daha diagnostik. Uzun tire ve noktalı virgül görmek metnin AI olduğunu neredeyse kanıtlıyor. Ama pasif çatı bağımlılığı tek başına AI kanıtı değil — insan yazarlar da pasif kullanır. Bu farkı mimarinin tanıması lazım.

**İkincisi**, hiyerarşi kullanıcıya karar sunmayı kolaylaştırıyor. "Bu paragrafta üç baskın sinyal var" demek "on iki farklı sinyalden bazıları var" demekten net.

**Üçüncüsü**, güven skoru yaklaşımı skill'i sürekli hesaplama makinesine çeviriyor. Basit ikili ayrım karar hızını artırıyor.

### Değişebilir mi

Sinyal seviyeleri (baskın/ikincil) değişebilir. Bir sinyal zamanla baskın hale gelirse ya da ikincile inerse yeniden ayarlanır.

## Karar 5 — TDK Referanslı Noktalama

### Ne

Skill Türkçe noktalama kararlarında TDK Yazım Kuralları'nı temel alır. Ama TDK'nın izin verdiği bazı yapıları yasaklar (uzun çizgi, noktalı virgül), TDK'nın yanlış saydığı bazı yapıları da yasaklar (karşıtlık bağlaçları öncesi virgül).

### Alternatif ne olabilirdi

TDK'yı hiç referans almamak — modern Türkçe düz yazı geleneğini doğrudan izlemek. Ya da TDK'yı harfi harfine izlemek — TDK ne izin veriyorsa hepsi geçerli.

### Neden bu karma yaklaşım

**Birincisi**, TDK Türkçe yazının otoritesi. Ondan kopmak skill'i keyfi hale getirir. Kullanıcı "neden bu yasak" diye sorduğunda referans göstermek gerekiyor.

**İkincisi**, TDK bazı yapılara izin veriyor ama modern Türkçe düz yazı geleneği bu yapıları terk etmiş (noktalı virgül gibi). Skill bu geleneği takip ediyor, TDK'yı değil.

**Üçüncüsü**, TDK'nın yanlış saydığı bazı yapılar AI'nin yaygın olarak ürettiği yapılar. Bu durumda skill TDK'yla aynı çizgide.

Sonuç: skill TDK'nın altkümesi de değil, üstkümesi de değil. Bazı yerlerde katı, bazı yerlerde aynı çizgide.

### Değişebilir mi

TDK Yazım Kuralları güncellenirse skill de gözden geçirilir. Ama modern Türkçe düz yazı geleneği referansı sabit kalır.

## Karar 6 — ASD-STE100 Esinli Nicel Metrik

### Ne

Skill sinyal yoğunluğunu 100 kelimeye normalize ederek ölçer. "Önce: 8.4 sinyal/100 kelime. Sonra: 1.2. İyileşme: %86."

### Alternatif ne olabilirdi

Yüzde bazlı ölçüm ("kelimelerin %X'ine dokunuldu"). Ya da kalitatif değerlendirme (yüksek/orta/düşük AI-imzalı). Ya da hiç ölçüm — sadece rapor.

### Neden ASD-STE100 mantığı

ASD-STE100 (Aerospace and Defence Simplified Technical English, 1986) havacılık sektöründe kullanılan kısıtlı bir yazım standardı. Bu standardın bir özelliği "kural ihlali puanı" — belge kaç kuralı ne kadar ihlal ediyor, sayısal olarak ölçülüyor.

Bu mantığı Türkçe humanizer'a getirmenin üç faydası var:

**Birincisi**, iyileşme sayısal olarak ifade edilebiliyor. Kullanıcı skill'in ne kadar iş yaptığını görüyor.

**İkincisi**, metrik sürüm karşılaştırması için kullanılabilir. v2.1 ve v3.0'ın aynı metin üzerindeki sinyal yoğunluğu karşılaştırılabilir.

**Üçüncüsü**, benchmark korpus için altyapı hazır. Standart korpus üzerinde skill'in her sürümü sayısal skor üretir.

### Uyarı

Metrik hedef haline getirilmemeli. Skill sinyal yoğunluğunu düşürmek için editör kalitesinden ödün vermez. Metrik rapor edilir, optimize edilmez.

### Değişebilir mi

Metriğe ek olarak "kabul oranı" metriği v3.0'da devrededir (kullanıcının önerilen değişiklikleri kabul etme yüzdesi). İleride başka metrikler eklenebilir.

## Karar 7 — Yapı Koruma Prensibi

### Ne

Skill belge iskeletine dokunmaz. Madde listeleri liste kalır, başlıklar başlık kalır, tablolar tablo kalır. Cümle içi ve paragraf içi dil düzeltmesi yapar.

### Alternatif ne olabilirdi

Skill yapıya da dokunabilirdi. "Bu liste paragraf olarak daha iyi akar" diye yeniden yapılandırabilirdi.

### Neden yapı korunuyor

Yapı içeriktir, süs değil. Bir kitap taslağında madde listesi bilinçli bir editör kararı — okur o formattan yararlanacak. Skill bu kararı bilmiyor. Yapıya dokunmak kullanıcının işini bozar.

Kullanıcı açıkça "yapıyı da değiştir" derse skill değiştirir. Ama varsayılan davranış "dokunma"dır.

### Değişebilir mi

Bu prensip sabit. v2.1'de eklendi, v3.0'da korunuyor.

## Karar 8 — Yazar Profil Sistemi (v3.0 Eki)

### Ne

Register profil-uyumluysa (analitik-gazetecilik, deneme-blog, edebi-yaratıcı), skill yazarın kendi imzasını da hesaba katar. Profil dosyası kullanıcının yapıştırdığı üç ile beş örnek metin.

### Alternatif ne olabilirdi

Profil sistemi hiç olmayabilirdi (v2.1'de yoktu). Ya da profil zorunlu olabilirdi (her kullanıcı önce profil oluşturmalı). Ya da profil detaylı forma dayanabilirdi (yaş, meslek, stil tercihleri).

### Neden opsiyonel örnek-tabanlı profil

**Birincisi**, zorunlu profil skill'in giriş engelini yükseltir. Kullanıcı "önce ödev yap sonra çalıştır" durumundan kaçar. Skill kullanılmaz olur.

**İkincisi**, form-tabanlı profil zaman ister ve kullanıcı hangi alanın önemli olduğunu bilmez. Örnek yapıştırmak zahmetsiz.

**Üçüncüsü**, "eski yazın nasıldı" sorusu kişiselleştirme için doğal bir soru. Kullanıcı YAML görmez, alan doldurmaz, sadece kendi metnini paylaşır.

### Değişebilir mi

Profil formatı zenginleşebilir (mesela "kaçınılacak kalıplar" listesi eklenebilir). Ama örnek-tabanlı temel yapı sabit.

## Karar 9 — Kullanıcıya Danışma Protokolü (v3.0 Eki)

### Ne

Skill kendi kararının sınırında olduğu her yerde kullanıcıya soru sorar. Metonim ayrımı, sınır bağlaçları, belirsiz register durumları için.

### Alternatif ne olabilirdi

Skill her zaman kendi kararını verebilirdi (danışma yok). Ya da her sinyal için danışabilirdi (aşırı danışma).

### Neden orta yol

**Birincisi**, aşırı danışma kullanıcıyı yorar. Skill her adımda "bunu yapayım mı" sorarsa kullanıcı skill'i kullanmaz.

**İkincisi**, hiç danışmama skill'i keyfi yapar. Sınırda kararlarda yazar-özgü tercih AI-imzası sanılıp temizlenir.

**Üçüncüsü**, v2.1 zaten kendi kararının sınırında olduğu yerlerde doğal olarak sormaya başladı. Bu emergent davranışı v3.0'da formalize etmek doğru yol.

### Değişebilir mi

Danışma tetikleyicileri (hangi durumlarda sorulur) zamanla netleşir. Ama danışma prensibi sabit.

## Karar 10 — Değişiklik Özeti ve Onay (v3.0 Eki)

### Ne

Skill her müdahalesini işaretler. Sonda kullanıcıya değişiklik listesi sunar, kullanıcı tek tek onaylar veya reddeder.

### Alternatif ne olabilirdi

Skill toplu değişiklik yapabilirdi (v2.1 böyleydi). Ya da hiç değişiklik yapmadan sadece önerebilirdi.

### Neden tek tek onay

**Birincisi**, toplu değişiklik editörü kör bırakır. Kullanıcı ya hepsini kabul eder ya baştan yazar — ara yol yok.

**İkincisi**, tek tek onay editörü tekrar merkeze koyar. Skill öneri makinesi, editör karar makinesi.

**Üçüncüsü**, kabul/red kararları log'a yazılır (feedback.jsonl). Bu kalibrasyon için veri sağlar.

### Değişebilir mi

Onay formatı zenginleşebilir (batch onay, kategori bazlı onay). Ama tek tek onay prensibi sabit.

## Kaynak ve İlham

- **ASD-STE100** (1986): Simplified Technical English standardı. Nicel metrik ilhamı.
- **harshaneel/humanize**: İngilizce humanizer projesi. Genel mimari ilhamı.
- **makotofalcon/humanizer-ja**: Japonca humanizer projesi. Register-farkındalık ilhamı.
- **Türk edebiyat ve akademi geleneği**: Ritim imzaları için stilometrik referans.

turkce-humanizer bu ilhamları Türkçe'nin kendi AI-imzası taksonomisi üzerine kurgular. İngilizce şablonun çevirisi değildir. Türkçe'nin eklemeli yapısı, serbest dizilimi, register salınımı ve TDK-referanslı noktalama kültürü mimariye yansımıştır.
