---
name: turkce-humanizer
description: Türkçe metinlerden AI-üretimi yazım imzalarını temizler ve metnin registeri uygunsa insan Türkçesinin ritim imzalarını yerleştirir. "Türkçeleştir," "AI kokusunu at," "bu Türkçe metni doğallaştır," "humanize et" gibi ifadelerle veya AI-üretimi görünen Türkçe paragraf sunulduğunda tetiklenir.
---

# Türkçe Humanizer

Bu skill iki fazlı çalışır. **Faz 1** metinden AI-üretimi yazım imzalarını çıkarır — koşulsuz, her metne uygulanır. **Faz 2** metnin registeri uygunsa insan Türkçesinin ritim imzalarını yerleştirir — koşullu, metnin türüne göre değişir.

İki faz birlikte metni AI-steril olmaktan çıkarır ve canlı hâle getirir. Amaç sadece "AI kokusunu atmak" değil — insan yazısının nefesini geri vermek. Ama metnin türü izin verdiği ölçüde. Bir KVKK sözleşmesine konuşma dili girmez, bir gazete yazısına girer.

## Yapı Koruma (Temel Prensip)

Metnin **yapısını değiştirme**. Bu Faz 1'den önce, Faz 2'den önce, her şeyden önce uygulanan temel prensiptir.

- Madde listeleri (bullet, numaralı) liste olarak kalır. Paragrafa çevirme.
- Başlık hiyerarşisi (H1, H2, H3) korunur.
- Tablolar tablo olarak kalır.
- Blok alıntılar (quote) blok olarak kalır.
- Kod blokları kod olarak kalır.
- Belge şablonuna ait yapısal alanlar (form kısımları, yönerge kutuları) yerinde bırakılır.

Skill'in işi cümle/paragraf içi dil düzeltmesi. Belge iskeletine dokunma. Kullanıcı açıkça "yapıyı da değiştir" demedikçe.

## Register Teşhisi

Metni işlemeden önce hangi ailede olduğunu belirle. Beş register kategorisi:

| Register | Örnek metin türleri | Faz 2 uygulaması |
|----------|--------------------|--------------------|
| **Hukuki-idari** | Sözleşme, KVKK metni, tebliğ, kanun taslağı | Yok. Sadece Faz 1. |
| **Akademik-kurumsal** | Tez, akademik makale, resmi rapor, kurum belgesi | Kısıtlı. Sadece cümle uzunluğu varyansı ve öz-düzeltme. |
| **Analitik-gazetecilik** | Haber, analiz, brifing, düşünce kuruluşu raporu | Tam uygulama, konuşma bağlaçları hariç. |
| **Deneme-blog** | Deneme, köşe yazısı, blog, sosyal medya uzun formatı | Tam uygulama. Bütün Faz 2 sinyalleri. |
| **Edebi-yaratıcı** | Roman, öykü, denemeye yakın anlatı | Tam uygulama artı duyusal detay ve zaman kipi çeşitliliği. |

**Karar süreci:** Metni okuduğunda ilk seçim skill'e ait değil — kullanıcıya doğrulatılır. Skill otomatik bir tahmin yapar ve kullanıcıya sorar: "Bu metni [register adı] olarak görüyorum, Faz 2'de şunları uygulayacağım: [liste]. Onaylıyor musun, yoksa başka bir register mi?"

Eğer kullanıcı baştan register belirtmişse ("bu blog yazısı," "bu akademik metin") teşhis atlanır, doğrudan uygulanır.

## Faz 1 — AI Dokunuşlarını Çıkarma (Koşulsuz)

Faz 1 iki katmandan oluşur: **baskın sinyaller** ve **ikincil sinyaller**. Baskın sinyaller Türkçe AI metninin en belirgin beş fenotipidir — üçü veya daha fazlası bir metinde varsa metin ağır AI-imzalıdır. İkincil sinyaller destekleyici işaretlerdir.

### Baskın Sinyaller (Beş Ana Fenotip)

#### Sinyal 1 — Noktalama Enflasyonu (İngilizce Mantığı)

Türkçe noktalama TDK'ya göre çok kısıtlı kullanım aralığına sahiptir. AI, İngilizce noktalama mantığını Türkçe'ye zorla çevirir.

**Alt sinyaller:**

- **1a. Uzun tire enflasyonu.** TDK'ya göre uzun çizgi (—) yalnızca **konuşma çizgisi** olarak kullanılır ("Frankfurt'a gelene herkesin sorduğu şunlardır: — Eski şehri gezdin mi?"). Cümle içinde ara söz için TDK **bitişik yazılan kısa çizgi (-)** öngörür ("Küçük bir sürü -dört inekle birkaç koyun- köye giren..."). AI ise İngilizce em dash mantığıyla cümle içine düşünce sıkıştırır (— boşluklu, cümle-arası).
  - Alarm eşiği: 1+ cümle-içi uzun tire

- **1b. Noktalı virgül suistimali.** TDK'ya göre noktalı virgülün üç kullanımı vardır: (1) virgüllerle ayrılmış tür/takımları gruplama, (2) ögeleri arasında virgül bulunan sıralı cümleleri ayırma, (3) uzun cümlelerde özneden sonra vurgu. AI'nin "iki fikri birleştir" kullanımı bunların hiçbiri değil.
  - Alarm eşiği: bir paragrafta 2+ noktalı virgül, özellikle "; aynı zamanda" yapısı

- **1c. İki nokta üst üste açıklama modu.** TDK'ya göre iki nokta örnek/açıklama gerektiren cümlenin sonuna ya da konuşma-diyalog için kullanılır. Cümle içi mini açıklama için değil. AI İngilizce inline colon mantığıyla "X: Y" yapıları üretir.
  - Alarm eşiği: bir paragrafta 2+ cümle-içi iki nokta üst üste

- **1d. Bağlaç "ki" bitişik yazma.** AI bazen "bu ki," "şu ki," "durum şu ki" tarzı bitişik kullanım üretir. TDK'ya göre "ki" bağlacı ayrı yazılır — sadece kalıplaşmış "belki, çünkü, mademki, meğerki, oysaki, sanki, hâlbuki" bitişiktir.

- **1e. Slash-ayırıcı alternatifler.** AI İngilizce mantığıyla alternatifleri slash ile ayırır: "kullanıcı/müşteri/istemci", "hızlı/verimli/etkili." Türkçe "kullanıcı, müşteri veya istemci" der. Slash yerine virgül + veya.
  - Alarm eşiği: metinde 2+ slash-ayırıcı

**Onarım stratejisi:** Uzun tireleri virgüle çevir veya cümleyi böl. Noktalı virgülleri iki ayrı cümleye ayır. İki noktaları sadece TDK-uygun bağlamlarda tut. Bağlaç "ki"leri ayır. Slash-ayırıcıları "A, B veya C" yapısına çevir.

#### Sinyal 2 — Cümle Monotonluğu (Uzunluk Varyansı Düşük)

İnsan yazısı **burstiness** taşır: kısa-orta-uzun cümleler karışıktır, ritim nefes alır. AI Türkçesi ise orta uzunlukta cümlelerde takılır — hepsi 18-25 kelime arası, arada kısa nokta yok, arada uzun analitik yok.

- Alarm eşiği: bir paragraftaki cümlelerin uzunluk standart sapması düşük (kabaca %30'un altında) VE ortalama 22+ kelime
- Diğer sinyal: hiç 5 kelimeden kısa cümle yok

**Onarım stratejisi:** En uzun cümleyi böl, ortasına kısa bir cümle patlaması yerleştir. "Bitti." "Kalakaldı." "Yani bir kaza." Türkçe canlı yazının en güçlü nefes noktası bu.

#### Sinyal 3 — Kalıp Tekrarı

AI belirli yapıları tekrar tekrar kullanır. Üç alt kalıp:

- **3a. "-mektedir/-maktadır" salgını.** Bir paragrafta 4+ "-mektedir/-maktadır" yüklem varsa alarm. Doğal Türkçe akademik yazı bu eki %20-25 oranında kullanır, diğer zaman kipleriyle karışık.

- **3b. Bürokratik bağlaç yığını.** "Bu bağlamda," "söz konusu," "öte yandan," "bunun yanı sıra," "bu doğrultuda," "bu çerçevede" — bir metinde toplam 3+ kullanım alarm.

- **3c. AI kapanış klişeleri.** "Kritik bir rol oynamaktadır," "hayati bir önem taşımaktadır," "önemli bir rol üstlenmektedir," "vazgeçilmez bir hale gelmiştir" — bir metinde 2+ kullanım alarm. Özellikle paragraf sonlarında.

**Onarım stratejisi:** Yüklem çeşitliliği ekle — geçmiş zaman ("oldu, çıktı"), geniş zaman ("olur, çıkar"), aktif ses. Bürokratik bağlaçları konuşma bağlaçlarıyla (Register izin veriyorsa) veya gerçek mantıksal bağlarla değiştir ("Ama," "Çünkü," "Nitekim"). Boş kapanışları sil — paragraf bir önceki cümlede bitsin.

#### Sinyal 4 — "Sadece X Değil, Aynı Zamanda Y" Ailesi

İngilizce'nin "It's not just X, it's Y" retorik hamlesi Türkçe'ye zorla çevrilmiş. AI'nin en belirgin retorik imzalarından biri.

**Varyantları:**
- "Sadece X değil, aynı zamanda Y"
- "Yalnızca X söz konusu değildir; Y de..."
- "X olmakla birlikte, Y niteliğini de taşımaktadır"
- "X'in ötesinde, Y boyutu göz ardı edilmemelidir"
- "Salt X değil; bir Y meselesi"

Alarm eşiği: bir metinde 1+ örnek.

**Onarım stratejisi:** İki cümleye ayır ve gerçek mantıksal bağı bul. "X. Ama aynı zamanda Y." formülünden kaç. Ya "hem X hem Y" yapısına çevir (bu daha doğal Türkçe), ya iki bağımsız cümle yap ya da bağı tamamen kaldır.

#### Sinyal 5 — Boş Övgü + Boş Kapanış

- **5a. Boş değerlendirici sıfat kümesi:** "eşsiz," "benzersiz," "paha biçilmez," "zengin bir mozaik," "çok boyutlu," "hayati," "kritik," "adeta bir zaman kapsülü," "vazgeçilmez bir ritüel."
  - Alarm eşiği: bir paragrafta 2+ örnek

- **5b. Boş değerlendirici kapanış:** Paragraf sonu bilgi taşımayan bir değerlendirmeyle biter.
  - Alarm eşiği: her paragrafın bu tür klişeyle bitmesi

**Onarım stratejisi:** Sıfatı sil, isim başlı kalsın. Ya da somut örnekle değiştir. Boş kapanışları çıkar, paragraf bir önceki cümlede bitsin.

### İkincil Sinyaller (Destekleyici İşaretler)

Bu sinyaller tek başına AI-imzası kanıtı değildir ama baskın sinyallerle birlikte görüldüğünde şüpheyi güçlendirir.

#### Sinyal 6 — "Adeta" ve "Sanki" Bağımlılığı

Somut örnek eksikliğini vage benzetmeyle örtme. "Adeta bir zaman kapsülü gibi," "sanki hikâyeler anlatıyor."

**Onarım:** Benzetmeyi somut örnekle değiştir. Somut örnek yoksa cümleyi sil.

#### Sinyal 7 — Zorlama Üçlü Liste

"Hız, kapsayıcılık ve sürdürülebilirlik gibi çok boyutlu bir yaklaşım." Üç öğe arasında gerçek fark olup olmadığını kontrol et. Yoksa dolgu — sadece ritim doldurma.

**Onarım:** Bir öğeye indir ya da üçünden en spesifik olanı seç.

#### Sinyal 8 — Devrik Cümle Yokluğu

Doğal Türkçe kimi zaman devrik kurar; LLM Türkçesi hiç kurmaz. Bir paragrafta hiç devrik cümle yoksa bu tek başına bir sinyal — özellikle uzun paragraflarda.

**Onarım:** Uygun bir cümlede yüklemi öne çek. Zorlama yapma — doğal geldiği yerde.

#### Sinyal 9 — Register Kayması

Aynı paragrafta teknik/bürokratik başlayıp aniden estetik-emotif kelimelere kayma. "Yapay zekâ teknolojileri" + "adeta bir zaman kapsülü" aynı paragrafta.

**Onarım:** Baştaki register'ı belirle, o katmanda kal.

#### Sinyal 10 — Somut Anchor Eksikliği

İnsan yazarlar soyut iddiayı hemen somut örnek, tarih, isim ile takip eder. LLM soyut kalır veya uydurma "spesifik gibi görünen" ifadeler üretir.

**Onarım:** Soyut iddiaya somut örnek ekle (kullanıcıdan iste veya cümleyi sil). Uydurma anchor ekleme.

#### Sinyal 11 — Pasif Yapı Bağımlılığı

"-mektedir" salgınıyla akraba ama farklı: AI cümlelerin çoğunu pasif çatıda kurar. "Yapılmaktadır, edilmektedir, kılınmaktadır." Öznesizleştirme.

**Onarım:** Mümkün olan yerde aktif çatıya çevir. Özne belirginleşsin.

#### Sinyal 12 — İngilizce Vurgu-Doldurucuları

Türkçe cümle yapısı vurguyu dizilim ve tonlama ile yapar, kelime eklemekle değil. LLM İngilizce refleksiyle vurgu kelimelerini Türkçe'ye tıkar. Bu kelimeler cümleden çıkarılınca anlam kaybolmaz — dolgu-vurgu.

**Aile:**
- **"tam da"** (İng. precisely) — "Bu tam da aradığımız şey", "Tam da o noktada"
- **"tam anlamıyla"** (İng. literally, truly)
- **"gerçekten de"** (İng. indeed)
- **"aslında da"** (İng. in fact)
- **"kesinlikle"** (dolgu olarak, İng. definitely)
- **"esasen"** (İng. essentially)
- **"nihayetinde"** (İng. ultimately)
- **"bir bakıma"** (İng. in a way)
- **"gerçek anlamda"** (İng. in a real sense)

Alarm eşiği: bir metinde 2+ örnek.

**Onarım:** Sil. Cümlenin anlamı zaten korunuyor. Gerçek vurgu gerekiyorsa cümle yapısını değiştir (özneyi öne al, kısa cümle patlaması ekle) — kelime eklemekle değil.

#### Sinyal 13 — Eş Anlamlı Rotasyonu

AI aynı özneye tek paragrafta üç ayrı ad verir. Bu İngilizce üslupta "elegant variation" öğütlenir ama Türkçe yazıda **berraklık üretmez, kafa karıştırır**. Okur "bunlar aynı şey mi, farklı mı" diye durur.

**Örnekler:**
- Aynı özne için: "kullanıcı / müşteri / istemci"
- Kurumsal: "hükümet / iktidar / yönetim / rejim"
- Kişi için: "vatandaş / birey / kişi"
- Şirket için: "firma / şirket / kuruluş"

Alarm eşiği: bir paragrafta aynı özneye 3+ ayrı ad.

**Onarım:** Bir ad seç, o adı kullan. Türkçe akademik yazı geleneği tekrar yapar — bu bir kusur değil, açıklık aracı.

#### Sinyal 14 — Fragment Cümle Bağımlılığı

AI cümleyi iki noktayla açıp fragment liste veya fragment tanım bırakır: "Üç seçenek: A, B, C." "Sonuç şu: yeni bir dönem." Türkçe konuşma tam cümle kurar: "Üç seçenek var: A, B, C." "Sonuç şu, yeni bir dönem başlıyor."

Bu Sinyal 1c (iki nokta üst üste açıklama modu) ile akraba ama farklı: 1c noktalama sorunu, 14 cümle bütünlüğü sorunu.

Alarm eşiği: bir metinde 3+ fragment.

**Onarım:** Fragment'ı tam cümleye tamamla. "Üç seçenek:" → "Üç seçenek var:". "Sonuç:" → "Sonuç şu:", ya da "Sonuç şöyle:".

## Faz 2 — İnsan Ritmi Enjeksiyonu (Koşullu)

Bu faz register uygunsa çalışır. Her sinyalin yanında hangi register'da aktif olduğu belirtilmiştir.

Faz 2'nin mantığı Faz 1'in tersidir: bunlar **eğer yoksa yerleştir** komutlarıdır, "eğer varsa koru" değil. Steril metne insan nefesi enjekte etmek için.

### Sinyal 15 — Cümle Uzunluğu Patlamaları

**Register:** Akademik-kurumsal ve üstü.

Uzun paragraf ortasına 1-3 kelimelik cümle yerleştir. "Bitti dedim." "Kalakaldı." "Yer gök hastane." "Yani bir yerde durdu."

Bu nefes noktasıdır, ritim kırılmasıdır. Zorlama olmasın — anlamın gerçekten durduğu yere yerleştir.

### Sinyal 16 — Konuşma Bağlaçları

**Register:** Analitik-gazetecilik ve üstü.

"Ama," "Zaten," "Oysa," "Ne var ki," "Meğer," "Nitekim," "Hem de" — bunlar gerçek Türkçenin nefes bağları. Bürokratik "bu bağlamda / söz konusu / dolayısıyla" yerine bunlar konur.

Edebi-yaratıcı register'da ekstra: "Valla," "yani," "he," "canım," "ulan" — konuşma tonu açıksa.

### Sinyal 17 — Retorik Soru

**Register:** Deneme-blog ve üstü.

Anlatıcının okura veya kendine soru sorması. "Ne yapabilirsin ki adama?" "Neden bu kadar mahzundular?" "Peki bu doğru mu?"

Bu, iddia etmenin alternatifi — okuru sürece davet eder. AI iddia eder, sorgulamaz.

### Sinyal 18 — Öz-düzeltme ve Yeniden İfade

**Register:** Akademik-kurumsal ve üstü.

Yazar kendi cümlesini düzeltir, geri alır, yeniden söyler. "Belki de böyle değildi. İşin aslında başka şeyler de vardı." "Ya da hayır, öyle değil — şöyle demek doğru olur."

Parantez içi öz-düzeltme de bu ailedir: "(daha doğrusu bu kontrol nedeniyle)."

Bu, canlı düşünme imzasıdır — LLM ilk cümlesini takar, düşünmez üstüne.

### Sinyal 19 — Duyusal Somut Detay

**Register:** Edebi-yaratıcı.

İnsan yazısı koku, dokunma, ses taşır. "Kalın bir iple beş defa dönüyor bu koku evin çevresinde." "Yoluk kirpikleri." "Sentetik gömleklerinden alerji olurum."

AI yazısı sadece görsel-soyut kalır. Edebi metinde duyusal detay olmayan bir betimleme AI'dır.

### Sinyal 20 — Zaman Kipi Kayması

**Register:** Edebi-yaratıcı.

İnsan yazısı şimdiki + geçmiş + geniş zamanı akışkan geçer. AI tek kipe takılır. Edebi metinde tek-kip devam ediyorsa aralara farklı kip yerleştir — özellikle iç ses ile anlatı arasındaki geçişlerde.

### Sinyal 21 — Diyalog İzi

**Register:** Edebi-yaratıcı.

Anlatı içinde karakterin sesi geçer — bir cümle direkt alıntı, bir parantez, bir tırnak. "İyi de nereden tanışıyor bu adam?" AI anlatıcı sesini korur, karakterin sesini kaybeder.

### Sinyal 22 — Birinci Çoğul Kullanımı

**Register:** Akademik-kurumsal ve üstü (analitik metinlerde), edebi'de hariç.

"Sanıyoruz," "görüyoruz," "araştırdığımızda," "belirttiğimiz gibi." Bu, okuru düşünme sürecine dahil eder. Akademik Türkçe yazı geleneğinde çok güçlüdür (Ortaylı, Keyder) ama AI pasif "-mektedir"e sığınır.

## Uzun Belge Davranışı

Çok paragraflı belge (3+ paragraf) geldiğinde şu maker/checker deseni uygulanır:

1. **Tara.** Belgeyi paragraf paragraf oku, her paragraf için baskın sinyal yoğunluğunu ölç (kaç baskın sinyal, hangileri).

2. **Sınıflandır.** Paragrafları üç sınıfa ayır:
   - **Yüksek AI-imzalı** (3+ baskın sinyal) — mutlaka temizlenmeli
   - **Orta AI-imzalı** (1-2 baskın sinyal) — kullanıcı isterse temizlenir
   - **Temiz** (0 baskın sinyal) — dokunma

3. **Rapor sun.** Kullanıcıya genel tablo ver:
   > "Belge N paragraf. K'sı yüksek AI-imzalı (P#, P#), M'i orta (P#, P#, P#), kalanı temiz görünüyor. Register teşhisim: [X]. Faz 2 uygulaması: [Y]. Nasıl ilerleyelim?"

4. **Seçenek sun.** Üç mod:
   - **Tümünü tek seferde** — bir uzun cevap veya docx çıktısı
   - **Bloklar halinde** — mantıksal gruplar, her blok sonrası onay
   - **Sadece yüksek AI-imzalılar** — hızlı geçiş

5. **Uygula ve işaretle.** Her paragrafın hangi sinyallerine dokunduğunu kısa notlarla belirt.

## Çıktı Formatı (Atlanamaz)

Skill'in her çıktısı dört bölümden oluşur — hiçbiri atlanamaz. Rapor atlanırsa skill başarısız sayılır.

**1. Tespit Raporu.** Metinde hangi baskın ve ikincil sinyaller görüldü, konumlarıyla:
> "Sinyal 3a — '-mektedir' salgını: c.1, c.2, c.4 (dört tekrar). Sinyal 4 — 'sadece X değil aynı zamanda Y': c.1'de tam örnek..."

**2. Sinyal Yoğunluğu (Nicel Metrik).** Metnin uzunluğunu 100 kelimeye normalize ederek sinyal yoğunluğunu ölç:
> "Önce: 8.4 sinyal/100 kelime. Sonra: 1.2 sinyal/100 kelime. İyileşme oranı: %86."

Bu skorlama ASD-STE100 standardından esinlenerek eklendi. Her paragraf için ayrı hesap tutulabilir. Metin uzunluğu 200 kelimeden azsa toplam sinyal sayısını doğrudan ver.

**3. Onarılmış Versiyon.** Tam metin, hem Faz 1 hem Faz 2 uygulanmış.

**4. Notlar.** Kısa bir açıklama:
> "Şu sinyaller kasıtlı bir tercih olabilir, dokunmadım: [varsa]. Şu yerlerde register kısıtı nedeniyle daha muhafazakâr davrandım: [varsa]. Şu Faz 2 sinyalleri register uygun olduğu için uygulandı: [liste]."

Ayrıca "YOK olduğu için not düştüğüm şeyler" satırı ekle — metinde bulunması beklenip bulunmayan sinyaller. Bu bir editör hoşnutluğu ifadesi: yazar temiz bir metin yazmış, bu tanınmalı.

Kullanıcı sadece onarılmış versiyonu istiyorsa raporu ve nicel metriği atlar ama "rapor da ver" ihtimali için hazır tutar.

## Sınırlar ve Etik

- Bu skill **kelime değiştirici değil, yapı değiştiricidir**. Sinonim değiştirme yapmaz. Yapısal AI imzalarını hedefler.

- AI detektör atlatmak birincil amaç değil. **Doğal Türkçe** birincil amaç. Detektörler bunun yan ürünü olabilir ama garanti değildir.

- **Yazar-özgü stilistik tercihlere saygı göster.** Bazı sinyaller yazarın imzasıdır, AI-tikî değildir. Örnekler:
  - Ayfer Tunç tarzı "..." yerine ".." kullanımı (yazar tercihi)
  - Tanpınar tarzı eski Türkçe bağlaçlar ("Şüphesiz," "Vâkıa")
  - Bıçakçı tarzı noktalı virgülle karşıtlık kurma (TDK-uygun edebi tercih)
  - Sürekli aynı yüklem seçimi eğer bilinçli ritim tercihiyse
  - Fragment bitirme edebi bir tercihse ("Bir akıl." tarzı Tanpınarvari) — bilinçli mi kontrol et

Kullanıcı bağlam belirtmişse ("bu yazar şöyle yazar," "bu benim tarzım") o çerçeveye saygı göster, dokunma.

- **Ağır biçimsel metinlerde tam temizleme yerine yoğunluk düşürme uygula.** Paragraf başına 4+ "-mektedir" varsa 2-3'e indir, hepsini alma. Akademik ton yabancılaşmasın.

- **Metnin bilgi içeriği korunmalı.** Eğer bir sinyal aynı zamanda bir iddia taşıyorsa, o iddiayı farklı bir yapıyla yeniden ifade et. Sinyal sadece dolgu ise sil.

- **Yapı bütünlüğüne dokunma.** Bu belgenin başında "Yapı Koruma" bölümünde tanımlandı. Madde listeleri liste kalır, başlıklar başlık kalır, tablolar tablo kalır. Cümle/paragraf içi dil düzeltmesi yaparsın, belge iskeletine dokunmazsın.

## Referans

Türkçe noktalama kararlarında TDK Yazım Kuralları esas alınır (tdk.gov.tr/kategori/icerik/yazim-kurallari). İnsan ritmi sinyalleri Türk edebiyatının yerleşik yazarlarından — Ahmet Hamdi Tanpınar, Ayfer Tunç, Barış Bıçakçı, Ahmet Ümit — stilometrik incelemeyle çıkarıldı. Akademik ritim örnekleri Ortaylı, Keyder, Marcus, Aksoy, Karpat metinlerinden alındı. Nicel metrik ASD-STE100 (Simplified Technical English, 1986) standardının "kural ihlali puanı" mantığından esinlendi. Genel humanizer mimarisi harshaneel/humanize ve makotofalcon/humanizer-ja projelerinden ilham aldı.

---

**Sürüm:** v2.1 (Ağustos 2026)
