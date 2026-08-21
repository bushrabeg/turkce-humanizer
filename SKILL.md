---
name: turkce-humanizer
description: Türkçe metinlerden AI-üretimi yazım imzalarını temizler ve metnin registeri uygunsa insan Türkçesinin ritim imzalarını yerleştirir. "Türkçeleştir," "AI kokusunu at," "bu Türkçe metni doğallaştır," "humanize et" gibi ifadelerle veya AI-üretimi görünen Türkçe paragraf sunulduğunda tetiklenir.
---

# Türkçe Humanizer

Bu skill iki fazlı çalışır. **Faz 1** metinden AI-üretimi yazım imzalarını çıkarır. Koşulsuzdur, her metne uygulanır. **Faz 2** metnin registeri uygunsa insan Türkçesinin ritim imzalarını yerleştirir. Koşulludur, metnin türüne göre değişir.

İki faz birlikte metni AI-steril olmaktan çıkarır ve canlı hâle getirir. Amaç sadece AI kokusunu atmak değil, insan yazısının nefesini geri vermek. Ama metnin türü izin verdiği ölçüde. Bir KVKK sözleşmesine konuşma dili girmez, bir gazete yazısına girer.

## Dört Mutlak Yasak (Register-Bağımsız)

Bu dört yasak her metinde, her register'da, her koşulda uygulanır. TDK'ya uygun olsa bile uygulanır. Faz 1'den önce, Faz 2'den önce, her şeyden önce.

**1. Uzun çizgi yasak.** Cümle içinde uzun çizgi (—) kullanılmaz. Ara söz için virgül kullanılır ya da cümle bölünür. Konuşma çizgisi olarak diyalog başında bile bu skill'in çıktısında yer almaz. Uzun çizgi Türkçe AI çıktısının en belirgin imzalarından biridir.

**2. Noktalı virgül yasak.** Hiçbir yerde noktalı virgül kullanılmaz. TDK'ya göre doğru bir kullanım olsa bile kullanılmaz. Noktalı virgül gördüğün her yerde cümleyi iki ayrı cümleye böl. Noktalı virgül AI'nin İngilizce "; also / ; however" mantığını Türkçe'ye zorla çevirdiği yerdir.

**3. Yarım cümle yasak.** Fragment cümle üretmez. "Bitti." "Kalakaldı." tarzı tek kelimelik veya yarım hissi veren cümleler yazmaz. Cümle her zaman özne ve yüklem yapısını taşır. Kısa cümle üretebilir, ama tam cümle olarak.

Tam cümle örneği (izinli): "İtibarı da bu tutumdan geldi."
Yarım cümle örneği (yasak): "Bu tutumdan."

**4. Bağlaçla açılan izole cümle yasak.** "Ama şu oldu." "Ancak durum farklıdır." gibi bağlaçla açılıp tek başına duran cümleler üretmez. Bağlaç ya cümlenin içinde kalır ya da iki cümle birleştirilir.

Yasak örneği: "Türkiye savaşa girmedi. Ama savaşın ekonomisini yaşadı."
İzinli hâli: "Türkiye savaşa girmedi ama savaşın ekonomisini yaşadı."

**5. Karşıtlık bağlaçlarından önce virgül yasak.** "Ama," "ancak," "fakat," "lakin," "yalnız" (bağlaç anlamında) gibi karşıtlık bağlaçlarından *önce* virgül konulmaz. Bu İngilizce'nin "but" ve "however" öncesi zorunlu virgül kuralının Türkçe'ye zorla çevirisidir. TDK'ya göre de yanlıştır. AI çıktısının en belirgin sözdizim imzalarından biridir.

Yasak örneği: "Plan hazırdı, ancak uygulamada aksadı."
İzinli hâli: "Plan hazırdı ancak uygulamada aksadı."

Yasak örneği: "Türkiye reform yaptı, ama sonuç sınırlı kaldı."
İzinli hâli: "Türkiye reform yaptı ama sonuç sınırlı kaldı."

Bu beş yasağın hiçbiri register'a bağlı değildir. Her metne uygulanır. Detaylı TDK gerekçeleri için `docs/tdk-referanslari.md` dosyasına bakılır (ama skill bu dosyaya bakmadan da yasakları uygular).

## Pozitif Yön — Güçlü Cümle Hedefi

Skill kısaltıcı değil, güçlendiricidir. Hedef Türk edebiyatının ve akademisinin yerleşik cümle mimarisidir. Örnek referanslar `examples/gucli-cumleler.md` dosyasında tutulur, skill oradaki cümleleri kalıp öğrenimi için okur.

Güçlü cümlenin üç niteliği:

- **Bir şey iddia eder.** Cümle bittiğinde okur bir şey biliyor olmalı. AI cümleleri kaçamaklı biter, güçlü cümle durur. "İnsan ruhunun yarası dikiş tutmaz." (Ahmet Ümit) — cümle bittiğinde bir iddia elimizde.
- **Tam bir yapıdadır.** Özne ve yüklem yerindedir. Uzunluk önemli değil, tamlık önemli. Kısa da olabilir uzun da.
- **Ritmi vardır.** Cümlenin içinde bir nefes akışı vardır. Bu ritim virgül sayısıyla ölçülmez, cümlenin okuma sesiyle ölçülür.

Referans yazarlar: Ahmet Hamdi Tanpınar, Ayfer Tunç, Barış Bıçakçı, Ahmet Ümit, İlber Ortaylı, Kemal Karpat, Çağlar Keyder. Akademik ritim için ek referans: Marcus, Aksoy. Skill bir cümle üretirken bu yazarların cümle mimarisine bakar. Zorlama taklit değil, kalıp öğrenimi.

## Yapı Koruma

Metnin yapısını değiştirme. Bu dört yasaktan sonra, her şeyden önce uygulanır.

- Madde listeleri liste olarak kalır. Paragrafa çevirme.
- Başlık hiyerarşisi korunur.
- Tablolar tablo olarak kalır.
- Blok alıntılar blok olarak kalır.
- Kod blokları kod olarak kalır.
- Belge şablonuna ait yapısal alanlar yerinde bırakılır.

Skill'in işi cümle içi ve paragraf içi dil düzeltmesi. Belge iskeletine dokunma. Kullanıcı açıkça "yapıyı da değiştir" veya "paragrafa çevir" demedikçe.

## Register Teşhisi

Metni işlemeden önce hangi ailede olduğunu belirle. Beş register kategorisi:

| Register | Örnek metin türleri | Faz 2 uygulaması | Profil sorgusu |
|----------|--------------------|--------------------|-----------------|
| **Hukuki-idari** | Sözleşme, KVKK, tebliğ, kanun taslağı | Yok | Yok |
| **Akademik-kurumsal** | Tez, akademik makale, resmi rapor | Kısıtlı: sadece cümle varyansı ve öz-düzeltme | Yok |
| **Analitik-gazetecilik** | Haber, analiz, brifing, düşünce kuruluşu raporu | Tam, konuşma bağlaçları hariç | Var |
| **Deneme-blog** | Deneme, köşe yazısı, blog, sosyal medya uzun formatı | Tam | Var |
| **Edebi-yaratıcı** | Roman, öykü, denemeye yakın anlatı | Tam artı duyusal detay ve zaman kipi | Var |

Metni okuduğunda ilk seçim skill'e ait değil, kullanıcıya doğrulatılır. Skill otomatik bir tahmin yapar ve kullanıcıya sorar: "Bu metni [register adı] olarak görüyorum. Onaylıyor musun, yoksa başka bir register mi?"

Kullanıcı baştan register belirtmişse ("bu blog yazısı," "bu akademik metin") teşhis atlanır. Sınır durumlarda (analitik-gazetecilik ile deneme-blog arasında sıkışan metinler gibi) detaylı ayrım için `docs/register-detayli.md` dosyasına bakılabilir.

## Yazar Profili Sistemi

Register profil-uyumluysa (analitik-gazetecilik, deneme-blog, edebi-yaratıcı), skill yazarın kendi imzasını da hesaba katar.

### Profil dosyası

Profil kullanıcının makinesinde `~/.claude/skills/turkce-humanizer/profiles/ben.md` yolunda tutulur. İçeriği ham metindir: kullanıcının kendi yazdığı üç ile beş örnek metin. Skill bu örneklerden yazarın nefes uzunluğunu, kavram üretme kalıplarını, tercih ettiği ritmi çıkarır ve koruma listesine ekler.

Kullanıcı YAML yazmaz, alan doldurmaz. Sadece metin yapıştırır.

### Profil sorgu mantığı

Kullanıcı bir metin atar ve humanize istendi. Skill register'ı belirler.

**Register profil-uyumsuz (hukuki, akademik-kurumsal):** Skill profil konusunu hiç açmaz. Doğrudan işi yapar.

**Register profil-uyumlu:** Skill profil dosyasını kontrol eder.

- Profil varsa: kullanır, işi yapar.
- Profil yoksa: bir kez sorar. "Bu metin profil uygulamaya uygun bir register'da. Senin ritmini katmamı istersen kendi yazdığın bir metni paylaş, profilini oluşturayım. İstemiyorsan devam edebilirim."
  - Kullanıcı örnek yapıştırırsa: skill profili oluşturur, dosyaya yazar, aynı çalıştırmada humanize yapar.
  - Kullanıcı "istemiyorum" derse: profilsiz humanize yapar.

### Tercih hafızası

"İstemiyorum" kararı sadece o sohbete özel. Yeni sohbette baştan sorulur. Aynı sohbette tekrar sorulmaz. Bir sohbet içinde ikinci ve sonraki profil-uyumlu metinlerde önceki karar uygulanır.

### Yazar imzası koruma

Profil aktifse, skill AI-sinyali ile yazar-imzası ayrımını profil örneklerine bakarak yapar. Örnek: yazar profil dosyasında sık kavram üretimi gösteriyorsa ("masada kazanılan egemenlik" tipi ifadeler), skill bu tür ifadeleri AI-sinyali sanıp temizlemez.

## Kullanıcıya Danışma Protokolü

Skill kendi kararının sınırında olduğu her yerde kullanıcıya açık soru sorar. Üç durumda:

- Bir sinyalin AI-imzası mı yoksa yazar tercihi mi olduğu belirsiz.
- Bir bağlaç veya kalıp register'da sınırda. Örnek: "Ne var ki" analitik-gazetecilik register'ının sınırında bir bağlaçtır. Skill "bu register'da 'Ne var ki' mi 'Ama' mı tercih edersin?" diye sorar.
- Metonim ile eş anlamlı rotasyonu ayrımı belirsiz. Örnek: "Türkiye / Ankara / cumhuriyet" aynı özne için farklı adlar değildir, metonimdir (Türkiye = ülke, Ankara = hükümet merkezi, cumhuriyet = rejim). Skill emin değilse sorar.

Sorular kısa ve tek seçim olur. Kullanıcı "hepsini sen bilirsin" derse skill kendi varsayılan kararını uygular.

## Faz 1 — AI Dokunuşlarını Çıkarma (Koşulsuz)

Faz 1 üç katmandan oluşur: **mutlak yasaklar** (yukarıda tanımlı), **baskın sinyaller** ve **ikincil sinyaller**. Baskın sinyaller Türkçe AI metninin en belirgin fenotipleridir. Üçü veya daha fazlası bir metinde varsa metin ağır AI-imzalıdır. İkincil sinyaller destekleyici işaretlerdir.

### Baskın Sinyaller

#### Sinyal 1 — Noktalama Enflasyonu

Türkçe noktalama TDK'ya göre kısıtlıdır. AI, İngilizce noktalama mantığını Türkçe'ye zorla çevirir.

**1a. Uzun tire.** Mutlak yasak (yukarıda tanımlı). Cümle içi uzun tire virgüle çevrilir veya cümle bölünür.

**1b. Noktalı virgül.** Mutlak yasak. TDK-uygun olsa bile kaldırılır. Cümle ikiye bölünür (bağlaçla açılan izole cümle üretmeden).

**1c. İki nokta üst üste açıklama modu.** Cümle içi mini açıklama için iki nokta kullanma. AI "X: Y" yapıları üretir. Türkçe'de iki nokta örnek/açıklama gerektiren cümle sonuna veya konuşma öncesine gelir.

- Alarm eşiği: bir paragrafta iki veya daha fazla cümle-içi iki nokta.
- Onarım: iki nokta üst üsteyi kaldır, iki cümle yap veya virgülle bağla.

**1d. Bağlaç "ki" bitişik yazma.** "bu ki," "şu ki," "durum şu ki" yapıları ayrı yazılır. Sadece kalıplaşmış "belki, çünkü, mademki, meğerki, oysaki, sanki, hâlbuki" bitişiktir.

- Onarım: bitişik "ki"leri ayır.

**1e. Slash-ayırıcı.** "kullanıcı/müşteri/istemci" yapısı Türkçe değildir. Virgül ve "veya" ile değiştirilir.

- Alarm eşiği: metinde iki veya daha fazla slash-ayırıcı.
- Onarım: "A, B veya C" yapısına çevir.

**1f. Karşıtlık bağlacı öncesi virgül.** Mutlak yasak 5 (yukarıda tanımlı). "Ama," "ancak," "fakat," "lakin," "yalnız" bağlaçlarından önce virgül gördüğün her yerde kaldır. İngilizce "but/however" öncesi zorunlu virgül kuralının Türkçe'ye zorla çevirisidir.

- Alarm eşiği: metinde bir örnek yeter.
- Onarım: bağlaç öncesindeki virgülü sil, cümle bütün olarak akmaya bıraksın.

#### Sinyal 2 — Cümle Monotonluğu

İnsan yazısı burstiness taşır. Kısa, orta ve uzun cümleler karışıktır, ritim nefes alır. AI Türkçesi ise orta uzunlukta cümlelerde takılır — hepsi 18-25 kelime arası, arada kısa tam cümle yok, arada uzun analitik yok.

- Alarm eşiği: paragrafta cümle uzunluğu standart sapması düşük (%30 altı) ve ortalama 22+ kelime.
- Diğer sinyal: hiç 8 kelimeden kısa tam cümle yok.

**Onarım:** En uzun cümleyi böl, ortasına kısa bir tam cümle yerleştir. Yarım cümle üretme (mutlak yasak 3). Kısa cümle özne ve yüklem taşır.

#### Sinyal 3 — Kalıp Tekrarı

AI belirli yapıları tekrar tekrar kullanır. Üç alt kalıp:

**3a. "-mektedir/-maktadır" salgını.** Bir paragrafta dört veya daha fazla "-mektedir/-maktadır" yüklem varsa alarm. Doğal Türkçe akademik yazı bu eki %20-25 oranında kullanır, diğer zaman kipleriyle karışık.

- Onarım: yüklem çeşitliliği ekle — geçmiş zaman ("oldu, çıktı"), geniş zaman ("olur, çıkar"), aktif ses.

**3b. Bürokratik bağlaç yığını.** "Bu bağlamda," "söz konusu," "öte yandan," "bunun yanı sıra," "bu doğrultuda," "bu çerçevede" — bir metinde toplam üç veya daha fazla kullanım alarm.

- Onarım: bürokratik bağlaçları gerçek mantıksal bağlarla değiştir ("Çünkü," "Nitekim," "Oysa"). Ama bağlaçlar cümle içinde kalır, izole cümle açmaz (mutlak yasak 4).

**3c. AI kapanış klişeleri.** "Kritik bir rol oynamaktadır," "hayati bir önem taşımaktadır," "önemli bir rol üstlenmektedir," "vazgeçilmez bir hale gelmiştir" — bir metinde iki veya daha fazla kullanım alarm. Özellikle paragraf sonlarında.

- Onarım: boş kapanışları sil, paragraf bir önceki cümlede bitsin.

#### Sinyal 4 — "Sadece X Değil, Aynı Zamanda Y" Ailesi

İngilizce'nin "It's not just X, it's Y" retorik hamlesi Türkçe'ye zorla çevrilmiş. AI'nin en belirgin retorik imzalarından biri.

Varyantlar:
- "Sadece X değil, aynı zamanda Y"
- "Yalnızca X söz konusu değildir; Y de..."
- "X olmakla birlikte, Y niteliğini de taşımaktadır"
- "X'in ötesinde, Y boyutu göz ardı edilmemelidir"
- "Salt X değil; bir Y meselesi"

Alarm eşiği: bir metinde bir örnek.

**Onarım:** Gerçek mantıksal bağı bul. Üç seçenek:
- "Hem X hem Y" yapısına çevir (bu daha doğal Türkçe)
- İki bağımsız cümle yap (ikinci cümle bağlaçla açılmasın, mutlak yasak 4)
- Bağı tamamen kaldır, sadece Y'yi bırak (X gereksizse)

#### Sinyal 5 — Boş Övgü ve Boş Kapanış

**5a. Boş değerlendirici sıfat kümesi:** "eşsiz," "benzersiz," "paha biçilmez," "zengin bir mozaik," "çok boyutlu," "hayati," "kritik," "adeta bir zaman kapsülü," "vazgeçilmez bir ritüel."

- Alarm eşiği: bir paragrafta iki veya daha fazla örnek.
- Onarım: sıfatı sil, isim başlı kalsın. Veya somut örnekle değiştir.

**5b. Boş değerlendirici kapanış:** Paragraf sonu bilgi taşımayan bir değerlendirmeyle biter.

- Alarm eşiği: her paragrafın bu tür klişeyle bitmesi.
- Onarım: boş kapanışları çıkar, paragraf bir önceki cümlede bitsin.

### İkincil Sinyaller

Bu sinyaller tek başına AI-imzası kanıtı değildir ama baskın sinyallerle birlikte görüldüğünde şüpheyi güçlendirir.

**Sinyal 6 — "Adeta" ve "sanki" bağımlılığı.** Somut örnek eksikliğini vague benzetmeyle örtme. "Adeta bir zaman kapsülü gibi," "sanki hikâyeler anlatıyor."
- Onarım: benzetmeyi somut örnekle değiştir. Somut örnek yoksa cümleyi sil.

**Sinyal 7 — Zorlama üçlü liste.** "Hız, kapsayıcılık ve sürdürülebilirlik gibi çok boyutlu bir yaklaşım." Üç öğe arasında gerçek fark yoksa dolgu.
- Onarım: bir öğeye indir veya üçünden en spesifik olanı seç.

**Sinyal 8 — Devrik cümle yokluğu.** Doğal Türkçe kimi zaman devrik kurar, LLM Türkçesi hiç kurmaz. Uzun paragrafta hiç devrik yoksa sinyal.
- Onarım: uygun bir cümlede yüklemi öne çek. Zorlama yapma.

**Sinyal 9 — Register kayması.** Aynı paragrafta teknik/bürokratik başlayıp aniden estetik-emotif kelimelere kayma.
- Onarım: baştaki register'ı belirle, o katmanda kal.

**Sinyal 10 — Somut anchor eksikliği.** İnsan yazarlar soyut iddiayı hemen somut örnek, tarih, isim ile takip eder. LLM soyut kalır.
- Onarım: soyut iddiaya somut örnek ekle (kullanıcıdan iste veya cümleyi sil). Uydurma anchor ekleme.

**Sinyal 11 — Pasif yapı bağımlılığı.** Cümlelerin çoğu pasif çatıda. "Yapılmaktadır, edilmektedir, kılınmaktadır." Öznesizleştirme.
- Onarım: mümkün olan yerde aktif çatıya çevir.

**Sinyal 12 — İngilizce vurgu-doldurucuları.** Türkçe cümle yapısı vurguyu dizilim ve tonlama ile yapar, kelime eklemekle değil. LLM İngilizce refleksiyle vurgu kelimelerini tıkar.

Aile:
- "tam da" (İng. precisely)
- "tam anlamıyla" (İng. literally, truly)
- "gerçekten de" (İng. indeed)
- "aslında da" (İng. in fact)
- "kesinlikle" (dolgu olarak, İng. definitely)
- "esasen" (İng. essentially)
- "nihayetinde" (İng. ultimately)
- "bir bakıma" (İng. in a way)
- "gerçek anlamda" (İng. in a real sense)

- Alarm eşiği: metinde iki veya daha fazla örnek.
- Onarım: sil. Cümlenin anlamı zaten korunuyor.

**Sinyal 13 — Eş anlamlı rotasyonu.** AI aynı özneye tek paragrafta üç ayrı ad verir. İngilizce üslupta "elegant variation" öğütlenir ama Türkçe'de kafa karıştırır.

Örnekler:
- Aynı özne için: "kullanıcı / müşteri / istemci"
- Kurumsal: "hükümet / iktidar / yönetim / rejim"
- Kişi için: "vatandaş / birey / kişi"
- Şirket için: "firma / şirket / kuruluş"

- Alarm eşiği: bir paragrafta aynı özneye üç veya daha fazla ayrı ad.
- Onarım: bir ad seç, o adı kullan. Türkçe akademik yazı geleneği tekrar yapar — bu bir kusur değil, açıklık aracı.
- Not: Metonim (Türkiye / Ankara / cumhuriyet gibi) eş anlamlı değildir. Ayrım belirsizse kullanıcıya sor.

**Sinyal 14 — Fragment cümle bağımlılığı.** Mutlak yasak 3 ile örtüşür. AI cümleyi iki noktayla açıp fragment liste veya fragment tanım bırakır: "Üç seçenek: A, B, C." "Sonuç şu: yeni bir dönem."
- Onarım: fragment'ı tam cümleye tamamla. "Üç seçenek var: A, B, C." "Sonuç şu, yeni bir dönem başlıyor."

## Faz 2 — İnsan Ritmi Enjeksiyonu (Koşullu)

Bu faz register uygunsa çalışır. Her sinyalin yanında hangi register'da aktif olduğu belirtilmiştir.

**Faz 2'nin mantığı Faz 1'in tersidir.** Faz 1'in kuralı: "eğer varsa çıkar." Faz 2'nin kuralı: "eğer yoksa yerleştir." Steril metne insan nefesi enjekte etmek için. Ama yerleştirme mutlak yasakları ihlal edemez — özellikle yarım cümle ve izole bağlaç yasağı Faz 2 sinyallerinde de geçerlidir.

### Sinyal 15 — Cümle Uzunluğu Varyansı

**Register:** Akademik-kurumsal ve üstü.

Uzun paragrafta ritim monotonsa, orta uzunlukta bir cümle daha kısa bir cümleye dönüştürülür. Kısa cümle **tam cümle** olur — özne ve yüklem taşır. Yarım cümle üretme (mutlak yasak 3).

İzinli örnek: "İtibarı da bu tutumdan geldi."
Yasak örnek: "Bu tutumdan."

Zorlama olmasın — anlamın gerçekten durduğu yere yerleştir.

### Sinyal 16 — Konuşma Bağlaçları

**Register:** Analitik-gazetecilik ve üstü.

"Ama," "Zaten," "Oysa," "Ne var ki," "Meğer," "Nitekim," "Hem de" — bunlar gerçek Türkçenin nefes bağları. Bürokratik "bu bağlamda / söz konusu / dolayısıyla" yerine bunlar konur.

Ama bu bağlaçlar tek başına cümle açamaz (mutlak yasak 4). Cümlenin içinde kalır.

Yasak örnek: "Türkiye reform yaptı. Ama sonuç sınırlı kaldı."
İzinli örnek: "Türkiye reform yaptı ama sonuç sınırlı kaldı."

Edebi-yaratıcı register'da ekstra bağlaçlar açılır: "Valla," "yani," "he," "canım" — konuşma tonu açıksa. Bu register bağlaçları da izole cümle açamaz.

### Sinyal 17 — Retorik Soru

**Register:** Deneme-blog ve üstü.

Anlatıcının okura veya kendine soru sorması. "Ne yapabilirsin ki adama?" "Neden bu kadar mahzundular?" "Peki bu doğru mu?"

Bu, iddia etmenin alternatifi — okuru sürece davet eder. AI iddia eder, sorgulamaz.

### Sinyal 18 — Öz-düzeltme ve Yeniden İfade

**Register:** Akademik-kurumsal ve üstü.

Yazar kendi cümlesini düzeltir, geri alır, yeniden söyler.

Örnek: "Belki de böyle değildi. İşin aslında başka şeyler de vardı."
Örnek: "Ya da hayır, öyle değil, şöyle demek doğru olur."

Parantez içi öz-düzeltme de bu ailedir: "(daha doğrusu bu kontrol nedeniyle)."

Bu canlı düşünme imzasıdır — LLM ilk cümlesini takar, düşünmez üstüne.

### Sinyal 19 — Duyusal Somut Detay

**Register:** Edebi-yaratıcı.

İnsan yazısı koku, dokunma, ses taşır. AI yazısı sadece görsel-soyut kalır.

Örnekler:
- "Kalın bir iple beş defa dönüyor bu koku evin çevresinde."
- "Yoluk kirpikleri."
- "Sentetik gömleklerinden alerji olurum."

Edebi metinde duyusal detay olmayan bir betimleme AI'dır. Duyusal detay skill tarafından üretilmez, kullanıcıya sorulur ("Bu sahnede karakterin duyduğu koku, ses veya dokunma varsa ekleyebilirim, ne var?").

### Sinyal 20 — Zaman Kipi Kayması

**Register:** Edebi-yaratıcı.

İnsan yazısı şimdiki, geçmiş, geniş zamanı akışkan geçer. AI tek kipe takılır. Edebi metinde tek-kip devam ediyorsa aralara farklı kip yerleştir — özellikle iç ses ile anlatı arasındaki geçişlerde.

### Sinyal 21 — Diyalog İzi

**Register:** Edebi-yaratıcı.

Anlatı içinde karakterin sesi geçer — bir cümle direkt alıntı, bir parantez, bir tırnak. "İyi de nereden tanışıyor bu adam?" AI anlatıcı sesini korur, karakterin sesini kaybeder.

### Sinyal 22 — Birinci Çoğul Kullanımı

**Register:** Akademik-kurumsal ve üstü (analitik metinlerde), edebi'de hariç.

"Sanıyoruz," "görüyoruz," "araştırdığımızda," "belirttiğimiz gibi." Bu, okuru düşünme sürecine dahil eder. Akademik Türkçe yazı geleneğinde çok güçlüdür (Ortaylı, Keyder) ama AI pasif "-mektedir"e sığınır.

## Uzun Belge Davranışı

Çok paragraflı belge (üç veya daha fazla paragraf) geldiğinde şu maker/checker deseni uygulanır:

1. **Tara.** Belgeyi paragraf paragraf oku, her paragraf için baskın sinyal yoğunluğunu ölç.

2. **Sınıflandır.** Paragrafları üç sınıfa ayır:
   - Yüksek AI-imzalı (üç veya daha fazla baskın sinyal) — mutlaka temizlenmeli.
   - Orta AI-imzalı (bir-iki baskın sinyal) — kullanıcı isterse temizlenir.
   - Temiz (baskın sinyal yok) — dokunma.

3. **Rapor sun.** Kullanıcıya genel tablo ver:
   > "Belge N paragraf. K'sı yüksek AI-imzalı (P#, P#), M'i orta (P#, P#, P#), kalanı temiz görünüyor. Register teşhisim: [X]. Faz 2 uygulaması: [Y]. Profil durumu: [Z]. Nasıl ilerleyelim?"

4. **Seçenek sun.** Üç mod:
   - Tümünü tek seferde — bir uzun cevap veya docx çıktısı.
   - Bloklar halinde — mantıksal gruplar, her blok sonrası onay.
   - Sadece yüksek AI-imzalılar — hızlı geçiş.

5. **Uygula ve işaretle.** Her paragrafın hangi sinyallerine dokunduğunu kısa notlarla belirt.

## Değişiklik Özeti ve Onay

Skill her müdahalesini işaretler. Sonda kullanıcıya değişiklik listesi sunar:

> Değişiklikler:
> 1. "delve into" → "incele" (Sinyal 3c — AI kapanış klişesi)
> 2. Uzun tire kaldırıldı (Mutlak yasak 1)
> 3. Noktalı virgül iki cümleye bölündü (Mutlak yasak 2)
> 4. ...

Kullanıcı ya "hepsi tamam" der ya da "1, 3, 5'i uygula, gerisini bırak" der. Skill final metni ona göre üretir.

Küçük belgelerde (bir paragraf) skill değişiklik özetini otomatik gösterir. Uzun belgelerde kullanıcı isterse özet sunulur, isterse atlanır.

## Kabul Oranı Log'u

Kullanıcının onayladığı ve reddettiği değişiklikler `~/.claude/skills/turkce-humanizer/feedback.jsonl` dosyasına yazılır. Her satır bir değişiklik: sinyal, register, öneri, kullanıcı kararı, tarih.

Bu log kullanıcı istediğinde özet olarak sunulabilir ("Sinyal 7 akademik-kurumsal'da %73 kabul, deneme-blog'da %31 kabul"). Ama zorunlu değil. Arka planda birikir, ileride kalibrasyon için kullanılır.

## Çıktı Formatı (Atlanamaz)

Skill'in her çıktısı beş bölümden oluşur. Hiçbiri atlanamaz. Rapor atlanırsa skill başarısız sayılır.

**1. Tespit Raporu.** Metinde hangi baskın ve ikincil sinyaller görüldü, konumlarıyla:
> "Sinyal 3a — '-mektedir' salgını: c.1, c.2, c.4 (dört tekrar). Sinyal 4 — 'sadece X değil aynı zamanda Y': c.1'de tam örnek..."

**2. Sinyal Yoğunluğu.** Metnin uzunluğunu 100 kelimeye normalize ederek sinyal yoğunluğunu ölç:
> "Önce: 8.4 sinyal/100 kelime. Sonra: 1.2 sinyal/100 kelime. İyileşme oranı: %86."

Bu skorlama ASD-STE100 standardından esinlenerek eklendi. Her paragraf için ayrı hesap tutulabilir. Metin uzunluğu 200 kelimeden azsa toplam sinyal sayısını doğrudan ver.

**3. Onarılmış Versiyon.** Tam metin, hem Faz 1 hem Faz 2 uygulanmış.

**4. Değişiklik Özeti.** Yukarıdaki formatta, kullanıcı onayı bekler.

**5. Notlar.**
- Kasıtlı tercih olabilir, dokunmadım: [varsa]
- Register kısıtı nedeniyle muhafazakâr davrandım: [varsa]
- Faz 2 uygulandı: [liste]
- Profil durumu: [aktif / soruldu-reddedildi / uyumsuz-register / yok]
- **YOK olduğu için not düştüğüm şeyler:** metinde bulunması beklenip bulunmayan sinyaller.

Bu son satır bir editör hoşnutluğu ifadesidir. Yazar temiz bir metin yazmışsa bu tanınmalı — sadece hataları saymak değil, iyi yapılanı da görmek. Bu skill'in editör-yazar ilişkisi kurma çabasının parçası.

## Sınırlar ve Etik

- Bu skill **kelime değiştirici değil, yapı değiştiricidir**. Sinonim rotasyonu yapmaz. Yapısal AI imzalarını hedefler.

- AI detektör atlatmak birincil amaç değil. **Doğal Türkçe** birincil amaç. Detektörler bunun yan ürünü olabilir ama garanti değildir.

- **Yazar-özgü stilistik tercihlere saygı göster.** Bazı sinyaller yazarın imzasıdır, AI-tikî değildir. Profil aktifse profile bak, profil yoksa kullanıcıya sor. Örnekler:
  - Ayfer Tunç tarzı "..." yerine ".." kullanımı (yazar tercihi)
  - Tanpınar tarzı eski Türkçe bağlaçlar ("Şüphesiz," "Vâkıa")
  - Bıçakçı tarzı ritim kırılmaları (yazar tercihi olarak)
  - Sürekli aynı yüklem seçimi eğer bilinçli ritim tercihiyse

Kullanıcı bağlam belirtmişse ("bu yazar şöyle yazar," "bu benim tarzım") o çerçeveye saygı göster.

- **Ağır biçimsel metinlerde tam temizleme yerine yoğunluk düşürme uygula.** Paragraf başına dört "-mektedir" varsa iki-üçe indir, hepsini alma. Akademik ton yabancılaşmasın.

- **Metnin bilgi içeriği korunmalı.** Bir sinyal aynı zamanda bir iddia taşıyorsa, o iddiayı farklı bir yapıyla yeniden ifade et. Sinyal sadece dolgu ise sil.

- **Yapı bütünlüğüne dokunma.** Madde listeleri liste kalır, başlıklar başlık kalır, tablolar tablo kalır. Kullanıcı açıkça istemedikçe.

- **Mutlak yasakların hiçbirinden taviz verilmez.** Yazar imzası bile olsa, kasıtlı tercih bile olsa, TDK-uygun bile olsa: uzun çizgi, noktalı virgül, yarım cümle, bağlaçla açılan izole cümle, karşıtlık bağlacı öncesi virgül skill çıktısında yer almaz.

## Referans

Türkçe noktalama kararlarında TDK Yazım Kuralları esas alınır. Ama TDK'nın izin verdiği bazı yapılar bu skill'de yasaktır (uzun çizgi, noktalı virgül). Ayrıca skill TDK'nın *yanlış* saydığı ama AI'nin yaygın olarak ürettiği yapıları da yasaklar (karşıtlık bağlaçları öncesi virgül). Bu skill TDK ile aynı değildir. Bazı yerlerde TDK'dan katı, bazı yerlerde TDK'yla aynı çizgide.

İnsan ritmi sinyalleri Türk edebiyatının ve akademisinin yerleşik yazarlarından stilometrik incelemeyle çıkarıldı. Edebi referans: Ahmet Hamdi Tanpınar, Ayfer Tunç, Barış Bıçakçı, Ahmet Ümit. Akademik referans: İlber Ortaylı, Kemal Karpat, Çağlar Keyder, Marcus, Aksoy.

Örnek cümleler ve kalıp öğrenimi için: `examples/gucli-cumleler.md`.

Detaylı TDK gerekçeleri, register ayrımının uç durumları ve mimari kararlar için: `docs/tdk-referanslari.md`, `docs/register-detayli.md`, `docs/mimari-kararlar.md`.

Nicel metrik ASD-STE100 (Simplified Technical English, 1986) standardının kural ihlali puanı mantığından esinlendi. Genel humanizer mimarisi harshaneel/humanize ve makotofalcon/humanizer-ja projelerinden ilham aldı.

---

**Sürüm:** v3.0 (Ağustos 2026)
