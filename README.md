# Türkiye E-Ticaret Sanal POS ve Ödeme Sağlayıcıları Rehberi

> Bir e-ticaret sitesi kurarken en kritik kararlardan biri **ödeme altyapısı** seçimidir. Bu rehber; Türkiye'deki sanal POS modellerini, banka ve ödeme kuruluşlarını, komisyon yapısını, 3D Secure ve taksit mantığını ve entegrasyon değerlendirme ölçütlerini açıklar.
>
> Eğitici ve sektörel içeriktir — kod veya sisteme özel teknik detay içermez. 2005'ten bu yana yürütülen e-ticaret projelerindeki saha deneyimine dayanır ([Alesta WEB](https://alestaweb.com)).

İçindekiler:

1. [Sanal POS nedir?](#1-sanal-pos-nedir)
2. [Banka sanal POS vs ödeme kuruluşu (aggregator)](#2-banka-sanal-pos-vs-ödeme-kuruluşu-aggregator)
3. [Banka sanal POS sağlayıcıları](#3-banka-sanal-pos-sağlayıcıları)
4. [Ödeme kuruluşları (PF/EPK)](#4-ödeme-kuruluşları-pfepk)
5. [Komisyon ve maliyet yapısı](#5-komisyon-ve-maliyet-yapısı)
6. [Taksit ve vade farkı](#6-taksit-ve-vade-farkı)
7. [3D Secure ve güvenlik](#7-3d-secure-ve-güvenlik)
8. [Entegrasyon modelleri](#8-entegrasyon-modelleri)
9. [BKM Express, dijital cüzdan ve alternatif ödemeler](#9-bkm-express-dijital-cüzdan-ve-alternatif-ödemeler)
10. [Yasal çerçeve: BDDK, TCMB, PCI DSS](#10-yasal-çerçeve-bddk-tcmb-pci-dss)
11. [Mutabakat, iade ve ters ibraz (chargeback)](#11-mutabakat-iade-ve-ters-ibraz-chargeback)
12. [Sağlayıcı seçim kontrol listesi](#12-sağlayıcı-seçim-kontrol-listesi)
13. [Sık yapılan hatalar](#13-sık-yapılan-hatalar)
14. [Kaynaklar](#14-kaynaklar)

---

## 1. Sanal POS nedir?

**Sanal POS** (Virtual POS / VPOS), fiziksel bir kart okuyucu olmadan internet üzerinden kredi/banka kartı tahsilatı yapmayı sağlayan bir bankacılık hizmetidir. Fiziksel POS'un e-ticaret karşılığıdır: müşterinin kart bilgisi, banka veya ödeme kuruluşunun güvenli ortamında işlenir, mağaza yalnızca işlemin sonucunu alır.

Bir e-ticaret sitesinin ödeme alabilmesi için iki şeyden en az birine ihtiyacı vardır:

- Bir **banka ile sanal POS sözleşmesi**, veya
- Bir **ödeme kuruluşu** (aggregator) üyeliği.

Hangisinin doğru olduğu; ciroya, sektöre, taksit ihtiyacına ve teknik kapasiteye göre değişir.

## 2. Banka sanal POS vs ödeme kuruluşu (aggregator)

İki temel model vardır ve farkları doğrudan maliyeti ve kullanıcı deneyimini etkiler:

| Ölçüt | Banka sanal POS | Ödeme kuruluşu (aggregator) |
|---|---|---|
| **Sözleşme** | Her banka ile ayrı | Tek üyelik, çok banka |
| **Kurulum süresi** | Daha uzun (banka onayı) | Daha hızlı |
| **Komisyon** | Genelde daha düşük | Aracılık nedeniyle bir miktar üstte |
| **Taksit yönetimi** | Banka kampanyalarına doğrudan erişim | Toplu yönetim, kolaylık |
| **Para akışı** | Doğrudan banka hesabına | Kuruluş havuzu → mağaza (gün/hafta vadeli) |
| **Teknik yük** | Her banka için ayrı entegrasyon | Tek API |
| **Kimler için** | Yüksek ciro, çok bankalı taksit isteyen | Hız ve sadelik isteyen, başlangıç |

Pratikte büyük mağazalar **birden çok banka sanal POS'unu paralel** kullanır (kart hangi bankadansa o bankanın POS'una yönlendirilir — "akıllı yönlendirme"), küçük/orta ölçek ise bir veya iki ödeme kuruluşuyla başlar.

## 3. Banka sanal POS sağlayıcıları

Türkiye'de sanal POS sunan başlıca bankalar ve kullandıkları altyapılar:

| Banka | Yaygın VPOS altyapısı |
|---|---|
| Garanti BBVA | GarantiPay / kendi VPOS |
| Türkiye İş Bankası | İşbank VPOS |
| Akbank | Akbank VPOS |
| Yapı Kredi | PosNet |
| QNB Finansbank | Payfor / EST |
| Ziraat Bankası | Ziraat VPOS |
| Halkbank | NestPay/EST tabanlı |
| VakıfBank | VakıfBank VPOS |
| Denizbank | InterVPOS |
| TEB | NestPay/EST tabanlı |
| ING | NestPay/EST tabanlı |
| Albaraka Türk | katılım VPOS |
| Kuveyt Türk | katılım VPOS |

> Not: Birçok banka, tarihsel olarak **NestPay/EST** ailesinden türeyen benzer protokoller kullanır; bu, çok bankalı entegrasyonu kısmen kolaylaştırır. Yine de her bankanın 3D akışı, hata kodları ve taksit alanları farklılık gösterir.

## 4. Ödeme kuruluşları (PF/EPK)

BDDK/TCMB lisanslı ödeme ve elektronik para kuruluşları, tek entegrasyonla çok bankaya erişim sağlar:

| Kuruluş | Öne çıkan özellik |
|---|---|
| **iyzico** | Yaygın API, kolay başlangıç, pazaryeri/marketplace çözümü |
| **PayTR** | Hızlı onay, iFrame ödeme formu, küçük/orta ölçek dostu |
| **Param** | TROY ve dijital cüzdan, kurumsal çözümler |
| **Sipay** | Çok ortaklı (split) ödeme, marketplace |
| **Moka (United Payment)** | Tekrarlayan (subscription) ödeme |
| **Paycell** | Turkcell ekosistemi, mobil ödeme |
| **Paynet** | B2B ve bayi tahsilatı |
| **Birleşik Ödeme** | Kurumsal tahsilat |

Ödeme kuruluşları özellikle **tekrarlayan ödeme (abonelik)**, **pazaryeri split ödeme** ve **hızlı entegrasyon** gereken senaryolarda öne çıkar.

## 5. Komisyon ve maliyet yapısı

Sanal POS maliyeti tek bir "komisyon oranı"ndan ibaret değildir. Değerlendirilmesi gereken kalemler:

- **İşlem komisyonu (%):** Tek çekimde genelde en düşük oran.
- **Taksit komisyonu:** Taksit sayısı arttıkça oran artar (vade farkı). 2/3 taksit ile 9/12 taksit arasında ciddi fark olur.
- **Vade / valör:** Paranın hesaba geçme süresi. "Bloke gün" sayısı nakit akışını etkiler.
- **Aylık/yıllık sabit ücret:** Bazı bankalarda POS bakım/üyelik ücreti.
- **Ek hizmet ücretleri:** İade, ters ibraz, ekstra rapor.

> Altın kural: İki sağlayıcıyı yalnızca "tek çekim komisyonu" ile karşılaştırmak yanıltıcıdır. Gerçek maliyet, **sizin gerçek taksit dağılımınıza** göre hesaplanır. Çok taksit satıyorsanız taksit tablosu, tek çekim oranından çok daha önemlidir.

## 6. Taksit ve vade farkı

Türkiye e-ticaretinde taksit, satışı doğrudan etkileyen bir faktördür. Üç temel yaklaşım vardır:

1. **Vade farkını müşteri öder:** Taksit komisyonu fiyata yansır; mağaza net tutarı alır.
2. **Vade farkını mağaza öder:** "Taksit farksız" kampanyası; rekabet avantajı ama maliyet mağazada.
3. **Banka kampanyaları:** Bankaların dönemsel "ekstra taksit / erteleme" kampanyaları (Bonus, World, Maximum, Axess, Paraf, Bankkart vb.) doğrudan banka POS ile kullanılabilir.

İyi bir e-ticaret altyapısı, **kart BIN'ine göre** doğru taksit seçeneklerini otomatik gösterebilmelidir (örneğin yalnızca o kartın bankasının izin verdiği taksitleri sunmak).

## 7. 3D Secure ve güvenlik

**3D Secure (3DS)**, kart sahibinin işlemi bankası üzerinden (SMS OTP veya mobil onay) doğrulamasıdır. Modeller:

- **3D Secure (zorunlu):** İşlem ancak doğrulama başarılıysa tamamlanır. Ters ibraz riskini büyük ölçüde mağazadan bankaya taşır. **Önerilen varsayılan budur.**
- **3D Pay / Hibrit:** Doğrulama denenir, başarısızsa bazı senaryolarda non-secure devam edebilir (riskli; genelde önerilmez).
- **Non-Secure:** Doğrulama yok. Ters ibraz riski tümüyle mağazada. Yalnızca özel durumlarda.

Güvenlik açısından kritik ilke: **kart verisi asla mağaza sunucusunda saklanmaz veya işlenmez.** Ödeme, sağlayıcının PCI DSS uyumlu ortamında gerçekleşir; mağaza yalnızca sonucu (başarılı/başarısız) alır.

## 8. Entegrasyon modelleri

| Model | Açıklama | Avantaj | Dikkat |
|---|---|---|---|
| **Yönlendirme (redirect)** | Müşteri ödeme için sağlayıcı sayfasına gider | En düşük PCI yükü | Marka deneyimi kesilir |
| **iFrame / popup** | Ödeme formu mağaza sayfasına gömülü gelir | İyi denge: güvenli + bütünleşik | iFrame uyumu test edilmeli |
| **API (kart alanı mağazada)** | Kart bilgisi mağaza formunda toplanır | Tam marka kontrolü | **Yüksek PCI DSS yükümlülüğü** — çoğu mağaza için önerilmez |

Çoğu e-ticaret sitesi için **iFrame veya redirect** doğru tercihtir: kart verisi sağlayıcıda kalır, mağazanın PCI yükü minimumda tutulur.

**Callback (bildirim) mantığı kritik:** Sipariş, müşterinin tarayıcısı geri döndüğü için değil, **sağlayıcıdan gelen sunucu-sunucu başarı bildirimi (callback/webhook)** doğrulandığı için "ödendi" sayılmalıdır. Doğru tasarım: sipariş yalnızca başarılı callback ile kesinleşir; bildirim **idempotent** işlenir (aynı bildirim iki kez gelirse çift sipariş oluşmaz).

## 9. BKM Express, dijital cüzdan ve alternatif ödemeler

Kart dışı ve hızlandırılmış ödeme seçenekleri dönüşümü artırır:

- **Kapıda ödeme:** Nakit/kart; kargo entegrasyonuyla.
- **Havale / EFT:** Komisyonsuz ama manuel mutabakat gerektirir.
- **Dijital cüzdanlar:** Papara, Paycell, Param cüzdanı vb.
- **Tek tık / kart saklama:** Sağlayıcının token altyapısıyla (kart numarası saklanmadan).
- **BNPL (sonra öde):** Yükselen bir model; sağlayıcı desteğine bağlı.

Birden çok ödeme yöntemi sunmak, sepet terk oranını düşüren en pratik yöntemlerden biridir.

## 10. Yasal çerçeve: BDDK, TCMB, PCI DSS

- **TCMB / BDDK:** Ödeme ve elektronik para kuruluşları lisanslıdır. Sözleşme yaptığınız aggregator'ın lisanslı olduğunu doğrulayın.
- **PCI DSS:** Kart verisi güvenliği standardı. Redirect/iFrame modelinde yük sağlayıcıdadır; kart verisini siz toplarsanız yük size geçer.
- **Mesafeli Satış Sözleşmesi & KVKK:** Ödeme sayfasında bilgilendirme, fatura ve kişisel veri yükümlülükleri.
- **e-Fatura / e-Arşiv:** Tahsilat sonrası belge süreci (GİB entegrasyonu).

## 11. Mutabakat, iade ve ters ibraz (chargeback)

Ödeme "alındıktan" sonra başlayan, çoğu zaman ihmal edilen süreçler:

- **Mutabakat (reconciliation):** Sağlayıcı raporu ile mağaza siparişlerinin günlük eşleştirilmesi. Eksik/çift kayıt buradan yakalanır.
- **İade (refund):** Kısmi/tam iade; orijinal işleme bağlı yapılmalı. Vade ve komisyon iadesi sağlayıcıya göre değişir.
- **Ters ibraz (chargeback):** Kart sahibinin itirazı. 3D Secure işlemlerde risk büyük ölçüde mağazadan çıkar; non-secure'da mağazadadır.

İyi bir e-ticaret altyapısı bu üç süreci raporlarıyla birlikte yönetebilmelidir.

## 12. Sağlayıcı seçim kontrol listesi

Bir ödeme sağlayıcısı seçmeden önce:

- [ ] Banka POS mu, ödeme kuruluşu mu, ikisi birden mi gerekiyor?
- [ ] Tek çekim **ve** gerçek taksit dağılımınıza göre toplam maliyet hesaplandı mı?
- [ ] Hangi taksit sayılarına (2–12) ihtiyaç var? Banka kampanyaları gerekiyor mu?
- [ ] 3D Secure varsayılan mı? Non-secure kapatılabiliyor mu?
- [ ] Entegrasyon modeli (redirect / iFrame / API) PCI yüküne uygun mu?
- [ ] Callback/webhook **sunucu taraflı** ve **idempotent** doğrulanıyor mu?
- [ ] Para hesaba ne kadar sürede geçiyor (valör/bloke)?
- [ ] İade, kısmi iade, ters ibraz akışı destekleniyor mu?
- [ ] Mutabakat raporu otomatik alınabiliyor mu?
- [ ] Tekrarlayan ödeme (abonelik) gerekiyor mu? Token saklama var mı?
- [ ] Pazaryeri/split ödeme gerekiyor mu?
- [ ] Sağlayıcı BDDK/TCMB lisanslı mı?
- [ ] Teknik dokümantasyon ve test ortamı yeterli mi?

## 13. Sık yapılan hatalar

- **Tek çekim komisyonuna göre seçmek:** Taksitli satıyorsanız gerçek maliyet farklıdır.
- **Siparişi tarayıcı dönüşünde oluşturmak:** Ödeme başarısız olsa bile sipariş düşebilir. Doğrusu sunucu callback'i ile kesinleştirmektir.
- **Callback'i idempotent yapmamak:** Çift bildirim → çift sipariş/çift tahsilat kaydı.
- **Tek sağlayıcıya bağımlı kalmak:** Sağlayıcı kesintisinde tüm satış durur. Yedek POS önerilir.
- **3D Secure'u kapatıp ters ibraz riskini almak.**
- **Mutabakatı atlamak:** Eksik geçen tahsilatlar fark edilmez.
- **Kart verisini mağazada toplamak:** Gereksiz PCI DSS yükü ve güvenlik riski.

## 14. Kaynaklar

- [BDDK — Bankacılık Düzenleme ve Denetleme Kurumu](https://www.bddk.org.tr)
- [TCMB — Ödeme Kuruluşları](https://www.tcmb.gov.tr)
- [BKM — Bankalararası Kart Merkezi](https://bkm.com.tr)
- [PCI Security Standards Council](https://www.pcisecuritystandards.org)
- [GİB — e-Fatura / e-Arşiv](https://www.gib.gov.tr)
- [TROY — Yerli Kart Şeması](https://www.troyodeme.com)

---

*Bu rehber eğitici amaçlıdır ve sektörel genel bilgiye dayanır; finansal veya hukuki tavsiye değildir. Sağlayıcı oran ve koşulları zamanla değişir — güncel bilgiyi sağlayıcıdan doğrulayın. E-ticaret saha deneyimi: [Alesta WEB](https://alestaweb.com). Katkı ve düzeltmeler için "Issues" bölümü açıktır.*
