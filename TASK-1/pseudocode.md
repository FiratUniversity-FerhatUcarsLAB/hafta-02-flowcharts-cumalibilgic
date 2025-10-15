BAŞLA

    // Sabitler ve başlangıç değerleri
    GÜNLÜK_LIMIT ← 2000
    PIN ← 1234
    bakiye ← 5000
    kalan_limit ← GÜNLÜK_LIMIT
    pin_hak ← 3
    işlem_tekrar ← "E"

    DÖNGÜA işlem_tekrar = "E" VEYA işlem_tekrar = "e" İSE

        // PIN doğrulama
        DÖNGÜA pin_hak > 0 İSE
            YAZ "Lütfen PIN kodunuzu giriniz:"
            OKU girilen_pin

            EGER girilen_pin = PIN İSE
                ÇIK PIN doğrulama döngüsünden
            DEĞİLSE
                pin_hak ← pin_hak - 1
                YAZ "Hatalı PIN. Kalan deneme hakkı: ", pin_hak
            SON
        SON

        EGER pin_hak = 0 İSE
            YAZ "3 kez hatalı PIN girildi. Kart bloke edildi."
            ÇIK ANA döngüden
        SON

        // Tutar girme ve kontroller
        YAZ "Çekmek istediğiniz tutarı giriniz (20 TL katı):"
        OKU çekilecek_tutar

        EGER çekilecek_tutar MOD 20 ≠ 0 İSE
            YAZ "Hatalı tutar. Lütfen 20 TL'nin katı bir tutar giriniz."
        DEĞİLSE
            EGER çekilecek_tutar > bakiye İSE
                YAZ "Yetersiz bakiye. Mevcut bakiye: ", bakiye
            DEĞİLSE
                EGER çekilecek_tutar > kalan_limit İSE
                    YAZ "Günlük limit aşıldı. Kalan günlük limit: ", kalan_limit
                DEĞİLSE
                    bakiye ← bakiye - çekilecek_tutar
                    kalan_limit ← kalan_limit - çekilecek_tutar
                    YAZ "İşlem başarılı. Yeni bakiye: ", bakiye
                    YAZ "Kalan günlük limit: ", kalan_limit
                SON
            SON
        SON

        // İşlem tekrarı
        YAZ "Başka bir işlem yapmak ister misiniz? (E/H)"
        OKU işlem_tekrar

    SON

YAZ "İyi günler! ATM'den çıkılıyor..."

BİTİR
