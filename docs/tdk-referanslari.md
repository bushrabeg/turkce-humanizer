# TDK Referansları

Bu dosya turkce-humanizer skill'inin noktalama yasaklarının TDK karşısındaki konumunu açıklar. Skill bu dosyayı okumak zorunda değil — mutlak yasaklar SKILL.md'de tanımlı. Bu dosya insan referansı içindir: katkı yapmak isteyen biri, "neden bu yasak" sorusuna cevap arayan biri, ya da yasağın TDK ile ilişkisini merak eden biri için.

## Skill'in TDK ile Üç Farklı İlişkisi

Skill TDK Yazım Kuralları'nı temel alır ama üç farklı ilişki taşır:

**1. TDK'dan katı olduğu yerler.** TDK'nın izin verdiği bazı yapıları skill yasaklıyor. Sebep: bu yapılar TDK'ya uygun olsa bile Türkçe AI çıktısının belirgin imzalarıdır. Örnek: noktalı virgül, uzun çizgi.

**2. TDK ile aynı çizgide olduğu yerler.** TDK'nın da yanlış saydığı ama AI'nin yaygın olarak ürettiği yapıları skill yasaklıyor. Örnek: karşıtlık bağlaçları öncesi virgül.

**3. TDK'nın belirsiz bıraktığı yerler.** TDK'nın açık bir kuralı olmadığı ama Türkçe yazı geleneğinin belirli bir tercihi olduğu yerlerde skill geleneği takip eder. Örnek: bağlaçla açılan izole cümle yasağı — TDK'da açık kural yok, ama Türk edebiyat ve akademi geleneğinde bu tür cümleler estetik olarak kabul görmez.

## Mutlak Yasak 1 — Uzun Çizgi

### TDK'nın kuralı

TDK Yazım Kuralları uzun çizgiye (—) sınırlı bir alan tanır. Ana kullanımı diyalog başındadır:

> — Eski şehri gezdin mi?
> — Evet, dün akşam.

Ayrıca satır sonunda kelime bölme durumunda ve bazı basılı yayınlarda cümle içi ara sözü belirtmek için de görülür.

### Skill'in duruşu

Skill uzun çizgiyi *tamamen* yasaklar. Diyalog başında bile kullanmaz — bu skill düz metin ürettiği için diyalog formatı zaten çıktısında yer almaz.

### Neden yasak

Türkçe AI çıktısı uzun çizgiyi İngilizce "em dash" mantığıyla kullanır — cümle içinde ara söz belirtmek için. Bu Türkçe'nin doğal noktalama refleksi değildir. Türkçe ara söz için ya virgül kullanır ya iki nokta üst üste ya da cümleyi böler. Uzun çizgi geldiğinde metin İngilizce çevirisi hissi verir.

## Mutlak Yasak 2 — Noktalı Virgül

### TDK'nın kuralı

TDK noktalı virgüle üç izinli kullanım tanır:

1. **Cümle içinde virgüllerle ayrılmış öbekleri birbirinden ayırmak için:**
   > "Aynı zamanda; hukukun uygulanması, adaletin sağlanması ve toplum düzeninin korunması bu ilkeye bağlıdır."

2. **Ögeleri arasında virgül bulunan sıralı cümleleri birbirinden ayırmak için:**
   > "At ölür, meydan kalır; yiğit ölür, şan kalır."

3. **İkiden fazla eş değer öge arasında virgülle sıralanmış cümlenin son ögesini vurgulamak için:**
   > "Ordu, kalabalık; halk, sessiz; şehir, karanlıktı."

### Skill'in duruşu

Skill noktalı virgülü *koşulsuz* yasaklar. TDK'nın üç izinli kullanımının hiçbiri geçerli değildir. Noktalı virgül gördüğü her yerde cümleyi ikiye böler (bağlaçla açılan izole cümle üretmeden).

### Neden yasak

İki sebep var:

**Birincisi**, noktalı virgül modern Türkçe düz yazısında son yıllarda neredeyse kaybolmuştur. Tanpınar, Ayfer Tunç, Barış Bıçakçı, İlber Ortaylı gibi referans yazarlar noktalı virgül kullanımından büyük ölçüde uzak durur. Bir metinde noktalı virgül görmek okuru "bu metin ya çok eski ya AI" izlenimine götürür.

**İkincisi**, AI Türkçe çıktısı noktalı virgülü İngilizce "; also / ; however / ; therefore" mantığıyla kullanır. Yani AI aslında İngilizce'nin sözdizim mantığını Türkçe'ye taşırken noktalı virgülü bağlaç yerine kullanıyor. Bu Türkçe'nin doğal ritmini bozar.

## Mutlak Yasak 3 — Yarım Cümle

### TDK'nın kuralı

TDK'nın "yarım cümle" ya da "fragment cümle" konusunda açık bir yasağı yoktur. Cümle tanımı özne ve yüklem üzerinden yapılır ama üslup meselesi olarak fragment cümleye izin verilir.

### Skill'in duruşu

Skill fragment cümle üretmez. Kısa cümle üretebilir ama tam cümle olarak — özne ve yüklem taşıyacak şekilde.

### Neden yasak

AI Türkçe çıktısı fragment cümleyi "vurgu aracı" olarak kullanır: "Bitti." "Kalakaldı." "Yeni bir dönem." Bu İngilizce copywriting geleneğinden gelir (özellikle marketing metinleri). Türkçe edebiyat ve akademi geleneğinde bu tarz fragment cümle nadirdir — Tanpınarvari bilinçli asılı bitirme dışında pek görülmez.

Kullanıcı bilinçli edebi tercihle fragment yazmak isterse skill'i kullanmaması gerekir. Skill'in görevi AI-imzasını temizlemek — ve fragment cümle Türkçe AI çıktısının belirgin imzalarından biri olduğu için koşulsuz yasak.

## Mutlak Yasak 4 — Bağlaçla Açılan İzole Cümle

### TDK'nın kuralı

TDK'da açık bir yasak yok. "Ama," "ancak," "fakat" gibi bağlaçların cümle başında kullanımı gramer olarak yanlış sayılmıyor.

### Skill'in duruşu

Skill "Ama şu oldu." "Ancak durum farklıdır." tarzı bağlaçla açılan izole cümleler üretmez. Bağlaç ya cümlenin içinde kalır ya iki cümle birleştirilir.

### Neden yasak

Bu skill'in TDK'yla çatıştığı değil, geleneği takip ettiği bir yasaktır. Türk edebiyat ve akademi geleneği bağlaçla açılan izole cümleyi estetik olarak zayıf sayar. Karşıtlık bağı zaten iki cümle arasında var — bağlacı ikinci cümlenin başına koyup birinciyle bağı koparmak retorik olarak yumuşamadır.

Referans yazarlar (Tanpınar, Ortaylı, Karpat, Keyder) bağlaçları neredeyse hep cümle içinde tutar. AI Türkçe çıktısı ise İngilizce "But..." "However..." cümle-başı bağlaç geleneğini taşıdığı için sık sık izole bağlaç açar.

## Mutlak Yasak 5 — Karşıtlık Bağlaçları Öncesi Virgül

### TDK'nın kuralı

TDK bu konuda skill ile aynı çizgidedir. "Ama," "ancak," "fakat," "lakin" gibi karşıtlık bağlaçlarından önce virgül konulmaz. Bu TDK Yazım Kuralları'nda açıkça belirtilir.

Doğru:
> "Türkiye reform yaptı ama sonuç sınırlı kaldı."

Yanlış:
> "Türkiye reform yaptı, ama sonuç sınırlı kaldı."

### Skill'in duruşu

TDK ile aynı — bağlaç öncesindeki virgülü siler.

### Neden yasak

İngilizce'de "but" ve "however" öncesi zorunlu virgül kuralı vardır ("...worked, but...", "...worked; however,..."). AI bu kuralı Türkçe'ye zorla çevirir. Sonuç: TDK'ya göre yanlış, Türkçe okur için hissedilir bir sözdizim bozukluğu, AI çıktısının en belirgin imzalarından biri.

Bu yasak "TDK'dan katı" değildir, TDK ile aynı çizgidedir. Skill sadece AI'nin yaptığı yanlışı düzeltir.

## İzinli Sayılan İki Nokta Üst Üste Kullanımları

Skill iki nokta üst üsteyi tamamen yasaklamaz — TDK'nın izin verdiği iki kullanım geçerlidir:

**1. Örneklendirme ya da açıklama gerektiren cümle sonunda:**
> "Üç ana referans yazar var: Tanpınar, Tunç, Bıçakçı."

**2. Konuşma öncesinde:**
> "Editör dedi ki: bu bölüm baştan yazılmalı."

Skill'in yasakladığı kullanım cümle-içi mini açıklama modu: "Bu durum: yeni bir dönemin başlangıcı." tarzı fragment tanımlar. Bunlar Sinyal 1c altında tanımlıdır.

## Kaynak

Türk Dil Kurumu, *Yazım Kuralları*. Erişim: [tdk.gov.tr](https://www.tdk.gov.tr).

Bu dosya TDK Yazım Kuralları'nın turkce-humanizer skill'inin kararları açısından incelemesidir. Kapsamlı bir noktalama rehberi değildir.
