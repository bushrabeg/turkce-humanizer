# turkce-humanizer

> Türkçe metinlerden yapay zekâ yazım imzalarını temizleyen Claude skill'i. A Claude skill that removes AI writing signatures from Turkish text.

**Güncel sürüm:** v3.2 · **Lisans:** MIT · **Uyumluluk:** Claude Desktop, Claude Code CLI, Claude.ai

---

## Ne Yapar

Türkçe LLM çıktılarının kendine özgü bir kokusu var: `-mektedir` salgını, "sadece X değil, aynı zamanda Y" retoriği, "bu bağlamda / söz konusu / kritik bir rol oynamaktadır" bürokratik bağlaç zinciri, boş değerlendirici sıfat kümeleri, zorlama noktalı virgül köprüleri, "işte" ve "tam da" gibi İngilizce vurgu-doldurucuları, cümle sonlarına iliştirilen boş övgü kapanışları.

Bu skill Claude'a bu imzaları tanımayı ve temizlemeyi öğretir. Türkçenin kendi ritim geleneğini referans alır — Tanpınar, Ayfer Tunç, Barış Bıçakçı, Ahmet Ümit, İlber Ortaylı, Kemal Karpat, Çağlar Keyder gibi yerleşik yazarların cümle mimarisini.

Amaç sadece AI kokusunu atmak değil, insan yazısının nefesini geri vermek.

## Nasıl Çalışır

İki fazlı mimari:

- **Faz 1 (koşulsuz):** Metinden AI-üretimi yazım imzalarını çıkarır. Her metne uygulanır.
- **Faz 2 (koşullu):** Metnin registeri uygunsa insan Türkçesinin ritim imzalarını yerleştirir. Türe göre değişir.

Beş register kategorisi tanır: hukuki-idari, akademik-kurumsal, analitik-gazetecilik, deneme-blog, edebi-yaratıcı. Her registerde farklı davranır. KVKK sözleşmesine konuşma dili sokmaz, blog yazısına akademik ritim dayatmaz.

## Beş Mutlak Yasak

Bu beş yasak her metinde, her register'da, her koşulda uygulanır. TDK-uygun olsa bile skill çıktısında yer almaz:

1. Uzun çizgi (—)
2. Noktalı virgül
3. Yarım cümle
4. Bağlaçla açılan izole cümle
5. Karşıtlık bağlaçlarından önce virgül ("ama" öncesi virgül gibi)

## Yazar Profili Sistemi

Skill senin yazım imzanı öğrenebilir. Sohbet başında (register profil-uyumluysa) skill sana sorar: "Senin ritmini katmamı istersen kendi yazdığın 2-3 kısa metin paylaş."

Örnek verdiğinde skill senin nefes uzunluğunu, bağlaç tercihini, kavramlaştırma kalıplarını ve ritim imzalarını çıkarır. O sohbet boyunca kullanır. Yeni sohbette baştan sorulur.

Kişisel dosya sistemine erişim gerekmez. Her platformda çalışır.

## Kurulum

### Claude Desktop (en yaygın)

1. Bu repo'yu ZIP olarak indir (yeşil **Code → Download ZIP** butonu).
2. Claude Desktop → **Settings → Capabilities → Skills**.
3. **Add Skill** ile ZIP klasörünü yükle.
4. Skill aktif hale gelir.

### Claude Code CLI

```bash
cd ~/.claude/skills/
git clone https://github.com/bushrabeg/turkce-humanizer.git
```

### Kullanım

Claude'a bir Türkçe metin at ve şunu de:

> "Bu metni humanize et."

Ya da:

> "AI kokusunu at."

Skill devreye girer. Önce register'ı doğrular, register profil-uyumluysa profil sorar, sonra iki fazlı işleme başlar. Sonda beş bölümlü rapor sunar: tespit raporu, sinyal yoğunluğu, onarılmış metin, değişiklik özeti, notlar.

## Repo Yapısı

```
turkce-humanizer/
├── SKILL.md              # Ana skill dosyası
├── README.md             # Bu dosya
├── ROADMAP.md            # v3.0+ yol haritası
├── LICENSE               # MIT
├── docs/
│   ├── tdk-referanslari.md      # TDK'nın noktalama kurallarıyla ilişki
│   ├── register-detayli.md      # Beş register kategorisi detayı
│   └── mimari-kararlar.md       # Tasarım kararlarının gerekçesi
└── examples/
    └── gucli-cumleler.md        # Türkçe güçlü cümle referans korpusu
```

## Sürüm Geçmişi

- **v3.2** (Ağustos 2026): Sohbet-içi profil sistemi (Desktop uyumluluğu), Sinyal 23 soru varyantı.
- **v3.1** (Ağustos 2026): "İşlem başlamadan önce" bölümü, Sinyal 23 (şudur/budur), Sinyal 4/12/18 zenginleşmesi.
- **v3.0** (Ağustos 2026): Beş mutlak yasak, yazar profili sistemi, kullanıcıya danışma protokolü, değişiklik özeti, kabul oranı log'u.
- **v2.1** (Ağustos 2026): 22 sinyal, yapı koruma prensibi, nicel metrik.
- **v2.0** (Ağustos 2026): İki fazlı mimari, 5 register kategorisi, 19 sinyal.
- **v0.1** (Ağustos 2026): İlk sürüm, 10 sinyal.

Detaylı release notları için [Releases](https://github.com/bushrabeg/turkce-humanizer/releases) sayfasına bakılır.

## Katkı

Katkıya açık. Özellikle şu alanlarda:

- `examples/gucli-cumleler.md` — Türkçe güçlü cümle örnekleri (kitap alıntıları, gazete köşeleri, akademik pasajlar).
- Yeni AI-imzası tespiti — Türkçe LLM çıktısında fark ettiğin yeni bir kalıbı issue olarak açabilirsin.
- Register sınır durumları — belirsiz register durumları için `docs/register-detayli.md`'ye örnek eklenebilir.

Pull request açmadan önce SKILL.md'nin mimari kararlarına ([docs/mimari-kararlar.md](docs/mimari-kararlar.md)) göz atman öneririm.

## Referans

- **TDK Yazım Kuralları** — noktalama kararlarında temel referans.
- **ASD-STE100 (1986)** — nicel metrik ilhamı.
- **harshaneel/humanize, makotofalcon/humanizer-ja** — genel humanizer mimarisi ilhamı.
- **Türk edebiyat ve akademi geleneği** — ritim imzaları için stilometrik referans.

## Yazar

**Büşra Begçecanlı** ([@bushrabeg](https://github.com/bushrabeg)) — Anadolu Ajansı gazeteci ve araştırmacı, AA Kitap yayın birimi, Teknopolitika.com kurucu ortağı, intelligence studies alanında yüksek lisans. Skill'in taksonomisi, kalibrasyonu ve editör kararları kendi editörlük deneyimine dayanıyor.

## Lisans

MIT — özgürce kullan, değiştir, dağıt.
