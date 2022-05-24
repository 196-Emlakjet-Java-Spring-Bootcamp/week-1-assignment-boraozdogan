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
- `switch` ifadeleri için *"pattern matching"* desteği getirildi. <!-- örneksiz anlatmak zor bunu -->

- "Sealed class" kavramı eklendi. Artık bir sınıftan türetilebilecek alt sınıfların hangileri olabileceği belirlenebiriyor.
 

 <!--
 Bora Özdoğan
 Mayıs 2022
 -->