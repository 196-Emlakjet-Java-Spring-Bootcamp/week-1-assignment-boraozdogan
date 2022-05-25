# Java 8'den Sonra Gelen Değişiklikler

## Java 11 Değişiklikleri

### Genel Değişiklikler
- Java'nın resmi yazılım geliştirme kiti (Oracle JDK) artık lisanslanmadan ticari amaçla kullanılamıyor. Onun yerine OpenJDK kullanılabilir.


### Dil Özelliklerindeki Değişiklikler
- *Lambda* ifadelerinde yerel değişken tanımlaması benzeri tanımlamalara destek verildi. Bu sayede *lambda* parametlerelerinde *annotation* yapıları kullanılabilir oldu:  
  - . . `(@Deprecated var it) -> it.toUpperCase()`

### Standart Kütüphanedeki Değişiklikler
- `String` nesnelerine yeni metotlar eklendi:
    - `#.isBlank()`
    - `#.lines()`
    - `#.strip()` <!--  #.trim() ile farkını anlat -->
    - `#.stripLeading()`
    - `#.repeat(int)`
- Standart kütüphaneye HTTP istemcisi eklendi. `java.net.http` paketinde
  <!-- örnek kod iliştir -->
- Standart kütüphaneye dosya içeriğini `String` biçiminde okuma ve yazmayı sağlayan yardımcı fonksiyonlar eklendi. `java.io.Files` sınıfına ait `readString` ve `writeString` fonksiyonları.
- Koleksiyon sınıflarına `toArray()` metodu eklendi.

### Teknik Araçlardaki Değişiklikler
- Java komut satırı derleyicisi artık uygulamayı tek aşamada kaynak koddan derleyip çalıştırabiliyor:  
  - . . `java App.java`
  - (Java 11 öncesinde `javac` ile CLASS dosyası olarak derleyip onun ardından `java` ile çalıştırmak şarttı.)
- Java için bir performans profilleme yazılımı olan "Java Flight Recorder" artık açık kaynak kodlu ve OpenJDK içerisinde yer alıyor. (Önceden Oracle'ın sahipli yazılımıydı.)
- Epsilon adında, sadece hafızada yer ayırma işlemi yapıp "çöp toplama" işlemini yapmayan *"No-Op"* bir *garbage collector* eklendi. Bazı test ortamları için yararlı olabilecek deneysel bir özellik.

### Diğer Değişiklikler
- TLS sürümü 1.3'e güncellendi. <!-- detay ver -->
- Düşük gecikmeli bir başka *garbage collector* eklendi. İsmi "ZGC".
- Unicode 10 standardı desteklenmeye başlandı. {&#x20BF;}



## Java 17 Değişiklikleri

### Dildeki değişiklikler
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
  Burada *type cast* işlemi koşul kontrolünde gerçekleştirildiğinden dolayı nesnenin metotlarını doğrudan çağırabiliyoruz. Ayrıca `switch` ifadesi artık bir *expression* olduğundan dolayı doğrudan `return` ile kullanılabiliyor.

  Bu özellik henüz *preview* aşamasında. Hala revize edilmekte ve ileride değişikliğe uğrayabilir. Şu anda bu özelliği kullanan kodu çalıştırmanın yolu derleyiciye *preview* özelliklerini kullanmasını söylemektir:
  ```sh
  java --enable-preview --source 17 PatternMatchingDeneme.java
  ```


- "Sealed class" kavramı eklendi. Eğer bir sınıftan türetilebilecek alt sınıfların belirlemeye ihtiyaç varsa bunu artık sınıf tanımında `sealed` ve `permits` anahtar kelimelerini kullanarak yapabiliyoruz:  
  ```java
  public abstract sealed class Sekil permits Daire, Kare, Dikdortgen {
      ...
  }
  ```

  `Sekil` sınıfını sadece `Daire`, `Kare` ve `Dikdortgen` sınıfları genişletebilir (`extends` anahtar kelimesi).

  > **Not:** Alt sınıflar tanımlanırken `final`, `sealed` ya da `non-sealed` ifadelerinden birinin kullanılması gerekiyor.
 
### Teknik değişiklikler
- *Floating point* sayıların işlenmesinde katı kuralların yeniden varsayılan hale getirilmesi. Bilimsel hesaplamalar yapan uygulamalar için tutarlılığı sağlamak adına yapılan bir düzenleme.
- Rassal sayı üreteçlerinde(*PRNGs*) iyileştirmeler
- Java Sanal Makinesi'nin *MacOS/AArch64* mimarisine uyarlanması. *Mac* sistemlerinde *low-level* işlemlerde stabilite ve performans artışı sağlayacaktır. <!-- Mac sahibi olmadığım için kesin yorum yapamıyorum buna. -->
- Bakım maliyetleri kullanılan alanlardaki faydalarına denk düşmediği için Java'nın *Ahead-of-Time* ve *Just-in-Time* derleyicilerinin desteği sonlandırıldı.

 <!--
 Bora Özdoğan
 Mayıs 2022
 -->