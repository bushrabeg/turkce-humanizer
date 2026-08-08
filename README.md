# turkce-humanizer

> Türkçe metinlerden yapay zekâ yazım imzalarını temizleyen Claude skill'i.
> A Claude skill that removes AI writing signatures from Turkish text.

## Ne Yapar

Türkçe LLM çıktılarının kendine özgü bir kokusu var: `-mektedir` salgını, "sadece X değil, aynı zamanda Y" retoriği, "bu bağlamda / söz konusu / kritik bir rol oynamaktadır" bürokratik bağlaç zinciri, boş değerlendirici sıfat kümeleri, zorlama noktalı virgül köprüleri. Bu skill Claude'a bu imzaları tanımayı ve **metnin bilgi içeriğini bozmadan** temizlemeyi öğretir.

## Neden Türkçe İçin Ayrı Bir Skill

İngilizce humanizer skill'leri (`harshaneel/humanize`, `blader/humanizer`) İngilizce'nin AI-imzasına — em dash, "delve/testament/pivotal," rule of three — göre kalibre edilmiş. Türkçe farklı davranıyor:

- Türkçe **eklemeli** bir dil; AI kokusu ekin sırasında ve sıklığında görünüyor
- Türkçe **serbest dizilim** izin veriyor ama LLM devrik cümleden kaçınıyor
- Türkçe **register salınımı** İngilizce'den daha keskin (Osmanlıca / Öz Türkçe / gündelik)
- **Ses uyumu** ve nadir kelimelerde ek uyumsuzluğu Türkçe-özgü sinyaller

Bu skill Türkçe'nin kendi AI-imzası taksonomisi üzerine kurulu, İngilizce şablonun çevirisi değil.

## Kurulum

### Claude Code

```bash
mkdir -p ~/.claude/skills/turkce-humanizer
curl -L -o ~/.claude/skills/turkce-humanizer/SKILL.md \
  https://raw.githubusercontent.com/bushrabeg/turkce-humanizer/main/SKILL.md
```

### Claude Desktop / claude.ai

`SKILL.md` dosyasını indir, Claude'a upload et veya proje skill dizinine yerleştir.

## Kullanım

```
Bu metni türkçeleştir:
[AI-üretimi Türkçe paragraf]
```

veya

```
Şu paragraftaki AI kokusunu at:
[metin]
```

## Kaynaklar ve Teşekkür

Bu skill şu çalışmalardan ilham aldı:

- [`harshaneel/humanize`](https://github.com/harshaneel/humanize) — dokuz-kaldıraç mimarisi ve maker/checker deseni
- [`makotofalcon/humanizer-ja`](https://github.com/makotofalcon/humanizer-ja) — dile özgü adaptasyon örneği
- [`blader/humanizer`](https://github.com/blader/humanizer) — Wikipedia "Signs of AI writing" temelli orijinal yaklaşım

Türkçe taksonomi, beş Türkçe kitabın (Ortaylı, Keyder, Marcus, Aksoy, Karpat) stilometrik incelemesi ve LLM üretimi kontrol paragraflarıyla karşılaştırma yoluyla kuruldu.

## Katkı

Issue açarak yeni AI-Türkçesi kalıpları önerebilir, yanlış tespit örnekleri paylaşabilirsiniz. PR'lar açıktır.

## Lisans

MIT
