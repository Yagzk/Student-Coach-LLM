
# 🧠 LLM Tabanlı Öğrenci Koçu – Embedding Destekli Soru Cevap Sistemi

Bu proje, **Large Language Model (LLM)** ve **vektör embedding** tekniklerini kullanan, **kendini genişletebilen** bir yapay zekâ tabanlı soru–cevap sistemidir.

Sistem; kullanıcıdan gelen doğal dil sorularını embedding’e dönüştürerek bir **vektör veritabanında (ChromaDB)** en uygun cevapla eşleştirir.  
Eğer yeterli eşleşme bulunamazsa, **OpenAI tabanlı bir LLM** kullanarak yeni cevap üretir ve bu cevabı veritabanına ekleyerek **kendini günceller**.

Proje; **LLM destekli retrieval (RAG-benzeri)** mimariyi, geri bildirim döngüsü ve quiz modülü ile birleştirir.

---

## 🎯 Projenin Amacı

- Embedding tabanlı bilgi erişimi (semantic search)
- LLM ile dinamik bilgi üretimi
- Kullanıcı geri bildirimiyle genişleyen bilgi tabanı
- Eğitim odaklı, ölçülebilir öğrenme çıktıları (quiz)
- Çoklu arayüz desteği (CLI, API, Telegram)

---

## 🧠 Kullanılan Yapay Zekâ Yaklaşımı

### 🔹 Embedding Modeli
- `paraphrase-multilingual-MiniLM-L12-v2`
- Çok dilli, semantik benzerlik odaklı
- Cosine similarity ile eşleşme

### 🔹 Vektör Veritabanı
- **ChromaDB**
- Kalıcı embedding saklama
- Metadata destekli (soru, cevap, quiz, feedback)

### 🔹 LLM Entegrasyonu
- **OpenAI GPT modeli**
- Düşük eşleşme durumunda:
  - Yeni cevap üretimi
  - Otomatik veritabanı güncelleme
- Kontrollü prompt (sadece ML soruları)

---

## 🧩 Sistem Mimarisi 

[Kullanıcı Sorusu]
        ↓
[Text Normalization]
        ↓
[Sentence Embedding]
        ↓
[ChromaDB Similarity Search]
        ↓
        +-----------------------------+
        |     Benzerlik Skoru?        |
        +--------------+--------------+
                       |
           +-----------+-----------+
           |                       |
           v                       v
+---------------------+   +--------------------------+
|  Yüksek Benzerlik   |   |   Düşük Benzerlik        |
+----------+----------+   +-----------+--------------+
           |                          |
           v                          v
+---------------------+   +--------------------------+
|  Mevcut Cevap       |   |  LLM ile Yeni Cevap      |
+----------+----------+   +-----------+--------------+
           |                          |
           v                          v
+---------------------+   +--------------------------+
|  Kullanıcıya Gönder |   |  Embedding Oluştur       |
+---------------------+   +-----------+--------------+
                                      |
                                      v
                           +--------------------------+
                           |  ChromaDB'ye Kaydet      |
                           +-----------+--------------+
                                      |
                                      v
                           +--------------------------+
                           |  Kullanıcıya Yeni Cevap  |
                           +--------------------------+




---

## 🚀 Özellikler

- 🧠 Embedding tabanlı semantik arama
- 🤖 LLM destekli dinamik cevap üretimi
- 📦 Kalıcı vektör veritabanı (ChromaDB)
- 🔁 Geri bildirim döngüsü (self-improving system)
- 🧪 Quiz modu (öğrenme ölçümü)
- 📧 Eğitmene otomatik e-posta raporları
- 🌐 Çoklu arayüz desteği:
  - CLI
  - REST API (Flask)
  - Telegram arayüzü

---

##🧪 Quiz Mekanizması

Kullanıcının sorduğu ML sorularından dinamik olarak oluşturulur

Embedding benzerliği ile cevap doğruluğu ölçülür

Sonuçlar eğitmene e-posta ile raporlanır

---

## 📁 Proje Yapısı

```text
.
├── embedding_manager.py    # LLM + embedding + retrieval ana mantığı
├── chroma_storage.py       # ChromaDB erişim ve kayıt katmanı
├── bot.py                  # Telegram arayüzü (frontend katmanı)
├── app.py                  # REST API / webhook
├── main.py                 # CLI arayüzü
├── populate_chroma.py      # Başlangıç veri setini DB’ye aktarma
├── populated_keywords.py   # ML keyword koleksiyonu
├── chromagor.py            # DB inspection / debug aracı
├── chroma_db/              # (repo dışı) vektör veritabanı
├── .env                    # (repo dışı) API anahtarları
└── README.md
```

#Gerekli Kütüphaneler
pip install sentence-transformers chromadb flask python-dotenv openai python-telegram-bot

---

#.env için:
OPENAI_API_KEY=...
TELEGRAM_BOT_TOKEN=...

MAIL_HOST=...
MAIL_PORT=587
MAIL_USER=...
MAIL_PASS=...
MAIL_TO=...
