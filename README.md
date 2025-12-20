# 🔍 Kripto Piyasa Analiz Botu

[![n8n](https://img.shields.io/badge/n8n-workflow-orange)](https://n8n.io)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Developer](https://img.shields.io/badge/Developer-xCodeWraith-purple)](https://github.com/xcodewraith)

> AI destekli otomatik kripto para piyasa analizi ve Telegram bildirimi sistemi

## 📋 İçindekiler

- [Genel Bakış](#-genel-bakış)
- [Özellikler](#-özellikler)
- [Gereksinimler](#-gereksinimler)
- [Kurulum](#-kurulum)
- [Yapılandırma](#️-yapılandırma)
- [Kullanım](#-kullanım)
- [Workflow Yapısı](#-workflow-yapısı)
- [API Referansları](#-api-referansları)
- [Maliyet Analizi](#-maliyet-analizi)
- [Sorun Giderme](#-sorun-giderme)
- [Lisans](#-lisans)

---

## 🎯 Genel Bakış

Bu n8n workflow'u, günde 2 kez otomatik olarak çalışarak:

1. **BTC, ETH, SOL ve XRP** için TradingView grafikleri oluşturur
2. Web'den **güncel kripto haberlerini** toplar
3. **AI ile analiz** yaparak anlamlı özetler üretir
4. Sonuçları **Telegram**'a bildirim olarak gönderir

### Kimler İçin?

- 📈 Kripto yatırımcıları
- 📊 Günlük piyasa takibi yapanlar
- 🤖 Otomasyon meraklıları
- 💼 Portföy yöneticileri

---

## ✨ Özellikler

| Özellik | Açıklama |
|---------|----------|
| 📊 **Grafik Analizi** | Chart-img API ile TradingView grafikleri |
| 🌐 **Haber Toplama** | BrowserAI ile web scraping |
| 🤖 **AI Analiz** | OpenRouter/GPT ile akıllı özetler |
| 📱 **Telegram Bildirimi** | Anlık bildirimler |
| ⏰ **Otomatik Zamanlama** | Günde 2 kez (08:00 & 20:00) |
| 🔄 **Hata Toleransı** | Otomatik yeniden deneme |

---

## 📋 Gereksinimler

### Yazılım

- **n8n** v1.0.0 veya üzeri
- Node.js 18+ (n8n için)

### API Anahtarları

| Servis | Amaç | Kayıt Linki |
|--------|------|-------------|
| **Chart-img** | Grafik görüntüleri | [chart-img.com](https://chart-img.com/account/api) |
| **BrowserAI** | Web scraping | [browser.ai](https://browser.ai/dashboard/page/account/tab/api_key) |
| **OpenRouter** | AI modeli | [openrouter.ai](https://openrouter.ai/keys) |
| **Telegram Bot** | Bildirimler | [@BotFather](https://t.me/BotFather) |

---

## 🚀 Kurulum

### Adım 1: Workflow'u İndirin

```bash
# Repository'yi klonlayın
git clone https://github.com/xcodewraith/n8n-workflows.git

# Klasöre gidin
cd n8n-workflows/kripto-piyasa-analiz-botu
```

### Adım 2: n8n'e Import Edin

1. n8n arayüzünü açın
2. **Settings** → **Import from File** seçin
3. `kripto-piyasa-analiz-botu.json` dosyasını seçin
4. **Import** butonuna tıklayın

### Adım 3: Credential'ları Ayarlayın

n8n'de şu credential'ları oluşturun:

#### Chart-img API
```
Credential Type: HTTP Bearer Auth
Token: YOUR_CHARTIMG_API_KEY
```

#### BrowserAI API
```
Credential Type: HTTP Bearer Auth
Token: YOUR_BROWSERAI_API_KEY
```

#### OpenRouter API
```
Credential Type: OpenRouter API
API Key: YOUR_OPENROUTER_API_KEY
```

#### Telegram Bot
```
Credential Type: Telegram API
Bot Token: YOUR_BOT_TOKEN
```

---

## ⚙️ Yapılandırma

### Telegram Chat ID

`Send a text message` node'unda `chatId` değerini ayarlayın:

```json
{
  "chatId": "YOUR_CHAT_ID"
}
```

Chat ID'nizi bulmak için:
1. Telegram'da [@userinfobot](https://t.me/userinfobot) ile mesajlaşın
2. `/start` gönderin
3. ID'nizi kopyalayın

### Zamanlama Ayarları

Varsayılan: Günde 2 kez (08:00 & 20:00 TR saati)

```
Cron: 0 5,17 * * *  (UTC)
```

Değiştirmek için `Schedule Trigger` node'unu düzenleyin.

### Desteklenen Kripto Paralar

Varsayılan:
- BTC/USD
- ETH/USD
- SOL/USD
- XRP/USD

Yeni coin eklemek için:
1. İlgili chart node'unu kopyalayın
2. Symbol'ü değiştirin (örn: `BINANCE:DOGEUSD`)
3. Merge node'larına bağlayın

---

## 📖 Kullanım

### Manuel Çalıştırma

1. n8n'de workflow'u açın
2. **Execute Workflow** butonuna tıklayın
3. Sonuçları Telegram'da görün

### Otomatik Çalıştırma

1. Workflow'u **Active** duruma getirin
2. Belirlenen saatlerde otomatik çalışacak

### Örnek Çıktı

```
Here's your half-day crypto update as of 19-December-2024:

💸 Bitcoin - Bitcoin rallied to $104k amid ETF inflows 
   exceeding $500M. Chart shows strong bullish structure.

💸 Ethereum - ETH consolidated at $3,800 while Pectra 
   upgrade timeline announced.

💸 Solana - SOL jumped 8% following new DeFi launches.

💸 XRP - XRP maintained $2.50 as Ripple developments continue.
```

---

## 🔄 Workflow Yapısı

```
┌─────────────────┐
│ Schedule Trigger│ ← Günde 2 kez
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Tarih Hesapla  │
└────────┬────────┘
         │
    ┌────┴────────────────────────┐
    │       PARALEL İŞLEM         │
    ▼                             ▼
┌─────────────┐            ┌─────────────┐
│ Chart-img   │            │ BrowserAI   │
│ (4 Grafik)  │            │ (Haberler)  │
└──────┬──────┘            └──────┬──────┘
       │                          │
       ▼                          ▼
┌─────────────┐            ┌─────────────┐
│ AI Grafik   │            │ Durum       │
│ Analizi     │            │ Kontrolü    │
└──────┬──────┘            └──────┬──────┘
       │                          │
       └──────────┬───────────────┘
                  ▼
           ┌─────────────┐
           │  AI Özet    │
           │  Oluşturma  │
           └──────┬──────┘
                  ▼
           ┌─────────────┐
           │  Telegram   │
           │  Gönder     │
           └─────────────┘
```

---

## 📚 API Referansları

### Chart-img API

```json
POST https://api.chart-img.com/v2/tradingview/advanced-chart

{
  "symbol": "BINANCE:BTCUSD",
  "interval": "1h",
  "width": 800,
  "height": 600,
  "style": "candle",
  "theme": "dark"
}
```

[Dokümantasyon](https://chart-img.com/docs)

### BrowserAI API

```json
POST https://browser.ai/api/v1/tasks

{
  "type": "crawler_automation",
  "instructions": [{
    "action": "Gather cryptocurrency news..."
  }]
}
```

[Dokümantasyon](https://browser.ai/docs)

---

## 💰 Maliyet Analizi

| Servis | Günlük Kullanım | Aylık Maliyet |
|--------|-----------------|---------------|
| Chart-img | 8 grafik | **Ücretsiz** |
| BrowserAI | 2 task | **Ücretsiz** |
| OpenRouter | ~4 çağrı | ~$3-5 |
| Telegram | Sınırsız | **Ücretsiz** |
| **Toplam** | | **~$5/ay** |

---

## 🔧 Sorun Giderme

### Chart-img Hatası

```
Error: 401 Unauthorized
```

**Çözüm:** API anahtarınızı kontrol edin, Bearer token olarak ayarlandığından emin olun.

### BrowserAI Timeout

```
Error: Task timeout
```

**Çözüm:** Wait sürelerini artırın (varsayılan: 45 saniye)

### Telegram Gönderim Hatası

```
Error: Chat not found
```

**Çözüm:** Bot'u gruba ekleyin veya Chat ID'yi kontrol edin.

---

## ⚠️ Yasal Uyarı

> Bu workflow yalnızca **kişisel kullanım** içindir ve **yatırım tavsiyesi** niteliği taşımaz. Bu aracı kullanarak yapılan işlemlerden kullanıcı sorumludur.

---

## 📄 Lisans

MIT License - Detaylar için [LICENSE](LICENSE) dosyasına bakın.

---

## 👤 Geliştirici

**xCodeWraith (DEV)**

- GitHub: [@xcodewraith](https://github.com/xcodewraith)

---

## 🙏 Teşekkürler

- [n8n](https://n8n.io) - Workflow otomasyon platformu
- [Chart-img](https://chart-img.com) - TradingView grafik API
- [BrowserAI](https://browser.ai) - AI web scraping
- [OpenRouter](https://openrouter.ai) - AI model erişimi
