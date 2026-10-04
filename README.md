# PDF Soru-Cevap Asistanı (RAG + LLM)

Bir PDF belgesine doğal dilde soru sorabileceğiniz, Retrieval-Augmented Generation (RAG) tabanlı bir çalışma asistanı. [Haystack](https://haystack.deepset.ai/) framework'ü kullanılarak geliştirilmiştir.

Sistem, yüklenen bir PDF'i parçalara ayırır, bu parçaları vektör temsillerine (embedding) çevirir ve bir vektör deposunda indeksler. Kullanıcı soru sorduğunda, soruyla en alakalı parçalar bulunur ve bir Büyük Dil Modeli'ne (LLM) verilerek yalnızca belgedeki bilgilere dayalı bir cevap üretilir.

## Nasıl Çalışır?

RAG mimarisi iki aşamadan oluşur:

**1. İndeksleme (Ingestion)**
PDF okunur → metin temizlenir → cümlelere bölünür → her parça embedding'e çevrilir → vektör deposuna yazılır.

**2. Sorgulama (Retrieval + Generation)**
Kullanıcının sorusu embedding'e çevrilir → vektör deposundan en benzer parçalar çekilir → bu parçalar ve soru bir prompt şablonuyla birleştirilip LLM'e verilir → cevap üretilir.

Bu yaklaşım, modelin yalnızca verilen belgeden cevap vermesini sağlayarak halüsinasyonu (uydurma cevap) azaltır.

## Kullanılan Teknolojiler

- **Haystack** — RAG pipeline orkestrasyonu
- **Sentence-Transformers** (`all-MiniLM-L6-v2`) — metin embedding'leri
- **InMemoryDocumentStore** — vektör deposu
- **OpenAI / Google Gemini** — cevap üreten LLM
- **PyPDF** — PDF metin çıkarımı

## Kurulum

```bash
pip install -r requirements.txt
```

## Kullanım

1. Notebook'u Google Colab'da veya yerel bir Jupyter ortamında açın.
2. API anahtarınızı bir ortam değişkeni olarak tanımlayın (aşağıdaki güvenlik notuna bakın).
3. İndeksleme hücresini çalıştırarak PDF'i belleğe alın.
4. Sorgu hücresinde `query` değişkenini değiştirerek sorularınızı sorun.

### Örnek Sorular

```python
query = "Belgede hangi programlama dillerinden bahsediliyor?"
query = "Bu kişinin eğitim geçmişi nedir?"
query = "Hangi projeler anlatılmış?"
```

## Güvenlik Notu

API anahtarlarınızı **asla** doğrudan koda yazmayın veya bu repoya yüklemeyin. Anahtarlar ortam değişkenleri üzerinden okunur:

```python
import os
os.environ["OPENAI_API_KEY"] = "..."  # Colab'da userdata, yerelde .env kullanın
```

Google Colab kullanıyorsanız, anahtarları "Gizli Anahtarlar" (Secrets) panelinden ekleyin ve koddan şöyle çağırın:

```python
from google.colab import userdata
os.environ["OPENAI_API_KEY"] = userdata.get("OPENAI_API_KEY")
```

## Geliştirme Fikirleri

- Çok dilli embedding modeli (`paraphrase-multilingual-MiniLM-L12-v2`) ile Türkçe sorgu kalitesini artırmak
- Cevaplarda kaynak sayfa referansı göstermek
- Kalıcı bir vektör veritabanına (ChromaDB, Qdrant) geçmek
- Web arayüzü (FastAPI + React) eklemek
- Yüklenen içerikten otomatik quiz üretmek

## Lisans

MIT
