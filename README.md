### Proje: Tarif Sınıflandırma (Transformer Modelleri ile)

Bu proje, yemek tariflerindeki malzeme listelerini kullanarak tariflerin farklı mutfak türlerine sınıflandırılmasını amaçlar. Projede BERT, RoBERTa, DeBERTa ve ALBERT gibi popüler **Transformer** modelleri kullanılmıştır.  

---

## Özellikler
- XML formatındaki tarif verilerinin işlenmesi ve sınıflandırma için hazırlanması.
- Malzeme listelerine göre otomatik etiketleme (Türk, Çin, İtalyan vb.).
- Hugging Face modelleri ile eğitim, değerlendirme ve karşılaştırma.
- Performans analizi için karmaşıklık matrisi, sınıflandırma raporu ve ROC eğrisi.

---

## Kullanılan Teknolojiler
- **Python**  
  - Pandas, XML (ElementTree), Matplotlib, Seaborn, Scikit-learn, PyTorch
- **Hugging Face Transformers**  
  - Model eğitimi ve değerlendirme
- **Google Colab**  
  - Eğitim ve test ortamı

---

## Kurulum

1. **Gerekli kütüphaneleri yükleyin**  
   ```bash
   pip install pandas matplotlib seaborn scikit-learn torch transformers datasets
   ```

2. **XML Dosyasını Yükleyin**  
   - `Bütün Tarifler.xml` dosyasını proje dizinine ekleyin. Dosya, malzeme listelerini `Ingredients > Ingredient` formatında içermelidir.

3. **Google Colab veya Yerel Ortamda Çalıştırın**  
   - Projeyi Google Colab üzerinde çalıştırabilir veya yerel bir Python ortamında çalıştırabilirsiniz.

---

## Kullanım

1. **Veri İşleme**  
   - XML dosyasından tarif verileri alınır ve Pandas DataFrame formatına dönüştürülür.  
   - Malzemelere göre mutfak türleri otomatik olarak etiketlenir (Türk, Çin, İtalyan, vb.).

2. **Model Eğitimi**  
   - **Transformer Modelleri**: BERT, RoBERTa, DeBERTa, ALBERT.  
   - Eğitim ve test verileri otomatik olarak bölünür (%80 eğitim, %20 test).  
   - Hugging Face `Trainer` sınıfı ile model eğitimi yapılır.

3. **Değerlendirme**  
   - Sınıflandırma raporu, karmaşıklık matrisi ve ROC eğrisi oluşturulur.  
   - Modellerin doğruluk (accuracy), F1-score gibi metrikleri karşılaştırılır.

---

## Çıktılar

- **Karmaşıklık Matrisi:** Sınıflandırma sonuçlarını görselleştirir.  
- **ROC Eğrisi:** Modellerin performansını karşılaştırır.  
- **Sınıflandırma Raporu:** Precision, recall ve F1-score değerlerini gösterir.

---

## Örnek Kod Kullanımı

```python
# Veri İşleme
tree = ET.parse("Bütün Tarifler.xml")
root = tree.getroot()

# DataFrame'e Dönüşüm
data = []
for ingredients in root.findall('Ingredients'):
    malzemeler = [ingredient.text for ingredient in ingredients.findall('Ingredient') if ingredient.text]
    data.append(" ".join(malzemeler))

df = pd.DataFrame(data, columns=['malzemeler'])

# Etiketleme
def etiket_olustur(malzeme_listesi):
    if any(keyword in malzeme_listesi.lower() for keyword in ["yoğurt", "bulgur", "sumak"]):
        return "Turk"
    return "Other"

df['etiket'] = df['malzemeler'].apply(etiket_olustur)

# Model Eğitimi
tokenizer = BertTokenizer.from_pretrained("bert-base-uncased")
model = BertForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)

# Tokenizasyon
def tokenize_function(examples):
    return tokenizer(examples['malzemeler'], padding="max_length", truncation=True)

dataset = Dataset.from_pandas(df)
tokenized_dataset = dataset.map(tokenize_function, batched=True)
```

---

## Katkıda Bulunun
- Bu projeyi geliştirmek veya katkıda bulunmak isterseniz pull request gönderebilirsiniz.  
- Daha fazla veri veya mutfak türü eklemek için katkılarınızı bekliyoruz.

---

## Lisans
Bu proje MIT Lisansı ile lisanslanmıştır.  
Detaylar için `LICENSE` dosyasına göz atabilirsiniz.
