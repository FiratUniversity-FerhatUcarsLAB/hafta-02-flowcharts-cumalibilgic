BAŞLA
  // 1. Öğrenci Girişi
  GİRİŞ YAP (öğrenciNo, şifre)
  EĞER girişBaşarısız İSE
    HATA GÖSTER("Geçersiz öğrenci no veya şifre.")
    ÇIKIŞ
  SON-EĞER

  // 2. Ders Seçim Döngüsü
  seçilenDerslerListesi = []
  toplamKredi = 0
  DEVAM_ET = EVET

  DÖNGÜ (DEVAM_ET == EVET)
    DERS LİSTESİNİ GÖSTER()
    seçilenDers = KULLANICIDAN DERS AL()

    // 3. Kontrol Mekanizmaları
    kontenjanUygun = KONTENJAN_KONTROLÜ(seçilenDers)
    önKoşulUygun = ÖNKOŞUL_KONTROLÜ(öğrenciNo, seçilenDers)
    zamanÇakışmasıYok = ZAMAN_ÇAKIŞMASI_KONTROLÜ(seçilenDers, seçilenDerslerListesi)
    krediLimitiAşılmadı = KREDİ_LİMİTİ_KONTROLÜ(toplamKredi, seçilenDers.kredi, 35)

    EĞER (kontenjanUygun VE önKoşulUygun VE zamanÇakışmasıYok VE krediLimitiAşılmadı) İSE
      seçilenDerslerListesi.EKLE(seçilenDers)
      toplamKredi = toplamKredi + seçilenDers.kredi
      BİLGİ VER("Ders başarıyla eklendi.")
    DEĞİLSE
      EĞER !kontenjanUygun İSE HATA GÖSTER("Ders kontenjanı dolu.")
      EĞER !önKoşulUygun İSE HATA GÖSTER("Ön koşul dersi sağlanmıyor.")
      EĞER !zamanÇakışmasıYok İSE HATA GÖSTER("Ders saatleri çakışıyor.")
      EĞER !krediLimitiAşılmadı İSE HATA GÖSTER("Maksimum kredi limitini aşıyorsunuz.")
    SON-EĞER

    kullanıcıCevabı = SOR("Başka ders eklemek istiyor musunuz? (E/H)")
    EĞER kullanıcıCevabı == 'H' İSE
      DEVAM_ET = HAYIR
    SON-EĞER
  SON-DÖNGÜ

  // 4. Danışman Onayı ve Kayıt Onaylama
  danışmanOnayıGerekli = HAYIR
  öğrenciGPA = GPA_GETİR(öğrenciNo)
  EĞER öğrenciGPA < 2.5 İSE
    danışmanOnayıGerekli = EVET
  SON-EĞER

  KAYIT ÖZETİNİ GÖSTER(seçilenDerslerListesi, toplamKredi, danışmanOnayıGerekli)
  onay = KULLANICIDAN ONAY AL("Kaydı tamamlamak istiyor musunuz? (E/H)")

  EĞER onay == 'E' İSE
    EĞER danışmanOnayıGerekli İSE
      KAYDI ONAYA GÖNDER()
      BİLGİ VER("Kayıt işleminiz danışman onayına gönderilmiştir.")
    DEĞİLSE
      KAYDI TAMAMLA()
      BİLGİ VER("Ders kaydınız başarıyla tamamlanmıştır.")
    SON-EĞER
  DEĞİLSE
    KAYDI İPTAL ET()
    BİLGİ VER("Kayıt işlemi iptal edildi.")
  SON-EĞER
SON
