# Java 8'den Sonra Gelen Değişiklikler

Java Deveplopment Kit'in <abbr title="Long-Term Support">LTS</abbr> sürümlerinde gelen değişiklileri listelediğim çalışmadır.

## Java 11 Değişiklikleri

### Genel Değişiklikler
- Java'nın resmi yazılım geliştirme kiti (Oracle JDK) artık lisanslanmadan ticari amaçla kullanılamıyor. Onun yerine OpenJDK kullanılabilir.
- *JavaFX* arayüz çatısı artık standart kütüphanenin bir parçası değil.
- *Java EE* ve *CORBA* modülleri kaldırıldı.

### Dil Özelliklerindeki Değişiklikler
- `var` anahtar kelimesi eklendi. Uzun değişken tanımlamalarında kendini tekrar etme durumunun önüne geçildi.
  - . .
    ```txt
    HashMap<String, String> dictionary = new HashMap<String, String>();
    ```

    yerine
    
    ```java
    var dictionary = new HashMap<String, String>();
    ```
- *Lambda* ifadelerinde yerel değişken tanımlaması benzeri tanımlamalara destek verildi. Bu sayede *lambda* parametlerelerinde *annotation* yapıları kullanılabilir oldu:  
  - . . `(@Deprecated var it) -> it.toUpperCase()`

### Standart Kütüphanedeki Değişiklikler
- `String` nesnelerine yeni metotlar eklendi:
    - `#.isBlank()` -> `bool`:  
      *String* nesnesinin boş olup olmadığını döndürür.
    - `#.lines()` -> ` java.util.stream.Stream<String>`:  
      Nesneyi satır ayraçlarından bölerek bir `Stream` nesnesi içerisine yerleştirir. Sonrasında Java 8 ile gelen *Stream API* özelliklerinden yararlanılabilir.
    - `#.strip()` -> `String`:
       *String* nesnesinin başındaki ve sonundaki boşlukları kırpar. `trim` metodundan farklı olarak *Unicode* standardında tanımlanan boşluk karakterlerini de tanır.
    - `#.stripLeading()` -> `String`:
      `strip` ile aynı ama sadece başlangıç kısmında işlem yapar.
    - `#.repeat(int)` -> `String`:
      Karakter dizisini belirtilen sayıda tekrar eder.
- Standart kütüphaneye HTTP istemcisi eklendi. `java.net.http` paketinde.
  - . .
    ```java
    HttpClient httpClient = HttpClient.newBuilder()
            .version(HttpClient.Version.HTTP_2)
            .connectTimeout(Duration.ofSeconds(20))
            .build();
    HttpRequest httpRequest = HttpRequest.newBuilder()
            .GET()
            .uri(URI.create("http://www.example.com"))
            .build();
    HttpResponse httpResponse = httpClient.send(httpRequest, HttpResponse.BodyHandlers.ofString());
    System.out.println(httpResponse.body());
    ```
- Standart kütüphaneye dosya içeriğini `String` biçiminde okuma ve yazmayı sağlayan yardımcı fonksiyonlar eklendi. `java.io.Files` sınıfına ait `readString` ve `writeString` fonksiyonları.
- Koleksiyon sınıflarına *Generics* kullanan `toArray(IntFunction<T[]>)` metodu eklendi. Bu sayede anlamsız bir nesne oluşturmadan dizinin tipi belirlenebiliyor.
  - . .
    ```java
    String[] arr = list.toArray(new String[1]);
    ```
    yerine
    
    ```java
    String[] arr = list.toArray(String[]::new);
    ```

### Teknik Araçlardaki Değişiklikler
- Java komut satırı derleyicisi artık uygulamayı tek aşamada kaynak koddan derleyip çalıştırabiliyor:  
  - . . `java App.java`
  - (Java 11 öncesinde `javac` ile CLASS dosyası olarak derleyip onun ardından `java` ile çalıştırmak şarttı.)
- Java için bir performans profilleme yazılımı olan "Java Flight Recorder" artık açık kaynak kodlu ve OpenJDK içerisinde yer alıyor. (Önceden Oracle'ın sahipli yazılımıydı.)
- Epsilon adında, sadece hafızada yer ayırma işlemi yapıp "çöp toplama" işlemini yapmayan *"No-Op"* bir *garbage collector* eklendi. Bazı test ortamları için yararlı olabilecek deneysel bir özellik.

### Diğer Değişiklikler
- *Transport Layer Security* protokolü [standardın](https://www.rfc-editor.org/info/rfc8446) 1.3 versiyonuna güncellendi. TLS kullanan uygulamalarda güvenlik ve performans artışı sağlamak amaçlanıyor.
- "ZGC" adında Düşük gecikmeli bir *garbage collector* eklendi.
- Unicode 10 standardı desteklenmeye başlandı. {&#x20BF;}



## Java 17 Değişiklikleri

### Dildeki değişiklikler
- `instanceof` işleci için *pattern matching* özelliği eklendi. *Type cast* için gereksiz kod yazılmasının önüne geçildi:
  - . .
    ```java
    if (obj instanceof String) {
        String s = (String) obj;
        ...
    }
    ```
    
    yerine

    ```java
    if (obj instanceof String s) {
        ...
    }
    ```

- Metin bloğu desteği eklendi. Çok satırlı *string* ifadeleri artık (`"""`) arasına alınarak daha okunaklı yazılabiliyor:
  ```java
  String html = """
                <html>
                    <head>
                        <title>Gömülü HTML Kodu Örneği</title>
                    </head>
                    <body>
                        <p>Merhaba Dünya!</p>
                    </body>
                </html>
                """;
  ```

- "Sealed class" kavramı eklendi. Eğer bir sınıftan türetilebilecek alt sınıfların belirlemeye ihtiyaç varsa bunu artık sınıf tanımında `sealed` ve `permits` anahtar kelimelerini kullanarak yapabiliyoruz:  
  ```java
  public abstract sealed class Sekil permits Daire, Kare, Dikdortgen {
      ...
  }
  ```

  `Sekil` sınıfını sadece `Daire`, `Kare` ve `Dikdortgen` sınıfları genişletebilir (`extends` anahtar kelimesi).

  > **Not:** Alt sınıflar tanımlanırken `final`, `sealed` ya da `non-sealed` ifadelerinden birinin kullanılması gerekiyor.

- Yeni çeşit `switch` ifadeleleri eklendi. `case` kelimesinden sonra "`:`"  yerine "`->`" işleci kullanılarak daha okunaklı kod yazılabiliyor. (Yeni `switch` ifadelerinde `break` anahtar kelimesi kullanılmıyor.)
  - . .
    ```java
    switch (gun) {
        case PAZARTESI:
        case CUMA:
        case PAZAR:
            System.out.println(6);
            break;
        case SALI:
            System.out.println(7);
            break;
        case PERSEMBE:
        case CUMARTESI:
            System.out.println(8);
            break;
        case CARSAMBA:
            System.out.println(9);
            break;
    }
    ```
    yerine

    ```java
    switch (gun) {
        case PAZARTESI, CUMA, PAZAR -> System.out.println(6);
        case SALI                   -> System.out.println(7);
        case PERSEMBE, CUMARTESI    -> System.out.println(8);
        case CARSAMBA               -> System.out.println(9);
    }
    ```
  Ayrıca bu `switch` ifadeleri *expression* statüsünde olduğu için değer döndürecek şekilde de kullanılabilir:
  - . .
   ```java
   int harfSayisi = switch (gun) {
        case SALI, CUMA           -> 4;
        case PAZAR                -> 5;
        case CARSAMBA, PERSEMBE   -> 8;
        case PAZARTESI, CUMARTESI -> 9;
  };
   ```

- *Record* sınıfları eklendi. Artık, veri taşımakla yükümlü sınıflar `record` kelimesi kullanılarak daha zahmetsiz oluşturulabiliyor.
  - . .
    ```java
    public final class Dikdortgen {
        private final double genislik;
        private final double yukseklik;

        public Dikdortgen(double genislik, double yukseklik) {
            this.genislik = genislik;
            this.yukseklik = yukseklik;
            if (genislik <= 0 || yukseklik <= 0) {
                throw new java.lang.IllegalArgumentException(
                    String.format("Geçersiz uzunluklar: %f, %f", genislik, yukseklik));
        }

        double genislik() { return this.genislik; }
        double yukseklik()  { return this.yukseklik; }


        // Tanımlanması gereken metotlar...
        public boolean equals...
        public int hashCode...
        public String toString() {...}
    }
    ```

    yerine

    ```java
    record Dikdortgen(double genislik, double yukseklik) {
        public Dikdortgen(double genislik, double yukseklik) {
            if (genislik <= 0 || yukseklik <= 0) {
                throw new java.lang.IllegalArgumentException(
                    String.format("Geçersiz uzunluklar: %f, %f", genislik, yukseklik));
            }
        }
    }
    ```

    > **Not:** Hatadan kaçınmak için *record* sınıflarında *constructor* tanımlarken parametre listesi atlanabilir. Yukarıdaki örnek şu şekilde de yazılabilir:
    > ```java
    > record Dikdortgen(double genislik, double yukseklik) {
    >     public Dikdortgen {
    >         ...
    >     }
    > }
    > ```

- `switch` ifadeleri için *"pattern matching"* desteği getirildi. Bu bazı koşullu ifadeleri daha okunaklı hale getirecek bir özellik. Örneğin tipini bilmediğimiz bir nesnenin sayı cinsinden değerini almak için şöyle bir fonksiyon yazabiliriz:  
  ```java
  static double getNumberValue(Object o) {
      double result;

      if (o instanceof Integer) {
          result = ((Integer) o).doubleValue();
      } else if (o instanceof Float) {
          result = ((Float) o).doubleValue();
      } else if (o instanceof String) {
          result = Double.parseDouble(((String) o));
      } else {
          result = 0;
      }

      return result;
  }
  ```
  Burada istediğimiz sonuca ulaşmak için her seferinde nesnenin tipini sorgulayıp kullanım sırasında da *type cast* işlemi yapmamız gerekti. (`o.doubleValue()` yerine `((Integer) o).doubleValue()` gibi.)

  Aynı fonksiyonu ***pattern matching*** kullanan `switch` ifadesinden yararlanarak şöyle yazabiliriz:
  ```java
  static double getNumberValue_switch(Object o) {
      return switch (o) {
          case Integer i -> i.doubleValue();
          case Float f -> f.doubleValue();
          case String s -> Double.parseDouble(s);
          default -> 0;
      };
  }
  ```
  Burada *type cast* işlemi koşul kontrolünde gerçekleştirildiğinden dolayı nesnenin metotlarını doğrudan çağırabiliyoruz.

  Bu özellik henüz *preview* aşamasında. Hala revize edilmekte ve ileride değişikliğe uğrayabilir. Şu anda bu özelliği kullanan kodu çalıştırmanın yolu derleyiciye *preview* özelliklerini kullanmasını söylemektir:
  ```sh
  java --enable-preview --source 17 PatternMatchingDeneme.java
  ```
 
### Teknik Değişiklikler
- *Floating point* sayıların işlenmesinde katı kuralların yeniden varsayılan hale getirilmesi. Bilimsel hesaplamalar yapan uygulamalar için tutarlılığı sağlamak adına yapılan bir düzenleme.
- Rassal sayı üreteçlerinde(*PRNGs*) iyileştirmeler
- Java Sanal Makinesi'nin *MacOS/AArch64* mimarisine uyarlanması.
- Bakım maliyetleri kullanılan alanlardaki faydalarına denk düşmediği için Java'nın *Ahead-of-Time* ve *Just-in-Time* derleyicilerinin desteği sonlandırıldı.


### Yeni *Incubator* Modülleri
Standart kütüphaneye eklenme sürecinde olan modüllerdir. 

- Vector API:
  İşlemcilerin vektör tabanlı komutlarını tutarlı bir şekilde kullanabilmeye yararan araçları barındıran modüldür. Birden fazla veride aynı işlem yapılan hesaplamaların optimizasyonu için kullanılan *"single instruction, multiple data"* türünde paralel işleme imkanı vermektedir. <!-- Detaylı bilgi: https://openjdk.java.net/jeps/338 -->
- Foreign Function & Memory API:
  Platforma bağımlı prosedürleri kullanaya ve verileri işlemeye yarayan Java API'si. Bu iş için mevcut alternatif olan <abbr title="Java Native Interface">JNI</abbr>'nin yerine geçmesi planlanıyor. Daha güvenli ve kullanımı kolay bir sistem olarak geliştiriliyor. <!-- Detaylı bilgi: https://openjdk.java.net/jeps/412 --> 
  


 <!--
 Bora Özdoğan
 Mayıs 2022
 -->