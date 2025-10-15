Başla

// 1. Kimlik Doğrulama
Kullanıcıdan TC Kimlik No ve Şifre al
Eğer kimlik doğrulama başarılıysa
    Devam et
Aksi halde
    "Kimlik doğrulama başarısız" mesajı göster
    Bitir

// 2. Hizmet Seçimi
Kullanıcıya iki seçenek sun:
    1. Randevu Sistemi
    2. Tahlil Sonucu Görüntüleme
Kullanıcıdan bir hizmet seçmesini iste

Eğer kullanıcı "Randevu Sistemi" seçtiyse:
    // Randevu Süreci
    Tüm poliklinikleri listele
    Kullanıcıdan bir poliklinik seçmesini iste

    Seçilen polikliniğe ait doktorları listele
    Kullanıcıdan bir doktor seçmesini iste

    Seçilen doktorun uygun randevu saatlerini listele
    Kullanıcıdan bir saat seçmesini iste

    Seçilen doktor, tarih ve saat bilgilerini kullanıcıya göster
    Kullanıcıdan onay al

    Eğer onaylandıysa
        Randevuyu veritabanına kaydet
        SMS gönder
        "Randevunuz başarıyla oluşturuldu" mesajı göster
    Aksi halde
        "Randevu iptal edildi" mesajı göster

Eğer kullanıcı "Tahlil Sonucu Görüntüleme" seçtiyse:
    // Tahlil Süreci
    Kullanıcının tahlil kaydı var mı kontrol et
    Eğer tahlil kaydı varsa
        Tahlil sonucu hazır mı kontrol et
        Eğer sonuç hazırsa
            Tahlil sonuçlarını ekranda göster
            Kullanıcıya "PDF olarak indir" seçeneği sun
            Eğer kullanıcı indir seçeneğini seçerse
                PDF dosyasını oluştur ve indir
                "PDF indirildi" mesajı göster
            Aksi halde
                "İndirme iptal edildi" mesajı göster
        Aksi halde
            "Sonuçlar henüz hazır değil, lütfen daha sonra tekrar deneyin" mesajı göster
    Aksi halde
        "Tahlil kaydı bulunamadı" mesajı göster

Bitir
