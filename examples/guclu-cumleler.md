# Güçlü Türkçe Cümleler — Örnek Korpusu

Bu dosya turkce-humanizer skill'inin "güçlü cümle" hedefi için referans korpusudur. Skill bir cümle üretirken bu örneklerin cümle mimarisine bakar. Zorlama taklit değil, kalıp öğrenimi.

## Güçlü Cümlenin Üç Niteliği

Bu korpus üç niteliği ortak taşıyan cümlelerden oluşur:

**1. Bir şey iddia eder.** Cümle bittiğinde okur bir şey biliyor olmalı. AI cümleleri kaçamaklı biter, güçlü cümle durur.

**2. Tam bir yapıdadır.** Özne ve yüklem yerindedir. Uzunluk önemli değil, tamlık önemli.

**3. Ritmi vardır.** Cümlenin içinde bir nefes akışı vardır. Bu ritim virgül sayısıyla ölçülmez, cümlenin okuma sesiyle ölçülür.

## Edebi Referanslar

### Ahmet Hamdi Tanpınar

**Beş Şehir:**
> "En büyük meselemiz mazi ile nerede ve nasıl bağlanacağımız sorusu. Hepimiz bir şuur ve benlik buhranının çocuklarıyız."

**Huzur:**
> "Vücutlarımız, birbirimize en kolay vereceğimiz şey. Asıl mesele, birbirimize hayatlarımızı verebilmek."

**Saatleri Ayarlama Enstitüsü:**
> "Saatin kendisi mekân, yürüyüşü zaman, ayarı insandır."

### Ayfer Tunç

**Bir Deliler Evinin Yalan Yanlış Anlatılan Kısa Tarihi:**
> "Yaş ilerleyince anlıyordu insan. Mutluluk öyle gökten zembille inmiyor, itina istiyordu."

**Suzan Defter:**
> "Yaşamak her şeye rağmen bir iz bırakmaktır yeryüzünde."

### Barış Bıçakçı

**Bizim Büyük Çaresizliğimiz:**
> "Her şeyin geçip gittiğine, yaşadıklarımızın geçmişte kaldığına kim inandırabilir bizi?"

**Denemeler (genel üslup):**
> "Önce aşk vardır. Hatırlamak da, acı çekmek de ondan sonra başlar."

### Ahmet Ümit

**İstanbul Hatırası:**
> "İnsan ruhunun yarası dikiş tutmaz."

## Akademik Referanslar

### İlber Ortaylı

**İmparatorluğun En Uzun Yüzyılı:**
> "Osmanlı Batılılaşması, Batı'yı hayranlıktan öte zorunluluk nedeniyle tercih etmiştir."

### Kemal Karpat

> "Osmanlı Devleti, daha sonra otuz kadar ulusa dönüşerek imparatorluğun varlığına son vermiş olan sayısız etnik, dilsel ve dinsel grubu bir arada tutan, tarihin en başarılı çok-kavimli, çok-dinli devletlerinden biriydi."

### Çağlar Keyder

**Türkiye'de Devlet ve Sınıflar:**
> "Jöntürklerin başlıca kaygısı, Osmanlı Devletinin özerkliğini ve coğrafi bütünlüğünü yeniden kurmaktı. Böylece 'devleti kurtarmak', geleneksel düzeni bürokrasinin ayrıcalıklı konumunu değiştirmeden korumanın sembolik formülü oldu."

## Korpus Notları

Bu korpus başlangıç sürümüdür. Zaman içinde şu yollarla zenginleştirilir:

- Kullanıcının okumalarında rastladığı "işte bu cümle" dediği örneklerin eklenmesi.
- Marcus, Aksoy gibi ek akademik referanslardan cümle çıkarımı.
- Farklı registerlerden örneklerin dengelenmesi (şu an edebi ağırlık baskın, akademik ve analitik-gazetecilik eksik).
- Katkı: pull request ile başkaları da örnek ekleyebilir.

## Neden Bu Yazarlar

Referans yazarların seçimi keyfi değil. Üç kriter:

**1. Türkçe düz yazı geleneğinin yerleşik isimleri.** Bu yazarlar kanonda. Ritim imzaları rastlantısal değil, gelenek üreten.

**2. Farklı registerlerin temsili.** Tanpınar edebi-yaratıcı. Ortaylı akademik-kurumsalın gazete köşesine kayan hali. Karpat ve Keyder tam akademik. Ayfer Tunç ve Bıçakçı çağdaş edebi. Ahmet Ümit popüler edebi.

**3. AI-Türkçesinden en uzak stiller.** Bu yazarların cümle mimarisi AI çıktısının tam karşıtı. Onları öğrenmek AI-imzasını çıkarmayı da öğrenmek demek.

Yaşayan yazarlardan örnek almak ayrıca faydalı — çünkü çağdaş Türkçe'nin ritim imzasını gösteriyor. Sadece klasik yazarlarla sınırlı kalmak modern Türkçe için yanıltıcı olurdu.

## Kullanım Notu (Skill İçin)

Skill bu korpusa şu şekilde bakar:

- Faz 2'de bir cümle üretirken bu örneklerin uzunluk ortalamasına, özne-yüklem yakınlığına, bağlaç kullanımına referans alır.
- Kelime seçimi taklit etmez. "Osmanlı" ya da "insan ruhu" gibi terimleri kopyalamaz.
- Cümle *iddia gücü* için bu örneklere bakar. Kaçamaklı bitiş üretmek yerine iddialı bitiş üretir.

Bu bir yazar taklidi değildir. Cümle mimarisinin öğrenimidir.
