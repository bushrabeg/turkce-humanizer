# Register Detayları

Bu dosya turkce-humanizer skill'inin beş register kategorisini uzun uzun açıklar. SKILL.md'de her registerin bir satırlık tarifi var; burada her registerin sınırları, örnek metin türleri, sınır durumları ve karar kriterleri detaylanır.

## Neden Register Önemli

Skill'in temel önermesi şudur: aynı temizlik kuralı her metne uygulanmaz. Bir KVKK sözleşmesine konuşma dili girmez, bir gazete yazısına girer. Bir tez yarım cümleyi tolere etmez, bir edebi metin ederse yazar bilincidir.

Faz 1 (mutlak yasaklar + baskın sinyaller + ikincil sinyaller) her registerde uygulanır — yani AI-imzası her yerden temizlenir. Faz 2 (insan ritmi enjeksiyonu) sadece uygun registerde uygulanır.

Register teşhisi yanlış yapılırsa iki tehlike var:
- **Yüksek registeri düşük sanıp Faz 2 uygulamak:** KVKK sözleşmesine konuşma bağlacı sokmak. Bu metnin işlevini bozar.
- **Düşük registeri yüksek sanıp Faz 2'yi kısıtlamak:** Blog yazısına akademik ritim vermek. Bu metni cansız bırakır.

Bu yüzden skill teşhis kararını her zaman kullanıcıya doğrulatır.

## Register 1 — Hukuki-idari

### Metin türleri

- Sözleşmeler (kira, iş, hizmet)
- KVKK metinleri, aydınlatma metinleri, açık rıza metinleri
- Kanun taslakları, kanun maddeleri, yönetmelikler
- Resmi tebliğler, genelgeler
- Mahkeme kararları
- Kurumsal politika metinleri (etik kod, disiplin yönetmeliği)
- Vekaletname, senet, protokol

### Sınırlar

Hukuki-idari register kişisel ses taşımaz. Sözleşmede "biz" veya "sanırım" olmaz. Metin bir işlem taşır, yorum değil.

### Faz 2 uygulaması

Yok. Bu registerde Faz 2 sinyallerinin hiçbiri devreye girmez. Skill sadece Faz 1'i çalıştırır — AI-imzasını temizler ama insan ritmi enjekte etmez.

### Profil sorgusu

Yok. Yazar imzası bu registerde geçerli değil.

### Sınır durum

Kurumsal iletişim metni (şirket bloguna yakın): hukuki-idari değildir. Analitik-gazetecilikte sayılır.

## Register 2 — Akademik-kurumsal

### Metin türleri

- Tezler (yüksek lisans, doktora)
- Akademik makaleler (dergi, konferans bildirisi)
- Resmi raporlar (kurum içi analiz, yıllık rapor)
- Devlet kurumu belgeleri (bakanlık raporu, komisyon raporu)
- Kitap bölümü, akademik el kitabı
- Standart, teknik şartname
- Politika belgeleri (policy paper)

### Sınırlar

Akademik-kurumsal register birinci çoğul kullanır ("araştırdığımızda," "gördüğümüz gibi") — bu yazar sesinin bir formudur ama akademik geleneğe uygundur. Kişisel deneme sesi taşımaz.

### Faz 2 uygulaması

Kısıtlı. Sadece Sinyal 15 (cümle uzunluğu varyansı), Sinyal 18 (öz-düzeltme, sınırlı) ve Sinyal 22 (birinci çoğul) devreye girer. Konuşma bağlaçları (Sinyal 16) ve retorik soru (Sinyal 17) uygulanmaz.

### Profil sorgusu

Yok. Bu registerde yazar imzası kurumsal ses tarafından örtülür.

### Sınır durum

Akademik ama "denemeye kayan" metinler (mesela Ortaylı'nın gazete yazıları): analitik-gazetecilikte sayılır, akademik-kurumsalda değil. Karar için metnin yayın mecrası (dergi mi köşe mi) bakılır.

## Register 3 — Analitik-gazetecilik

### Metin türleri

- Haber (analiz-ağırlıklı, düz haber değil)
- Analiz yazısı
- Brifing, executive summary
- Düşünce kuruluşu (think tank) raporu
- Gazete köşe yazısı (analitik ağırlıklı)
- Sektör raporu (analiz kısmı)
- Uzman görüşü yazısı
- Politika analizi

### Sınırlar

Analitik-gazetecilik yazar sesini taşır ama kişisel değil, uzman sesidir. "Ben" nadiren geçer, "biz" analitik anlamda geçer ("gördüğümüz üzere"), ama "canım," "yani," "he" gibi konuşma dili işaretleri girmez.

### Faz 2 uygulaması

Tam. Konuşma bağlaçları (Sinyal 16) uygulanır — "Ama," "Zaten," "Oysa," "Ne var ki," "Nitekim." Ama bu bağlaçlar cümlenin içinde kalır, izole cümle açmaz (mutlak yasak 4).

Retorik soru (Sinyal 17) sınırlı uygulanır. Öz-düzeltme (Sinyal 18) uygulanır.

Duyusal detay (Sinyal 19), zaman kipi kayması (Sinyal 20), diyalog izi (Sinyal 21) uygulanmaz — bunlar edebi registere aittir.

### Profil sorgusu

Var. Bu register profil-uyumludur. Skill profil dosyası varsa kullanır, yoksa bir kez sorar.

### Sınır durum

Politika analizi mi akademik makale mi belirsizse: yayın mecrasına bakılır. Hakemli dergide çıkacaksa akademik-kurumsal, düşünce kuruluşu web sitesinde çıkacaksa analitik-gazetecilik.

## Register 4 — Deneme-blog

### Metin türleri

- Kişisel deneme (essay)
- Köşe yazısı (kişisel ağırlıklı, analitik değil)
- Blog yazısı
- Sosyal medya uzun formatı (Twitter thread, LinkedIn post)
- Substack bültenleri
- Kişisel web sitesi yazıları

### Sınırlar

Deneme-blog yazar sesinin en görünür olduğu registerdir. Birinci tekil ("ben") sık, kişisel deneyim referansı sık, yazarın estetik tercihleri açık.

### Faz 2 uygulaması

Tam. Bütün Faz 2 sinyalleri devreye girer (duyusal detay, zaman kipi kayması, diyalog izi hariç — bunlar edebiye özgü).

### Profil sorgusu

Var. Bu register profil-uyumludur.

### Sınır durum

Blog yazısı mı analitik yazı mı belirsizse: yazarın birinci tekil kullanımına bakılır. "Ben bugün fark ettim ki..." açıksa deneme-blog. "Analiz şunu gösteriyor..." açıksa analitik-gazetecilik.

## Register 5 — Edebi-yaratıcı

### Metin türleri

- Roman, öykü
- Denemeye yakın anlatı
- Şiir (sınırlı — skill şiir üzerinde temkinli çalışır)
- Otobiyografik anlatı
- Yaratıcı non-fiction (creative non-fiction)
- Karakter içi düşünce akışı

### Sınırlar

Edebi-yaratıcı register her şeyin izinli olduğu register değildir. Yazar sesi, karakter sesi, anlatıcı sesi ayrımı vardır. Ama duyusal detay, zaman kipi kayması, diyalog gibi imzalar bu registere özgüdür.

### Faz 2 uygulaması

Tam artı üç ek sinyal: duyusal somut detay (Sinyal 19), zaman kipi kayması (Sinyal 20), diyalog izi (Sinyal 21).

### Profil sorgusu

Var. Ama edebi profil oluşturmak zor — çünkü edebi yazar sesi çok bireyseldir. Skill profil sorgusunda özellikle dikkatli olur: "Bu edebi bir metin, senin sesin çok belirleyici. Kendi yazdığın bir edebi parça paylaşırsan çok daha iyi çalışırım. Sadece analitik metnini değil, edebi metnini paylaşman lazım."

### Sınır durum

Edebi denemeler (Tanpınar'ın *Beş Şehir*'i gibi): edebi-yaratıcı sayılır, deneme-blog değil. Karar için metnin edebi imzalarına bakılır — duyusal detay, mecaz yoğunluğu, ritim bilinci.

## Register Teşhis Kararı Nasıl Verilir

Skill metni okuduğunda üç şeye bakar:

**1. Mecra ipuçları.** Kullanıcı metnin nerede yayımlanacağını söylediyse (mesela "bu bir tez bölümü," "bu bir blog yazısı") bu belirleyicidir.

**2. Sözdizim ipuçları.** Birinci tekil sıklığı, konuşma bağlacı varlığı, teknik terim yoğunluğu, cümle uzunluğu ortalaması, pasif çatı oranı.

**3. İşlev ipuçları.** Metin bir işlem taşıyor mu (hukuki), bir bilgi üretiyor mu (akademik), bir yorum sunuyor mu (analitik), bir deneyim aktarıyor mu (deneme), bir dünya kuruyor mu (edebi).

Bu üç ipucundan çıkardığı tahmini kullanıcıya doğrulatır. Kullanıcı düzeltirse skill kabul eder ve baştan başlar.

## Register Karışımı

Bir metnin farklı bölümleri farklı registerlerde olabilir. Örnek: bir kitap taslağının önsözü deneme-blog, giriş bölümü analitik-gazetecilik, ana bölümleri akademik-kurumsal olabilir.

Bu durumda skill her bölüm için ayrı teşhis yapar ve kullanıcıya sunar:
> "Bu belgeyi tek register olarak görmüyorum. Önsöz deneme-blog, giriş analitik-gazetecilik, ana bölümler akademik-kurumsal. Her bölümü kendi registerinde işleyeyim mi?"

Kullanıcı "hepsini tek register olarak işle" diyebilir. O zaman skill baskın registeri seçer ve ona göre çalışır — ama not düşer: "X bölümünü Y registerinde işledim, dokunmadığım stilistik özellikler olabilir."
