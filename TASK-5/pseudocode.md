BAŞLA

  // Sistem değişkenlerini başlangıç durumuna getir
  ALARM_DURUMU = "PASİF"

  // Ana döngü sistem çalıştığı sürece sürekli devam eder
  DÖNGÜ (DOĞRU)

    // 1. ADIM: Sensör Verilerini Oku
    // Her döngüde tüm sensörler kontrol edilir
    hareket_algilandi = hareketSensörünüOku()
    kapi_pencere_acik = kapiPencereSensörünüOku()
    kamera_goruntusu = kamerayiOku()
    alarm_sifirlama_istegi = alarmSifirlamaKomutunuKontrolEt()

    // Alarm sıfırlama komutu geldiyse alarmı kapat ve döngüye baştan başla
    EĞER (alarm_sifirlama_istegi == EVET VE ALARM_DURUMU == "AKTİF")
      alarmıSustur()
      ALARM_DURUMU = "PASİF"
      kullaniciyaBildirimGonder("Sistem alarmı kullanıcı tarafından sıfırlandı.")
      DEVAM ET // Döngünün bu adımını sonlandır ve başa dön
    SON_EĞER

    // Alarm aktif değilse tehdit kontrolü yap
    EĞER (ALARM_DURUMU == "PASİF")
    
      // 2. ADIM: Tehdit Algılama ve Seviyelendirme
      tehdit_seviyesi = "YOK"

      EĞER (kapi_pencere_acik == EVET)
        tehdit_seviyesi = "YÜKSEK"
      YOKSA EĞER (hareket_algilandi == EVET)
        tehdit_seviyesi = "ORTA"
      YOKSA EĞER (kamera_goruntusu icinde supheli_nesne_var)
        tehdit_seviyesi = "DÜŞÜK"
      SON_EĞER


      // 3. ADIM: Tehdit Seviyesine Göre Aksiyon Al
      EĞER (tehdit_seviyesi == "YÜKSEK")
        // Alarm çal ve en üst seviye bildirim gönder
        ALARM_DURUMU = "AKTİF"
        yüksekSesliAlarmCal()
        kullaniciyaBildirimGonder("ACİL: Kapı/Pencere izinsiz açıldı!")
        poliseHaberVer() // Opsiyonel

      YOKSA EĞER (tehdit_seviyesi == "ORTA")
        // Alarm çal ve uyarı bildirimi gönder
        ALARM_DURUMU = "AKTİF"
        ortaSesliAlarmCal()
        kullaniciyaBildirimGonder("UYARI: Ev içinde hareket algılandı!")

      YOKSA EĞER (tehdit_seviyesi == "DÜŞÜK")
        // Sadece kullanıcıyı bilgilendir, alarm çalma
        kullaniciyaBildirimGonder("BİLGİ: Kamera şüpheli bir nesne gördü.")
        olayinVideosunuKaydet()
        
      SON_EĞER

    SON_EĞER
    
    // Sistem kaynaklarını verimli kullanmak için döngüler arasında kısa bir bekleme ekle
    bekle(1 saniye)

  SON_DÖNGÜ

SON
