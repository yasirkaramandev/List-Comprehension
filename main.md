# List Comprehension

Bu blogta List Comprehension kullanımı ve amaçları hakkında bilgi vereceğim. 

**İçindekiler**:

- [List Comprehension Nedir?](#list-comprehension-nedir)
    + [Syntax Kuralları](#syntax-kurallari)
    + [İleri Seviye Kullanım](#ileri-seviye-kullanim)
        + [Performans Avantajları](#performans-avantajlari)
        + [Okunabilirlik ve Kısalık](#okunabilirlik-ve-kisalik)
        + [Karmaşık Veri Yapıları ile Çalışma](#karmasik-veri-yapilari-ile-calisma)
- [Uygulama Alanları](#uygulama-alanlari)
    - [Veri Analizi ve İşleme](#veri-analizi-ve-isleme)
    - [Web Scraping](#web-scraping)
    - [Metin İşleme](#metin-i̇şleme)
- [Sonuç ve Bitiş](#sonuc-ve-bitis)

## List Comprehension Nedir?
List Comprehension yazılımdaki uç noktalardan biridir diyebiliriz asıl amacı kısa kod satırlarıyla büyük işler yapmayı sağlamaktır. Bu blogta nasıl yapılacağını iyi bir şekilde öğreneceksiniz! Ufak bir örnek ile başlayalım. cars Listemizdeki araçları içerisinde "a" olanları a Listemize ekleyelim. 


```python

cars = ["Porsche","Ferrari","Alfa Romeo","Bentley","Audi","BMW","Mercedes"]

a = []

for i in cars:
    if "a" in i.lower():    # .lower() Büyük harfleri küçüğe çevirmesi için.
        a.append(i)

print(a)    # Çıktı : ['Ferrari', 'Alfa Romeo', 'Audi']
```

Örnekte içerisinde "a" harfi olan bütün araba isimlerini a isimli Listemize attık. Bunu toplamda 3 satırda yaptık evet 3 satır gerçekten az gibi görünüyor ancak bunun başlangıç seviye bir örnek olduğunu unutmayın daha zorlu problemlerde performansı etkileyebilecek kadar büyük sıkıntılar çekebilirsiniz kısaca bu sizi alıştıracak basit bir örnek. Şimdide bu örneğin aynısını List Comprehension ile yapalım.


```python
cars = ["Porsche","Ferrari","Alfa Romeo","Bentley","Audi","BMW","Mercedes"]

a = [i for i in cars if "a" in i.lower() ]

print(a)    # Çıktı : ['Ferrari', 'Alfa Romeo', 'Audi']
```

Evet ilk List Comprehension örneğimiz. Yazımın bu kısmına kadar daha anlatmaya geçmedim çünkü fikrimce en verimli öğrenme yolu örnek üzerinden öğrenmektir. Kodu incelediğimizde az önce 3 satırda hallettiğimiz kısmı şuan 1 satırda halletmiş olduk. Peki bunu nasıl yaptık? 

### Syntax Kurallari.
Python'da liste üretimleri (List Comprehensions) güçlü ve okunabilir bir yol sağlar. Bu özellik, for döngülerini ve koşullu ifadeleri tek bir satırda kullanarak yeni listeler oluşturmanıza olanak tanır. İşte Python'da liste üretimlerinin temel sözdizimi ve kullanımı:


```python
[expression for item in iterable]
```

Burada:

- **expression**: Her öğe için uygulanacak ifade.
- **item**: Döngüdeki her bir öğe.
- **iterable**: Üzerinde iterasyon yapılacak koleksiyon (örneğin, bir liste, dizi, set veya sözlük).

Basit ama kavrayıcı bir kaç örnek:

**Basit Liste Oluşturma**

numbers listesindeki her sayının karesini alma işlemi.


```python
numbers = [1, 2, 3, 4, 5]
squares = [n**2 for n in numbers]
print(squares)  # Çıktı: [1, 4, 9, 16, 25]

```

**Koşullu Liste Üretimi**

Bu örnekte numbers ta bulunan sayılardan 2 ye bölünenlerin karelerini almaları sağlanır.


```python
numbers = [1, 2, 3, 4, 5]
even_squares = [n**2 for n in numbers if n % 2 == 0]
print(even_squares)  # Çıktı: [4, 16]

```

**İç İçe Döngüler ile Liste Üretimi**

Bu örnekte 3x3 lük matrix'i tek bir listeye dönüştürdük.


```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat_list = [num for row in matrix for num in row]
print(flat_list)  # Çıktı: [1, 2, 3, 4, 5, 6, 7, 8, 9]

```

**Koşullu İfade ile Liste Üretimi**

Bu örnekte sayıların 2 ye bölündüğününü kontrol ediyor eğer 2 ye bölünmüyorsa küpünü alıyor bölünüyorsa karesini alıyor.


```python
numbers = [1, 2, 3, 4, 5]
result = [n**2 if n % 2 == 0 else n**3 for n in numbers]
print(result)  # Çıktı: [1, 4, 27, 16, 125]

```

Bu temel örnekler olayı kavramanızda yardımcı olduğunu düşünüyorum bu 4 örnek en çok rastladığımız List Comprehension örnekleridir. Biraz daha derinlemesine bakalım.

### Ileri Seviye Kullanim.
Python'da liste üretimleri (List Comprehensions), ileri düzey kullanımlarda önemli avantajlar ve esneklikler sağlar. Bu teknik, veri işleme, performans iyileştirme ve kod okunabilirliğini artırma gibi birçok alanda kullanılır. İşte ileri düzey kullanımlarının bazı faydaları ve neden tercih edildiklerine dair detaylar:

#### Performans Avantajlari

Liste üretimleri, genellikle geleneksel döngülere kıyasla daha hızlıdır çünkü döngüler ve liste ekleme işlemleri tek bir ifadede optimize edilir. Bu, özellikle büyük veri kümeleri üzerinde çalışırken önemli performans kazanımları sağlar. Şimdi bunu 2 kod bloğu ile test edelim.


```python
import time

# Büyük bir liste oluşturma
large_list = list(range(100000))

# Liste üretimi ile zaman ölçümü
start_time = time.time()
squares = [x**2 for x in large_list]
end_time = time.time()
liste_sure = end_time - start_time
print("Liste üretimi süresi:", liste_sure)

# Geleneksel döngü ile zaman ölçümü
start_time = time.time()
squares = []
for x in large_list:
    squares.append(x**2)
end_time = time.time()
geleneksel_sure = end_time - start_time
print("Geleneksel döngü süresi:", geleneksel_sure)

print("Liste Üretim - Geleneksel Döngü = ", liste_sure - geleneksel_sure)

```

#### Okunabilirlik ve Kisalik

Liste üretimleri, karmaşık döngü ve koşul yapılarının tek bir satırda yazılmasına olanak tanır, bu da kodun daha kısa ve okunabilir olmasını sağlar.


```python
# Geleneksel döngü
result = []
for x in range(10):
    if x % 2 == 0:
        result.append(x**2)
print(result)  # Çıktı: [0, 4, 16, 36, 64]

# Liste üretimi ile
result = [x**2 for x in range(10) if x % 2 == 0]
print(result)  # Çıktı: [0, 4, 16, 36, 64]

```

#### Karmasik Veri Yapilari ile Calisma
Liste üretimleri, iç içe geçmiş veri yapıları (örneğin, listeler, sözlükler) ile etkili bir şekilde çalışmak için kullanılabilir. Bu, çok boyutlu verileri düzleştirmek veya filtrelemek için idealdir.


```python
# 2 boyutlu listeyi düzleştirme
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat_list = [item for sublist in matrix for item in sublist]
print(flat_list)  # Çıktı: [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Sözlük içindeki değerlere uygulama
dict_data = {'a': 1, 'b': 2, 'c': 3}
squared_values = {k: v**2 for k, v in dict_data.items()}
print(squared_values)  # Çıktı: {'a': 1, 'b': 4, 'c': 9}

```

## Uygulama Alanlari.
List Comprehensions, Python programlamada geniş bir kullanım alanına sahiptir ve çeşitli uygulamalarda önemli avantajlar sunar. İşte List Comprehensions'ın öne çıktığı bazı yaygın uygulama alanları:

### Veri Analizi ve Isleme

Veri analizi ve işleme, List Comprehensions'ın en yaygın kullanıldığı alanlardan biridir. Büyük veri setleri üzerinde hızlı ve etkili bir şekilde işlem yapmayı sağlar. Verilerin filtrelenmesi, dönüştürülmesi ve özetlenmesi gibi işlemler, List Comprehensions kullanılarak daha kısa ve okunabilir kodlarla gerçekleştirilebilir.


```python
# Yaş verilerinin olduğu bir liste
ages = [15, 23, 42, 18, 36, 27, 19]

# 18 yaş üstündekileri filtreleme
adults = [age for age in ages if age >= 18]
print(adults)  # Çıktı: [23, 42, 18, 36, 27, 19]

```

Bu örnekte, yaş listesinde 18 yaş ve üzerindeki bireyler filtrelenerek yeni bir liste oluşturuluyor. Bu tür işlemler, büyük veri setleri üzerinde çalışırken çok daha karmaşık olabilir ve List Comprehensions, bu tür durumlarda kodun kısa ve etkili olmasını sağlar.

### Web Scraping
Web scraping, web sayfalarından veri toplama işlemidir ve List Comprehensions, çekilen verileri temizlemek ve düzenlemek için ideal bir araçtır. HTML veya XML gibi yapılandırılmış veriler üzerinde işlem yaparak, gerekli bilgileri hızlı ve verimli bir şekilde elde etmenizi sağlar.


```python
import requests
from bs4 import BeautifulSoup

# Bir web sayfasından veri çekme
response = requests.get('https://example.com')
soup = BeautifulSoup(response.text, 'html.parser')

# Sayfadaki tüm bağlantıları listeleme
links = [a['href'] for a in soup.find_all('a', href=True)]
print(links)

```

Bu örnekte, requests ve BeautifulSoup kullanılarak bir web sayfasından tüm bağlantılar (linkler) çekiliyor ve List Comprehensions ile bu bağlantılar bir listeye ekleniyor. Bu, veri toplama ve temizleme işlemlerini basitleştirir ve hızlandırır.

### Metin İşleme
Metin verilerini işlerken, List Comprehensions kullanarak metinleri temizlemek, filtrelemek ve dönüştürmek mümkündür. Bu, özellikle doğal dil işleme (NLP) ve veri temizleme süreçlerinde çok faydalıdır.




```python
# Metinlerin olduğu bir liste
texts = ["Hello World!", "Python is great.", "List comprehensions are powerful."]

# Metinleri küçük harfe çevirme ve noktalama işaretlerini kaldırma
cleaned_texts = [''.join(char.lower() for char in text if char.isalnum() or char.isspace()) for text in texts]
print(cleaned_texts)
# Çıktı: ['hello world', 'python is great', 'list comprehensions are powerful']

```

Bu örnekte, metinler küçük harfe dönüştürülüp noktalama işaretlerinden arındırılarak temizleniyor. Metin işleme işlemlerinde List Comprehensions kullanarak daha kısa ve anlaşılır kodlar yazabilirsiniz.

## Sonuc ve Bitis
List Comprehensions, Python'da çeşitli alanlarda kodun daha verimli ve okunabilir olmasını sağlar. Veri analizi, web scraping, bilimsel hesaplamalar ve metin işleme gibi çeşitli uygulamalarda kullanım kolaylığı sunar. Bu sayede, karmaşık işlemleri daha az kodla gerçekleştirebilir ve performans avantajları elde edebilirsiniz. List Comprehensions kullanarak Python'da daha etkili ve okunabilir kodlar yazabilirsiniz, bu da projelerinizde daha hızlı ve verimli sonuçlar almanıza yardımcı olur.

List Comprehensions'ın sunduğu kısa ve anlaşılır kod yazma yeteneği, Python'un gücünü ve esnekliğini artırır. Veri işleme ve analizden web scraping'e, bilimsel hesaplamalardan metin işlemeye kadar birçok alanda List Comprehensions'ın avantajlarından yararlanabilirsiniz. Bu teknik, Python programlamada ustalaşmanıza ve daha temiz, daha etkili kodlar yazmanıza yardımcı olacaktır.

Başka yazılarda görüşmek üzere! [Yasir Karaman](https://yasirkaraman.medium.com/)
