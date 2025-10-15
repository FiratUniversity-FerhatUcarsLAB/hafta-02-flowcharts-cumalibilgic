BAŞLA KullanıcıGirişi
    EGER kullanıcı_giriş_yaptı ISE
        kullanıcı_sepeti = yeni_sepet()
    DEĞİLSE
        yönlendir("Giriş veya Kayıt Sayfası")
BITIR KullanıcıGirişi

BAŞLA ÜrünEkleme
    ürün = seçilen_ürün()
    EGER ürün.stok > 0 ISE
        kullanıcı_sepeti.ekle(ürün)
    DEĞİLSE
        göster("Ürün stokta yok")
BITIR ÜrünEkleme

BAŞLA SepetYönetimi
    DONGU ürün IN kullanıcı_sepeti
        göster(ürün.ad, ürün.adet, ürün.fiyat)
        EGER kullanıcı_adet_değiştirirse ISE
            ürün.adet = yeni_adet
        EGER kullanıcı_ürün_silerse ISE
            kullanıcı_sepeti.sil(ürün)
    BITIR DONGU
    toplam_tutar = kullanıcı_sepeti.hesapla_toplam()
BITIR SepetYönetimi

BAŞLA İndirimKoduUygulama
    EGER kullanıcı_indirim_kodu_girerse ISE
        EGER kod.geçerli_mi() ISE
            indirim = kod.hesapla_indirim(toplam_tutar)
            toplam_tutar = toplam_tutar - indirim
        DEĞİLSE
            göster("Kod geçersiz")
BITIR İndirimKoduUygulama

BAŞLA KargoHesaplama
    adres = kullanıcı_adresi()
    kargo_seçenekleri = hesapla_kargo(adres, kullanıcı_sepeti)
    göster(kargo_seçenekleri)
    seçilen_kargo = kullanıcı_seçimi()
    toplam_tutar += seçilen_kargo.ucret
BITIR KargoHesaplama

BAŞLA ÖdemeAşaması
    göster("Toplam Tutar: " + toplam_tutar)
    ödeme_yöntemi = kullanıcı_seçimi()
    EGER ödeme_yöntemi == "Kredi Kartı" ISE
        kart_bilgileri = kullanıcı_girişi()
        EGER kart_bilgileri.geçerli_mi() ISE
            işlem = ödeme_sistemi.kartla_öde(kart_bilgileri, toplam_tutar)
        DEĞİLSE
            göster("Kart bilgileri geçersiz")
    EGER işlem.basarili_mi() ISE
        sipariş_oluştur(kullanıcı_sepeti, adres, seçilen_kargo)
        gönder_onay_maili()
    DEĞİLSE
        göster("Ödeme başarısız")
BITIR ÖdemeAşaması

BAŞLA SiparişTakibi
    sipariş_durumu = "Hazırlanıyor"
    göster("Siparişiniz alındı. Durum: " + sipariş_durumu)
    kargo_takip = oluştur_kargo_takip()
    göster("Kargo Takip No: " + kargo_takip)
BITIR SiparişTakibi
