---
name: turkce-humanizer
description: Türkçe metinlerden AI-üretimi yazım imzalarını temizler. "Türkçeleştir," "AI kokusunu at," "bu Türkçe metni doğallaştır" gibi ifadelerle veya AI-üretimi görünen Türkçe paragraf sunulduğunda tetiklenir.
---

# Türkçe Humanizer

Bu skill, Türkçe LLM çıktılarını insan-sesli Türkçe'ye dönüştürür. Kelime değiştirmeden değil, **yapısal AI imzalarını hedefleyerek** çalışır.

## Çalışma Prensibi

Metni okuduğunda önce aşağıdaki on sinyali tarar. Her sinyal için o metinde kaç kere göründüğünü ve konumunu tespit eder. Sonra sinyaller "bilgi taşıma" işlevi olmayan dolgular ise onları çıkarır, sinyal gerçek bir işlev taşıyorsa dokunmaz.

**Kritik kural:** Metnin *bilgi içeriği* korunmalı. Eğer sinyal aynı zamanda bir iddia taşıyorsa, o iddiayı farklı bir yapıyla yeniden ifade et. Sinyal sadece dolgu ise sil.

## Türkçe AI-İmzası Taksonomisi

### 1. Bağlaç ve Geçiş Klişeleri

LLM'in Türkçe'de en sık kullandığı yapısal köprüler:

- **"Sadece X değil, aynı zamanda Y"** ve varyantları: "yalnızca X söz konusu değildir; Y de..." / "X olmakla birlikte, Y niteliğini de taşımaktadır" / "X'in ötesinde, Y boyutu göz ardı edilmemelidir"
- **"Bu bağlamda"** — analitik köprü klişesi
- **"Söz konusu [şey]"** — dolgu belirleyici
- **"Öte yandan"** — bürokratik varyant
- **"Bunun yanı sıra"**
- **"Önemli bir husus olarak karşımıza çıkmaktadır"**

**Onarım:** İki cümleyi ayır, gerçek mantıksal bağı bul. "X. Ama Y." veya "X çünkü Y." Gerçek bağ zayıfsa cümleyi tek başına bırak.

### 2. "-mektedir/-maktadır" Salgını

Bir paragrafta dört veya daha fazla `-mektedir/-maktadır` varsa alarm. Doğal Türkçe akademik yazı bu eki %20-25 oranında kullanır, diğer zamanlarla karışık.

**Onarım:** Yüklem çeşitliliği ekle: geçmiş zaman ("oldu, çıktı"), geniş zaman ("olur, çıkar"), aktif ses ("... yapıyor" / "... görüyoruz").

### 3. Boş Değerlendirici Sıfat Kümesi

"Eşsiz," "benzersiz," "paha biçilmez," "zengin bir mozaik," "kritik bir rol," "hayati bir önem," "vazgeçilmez bir ritüel," "adeta bir zaman kapsülü."

**Onarım:** Sıfatı sil, isim başlı kalsın. Ya da somut örnekle değiştir. "Kritik bir rol oynamaktadır" → sadece "önemlidir" veya doğrudan hangi rolü oynadığını yaz.

### 4. Boş Değerlendirici Kapanış

Paragraf sonlarında: "kritik bir rol oynamaktadır," "hayati bir önem taşımaktadır," "vazgeçilmez bir hale gelmiştir." Bu cümlenin **hiçbir yeni bilgi taşımadığını** kontrol et.

**Onarım:** Sil. Paragraf bir önceki cümlede bitsin. Gerekirse yeni bir somut cümleyle kapat.

### 5. "Adeta" ve "Sanki" Bağımlılığı

Somut örnek eksikliğini vage benzetmeyle örtme. "Adeta bir zaman kapsülü gibi," "sanki hikâyeler anlatıyor."

**Onarım:** Benzetmeyi somut örnekle değiştir. Somut örnek yoksa cümleyi sil.

### 6. Zorlama Üçlü Liste

"Hız, kapsayıcılık ve sürdürülebilirlik gibi çok boyutlu bir yaklaşım." Üçlemenin üç öğesi arasında gerçek fark olup olmadığını kontrol et. Yoksa dolgu.

**Onarım:** Bir öğeye indir. Ya da üçünden en spesifik olanı seç.

### 7. Noktalı Virgül + "Aynı Zamanda" Köprüsü

"X'tir; aynı zamanda Y'dir." Bu Türkçe'de doğal olmayan bir yapı — İngilizce'nin "It's not just X, it's Y"nin Türkçe'ye zorla çevirisi.

**Onarım:** İki ayrı cümleye böl. Ya da "hem X hem Y" yapısına çevir — bu daha doğal Türkçe.

### 8. Devrik Cümle Yokluğu

Doğal Türkçe kimi zaman devrik kurar; LLM Türkçesi hiç kurmaz. Bir paragrafta hiç devrik cümle yoksa bu tek başına bir sinyal.

**Onarım:** Uygun bir cümlede yüklemi öne çek. Zorlama yapma — doğal geldiği yerde.

### 9. Register Kayması

Aynı paragrafta teknik/bürokratik başlayıp aniden estetik-emotif kelimelere kayma. "Yapay zekâ teknolojileri" + "adeta bir zaman kapsülü" aynı paragrafta.

**Onarım:** Baştaki register'ı belirle, o katmanda kal.

### 10. Somut Anchor Eksikliği

İnsan yazarlar soyut iddiayı hemen somut örnek, tarih, isim ile takip eder. LLM soyut kalır veya uydurma "spesifik gibi görünen" ifadeler üretir.

**Onarım:** Soyut iddiaya somut örnek ekle (kullanıcıdan iste veya cümleyi sil). Uydurma anchor ekleme.

## İnsan İmzasını Koruma

Şu özellikler *insan Türkçesi* imzasıdır — metinde varsa **koru veya güçlendir**:

- Cümle uzunluğu varyansı (kısa-uzun-orta karışımı)
- Konuşma dilinden bağlaçlar: "Zaten," "Oysa," "Tersine," "Ne var ki," "Lâkin"
- Birinci çoğul kullanımı: "sanıyoruz," "görüyoruz," "araştırdığımızda"
- Devrik cümle
- Parantez içi öz-düzeltme veya bilgi ekleme
- İronik tırnak kullanımı
- Tutarlı tek-katman register
- Somut tarih, isim, belge, sayı

## Çıktı Formatı

Metni işlerken:

1. Önce tespit raporu ver: "Bu metinde şu sinyaller görüldü: [liste, konumlarla]"
2. Sonra onarılmış versiyonu sun
3. Kısa bir not: "Şu sinyaller kasıtlı bir tercih olabilir, dokunmadım: [varsa]"

Kullanıcı sadece onarılmış versiyonu istiyorsa raporu atla.

## Sınırlar

- Bu skill **kelime değiştirici değil, yapı değiştiricidir.** Sinonim değiştirme yapmaz.
- AI detektör atlatmak birincil amaç değil; **doğal Türkçe** birincil amaç.
- Sertçe biçimsel metinlerde (hukuki metin, resmi tebliğ) bazı sinyaller kasıtlıdır — kullanıcı bağlamı belirtmezse standart uygular.
