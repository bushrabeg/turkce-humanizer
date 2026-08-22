# turkce-humanizer

> Türkçe metinlerden yapay zekâ yazım imzalarını temizleyen Claude skill'i.
> A Claude skill that removes AI writing signatures from Turkish text.

**Güncel sürüm:** v2.1 ([release notları](https://github.com/bushrabeg/turkce-humanizer/releases))

## Ne Yapar

Türkçe LLM çıktılarının kendine özgü bir kokusu var: `-mektedir` salgını, "sadece X değil, aynı zamanda Y" retoriği, "bu bağlamda / söz konusu / kritik bir rol oynamaktadır" bürokratik bağlaç zinciri, boş değerlendirici sıfat kümeleri, zorlama noktalı virgül köprüleri, "tam da" gibi İngilizce vurgu-doldurucuları. Bu skill Claude'a bu imzaları tanımayı ve **metnin bilgi içeriğini bozmadan** temizlemeyi öğretir. Register uygunsa insan Türkçesinin ritim imzalarını da yerleştirir.

## Neden Türkçe İçin Ayrı Bir Skill

İngilizce humanizer skill'leri (`harshaneel/humanize`, `blader/humanizer`) İngilizce'nin AI-imzasına — em dash, "delve/testament/pivotal," rule of three — göre kalibre edilmiş. Türkçe farklı davranıyor:

- Türkçe **eklemeli** bir dil; AI kokusu ekin sırasında ve sıklığında görünüyor
- Türkçe **serbest dizilim** izin veriyor ama LLM devrik cümleden kaçınıyor
- Türkçe **register salınımı** İngilizce'den daha keskin (Osmanlıca / Öz Türkçe / gündelik)
- **Ses uyumu** ve nadir kelimelerde ek uyumsuzluğu Türkçe-özgü sinyaller
- **Noktalama mantığı farklı**: TDK'ya göre noktalı virgül, iki nokta ve uzun tire kısıtlı kullanılır — AI İngilizce mantığıyla bunları şişirir

Bu skill Türkçe'nin kendi AI-imzası taksonomisi üzerine kurulu, İngilizce şablonun çevirisi değil. Ayrıca **register-farkında**: bir KVKK sözleşmesine konuşma dili girmez, bir gazete yazısına girer.

## Nasıl Çalışıyor

**İki fazlı mimari:**

- **Faz 1** — AI dokunuşlarını çıkarır (14 sinyal), koşulsuz uygulanır
- **Faz 2** — İnsan Türkçesinin ritim imzalarını yerleştirir (8 sinyal), metnin registerine göre koşullu uygulanır

**Register teşhisi** — Beş kategori: hukuki-idari, akademik-kurumsal, analitik-gazetecilik, deneme-blog, edebi-yaratıcı. Skill otomatik teşhis yapar ve kullanıcıya doğrulatır.

**Nicel metrik** — Sinyal-başına-100-kelime skoru (ASD-STE100 esinli). Skill "önce X sinyal / 100 kelime, sonra Y sinyal / 100 kelime, iyileşme %Z" raporu verir.

## Örnek

**AI Türkçesi:**

> Söz konusu dinamikler, hükümetler ve şirketler arasındaki güç dengelerini yeniden tanımlamaktadır. Bu bağlamda, yapay zekâ çağında ülkelerin stratejik pozisyonlanması, hayati bir önem taşımaktadır.

**Skill'den geçen:**

> Ortaya çıkan tablo, hükümetlerle büyük model şirketleri arasındaki gücü yeniden dağıtıyor. Bir ülkenin bu dağılımda nereye düştüğü, önümüzdeki on yılın soruları arasında en belirleyici olanı.

## Kurulum

### Claude Code

```bash
mkdir -p ~/.claude/skills/turkce-humanizer
curl -L -o ~/.claude/skills/turkce-humanizer/SKILL.md \
  https://raw.githubusercontent.com/bushrabeg/turkce-humanizer/main/SKILL.md
```

### Claude.ai / Claude Desktop

`SKILL.md` dosyasını indir, Claude'a upload et veya Skills panelinden yükle.

### Codex

Bu skill'in mevcut `SKILL.md` formatı Codex ile de uyumludur. Proje kapsamındaki bir Codex oturumunda kullanmak için hedef projenin kök dizininde aşağıdaki komutu çalıştır:

```bash
mkdir -p .agents/skills/turkce-humanizer
curl -L -o .agents/skills/turkce-humanizer/SKILL.md \
  https://raw.githubusercontent.com/bushrabeg/turkce-humanizer/main/SKILL.md
```

Windows PowerShell için:

```powershell
$skillDir = Join-Path $PWD ".agents\skills\turkce-humanizer"
New-Item -ItemType Directory -Force -Path $skillDir
Invoke-WebRequest -UseBasicParsing `
  -Uri "https://raw.githubusercontent.com/bushrabeg/turkce-humanizer/main/SKILL.md" `
  -OutFile (Join-Path $skillDir "SKILL.md")
```

Skill'i tüm projelerde kullanmak istersen, komutlardaki proje kökü yerine kullanıcı dizinini kullan: Bash'te `~/.agents/skills/turkce-humanizer`, PowerShell'de `Join-Path $HOME ".agents\skills\turkce-humanizer"`.

Kurulumdan sonra Codex skill'i otomatik olarak keşfeder. Açıkça çağırmak için `$turkce-humanizer` yazabilir; "AI kokusunu at" veya "bu Türkçe metni doğallaştır" gibi isteklerle otomatik tetiklenmesini de sağlayabilirsin.

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

Skill önce register teşhisi yapar, sonra tespit raporu + nicel metrik + onarılmış versiyon + notlar sunar.

## Kaynaklar ve Teşekkür

Bu skill şu çalışmalardan ilham aldı:

- [`harshaneel/humanize`](https://github.com/harshaneel/humanize) — dokuz-kaldıraç mimarisi ve maker/checker deseni
- [`makotofalcon/humanizer-ja`](https://github.com/makotofalcon/humanizer-ja) — dile özgü adaptasyon örneği
- [`blader/humanizer`](https://github.com/blader/humanizer) — Wikipedia "Signs of AI writing" temelli orijinal yaklaşım
- **ASD-STE100** (Simplified Technical English, 1986) — nicel metrik ilhamı

Türkçe taksonomi, beş Türkçe kitabın (Ortaylı, Keyder, Marcus, Aksoy, Karpat) stilometrik incelemesi ve dört Türk yazarın (Tanpınar, Ayfer Tunç, Barış Bıçakçı, Ahmet Ümit) ritim analiziyle kuruldu. Noktalama katmanı TDK Yazım Kuralları temelli.

## Katkı

Issue açarak yeni AI-Türkçesi kalıpları önerebilir, yanlış tespit örnekleri paylaşabilirsiniz. PR'lar açıktır.

## Lisans

MIT
