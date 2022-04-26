# Code Review Checklist

- İlgili feature geliştirmesi yapılmış mı? yüzeysel kontrol

- Değişken ve metod isimlendirmeleri kontrolü

- Magic number kullanımı var mı? ortalıkta dolaşan - ne olduğu belli olmayan sayılar

- Code style kontrolü - (reformat edilmiş mi)

- Kullanılmayan değişken, metod veya sınıf var mı?

- Release'de gözüekn Log kalmış mı?

- Todo kalmış mı?

- Yorum içine alınmış kod kalmış mı?

- Memory leak oluşturabilecek durumlar var mı? (context’in sağa sola paslanması gibi)

> Örnek: Servis isteği ekran sonlandığında bitirilmiş olmalı aksi takdirde callback’ten ui düzenlemeleri yapılıyorsa crash’e sebep olacaktır.

- Code duplication var mı?

- Projedeki standartları gözetilmiş mi? (stiller vs.)

- Projede olan bir componentin, kullanılmayıp yerine benzeri oluştrulmuş mu?

- Bariz bir crash durumu kontrolü

- Unit testlerin yazılmış olması kontrolü

- Android lint uyarıları da dikkate alınmalı

- Lokalizasyon kontrolü (Tüm metinler strings.xml altında tanınmlı olmalı)

- CHANGELOG’a ilgili madde eklenmiş olmalı

**< [Go back to Android section](../android)**
