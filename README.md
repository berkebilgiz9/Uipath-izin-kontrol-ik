# Uipath-izin-kontrol-ik
Personel ve mağaza müdürü shift raporlarındaki izin girişlerini karşılaştırarak tutarsızlıkları otomatik tespit eden ve ilgili mağazaya bildirim gönderen bir RPA çözümü

##REFrameWork Şablonu
Robotic Enterprise Framework (Kurumsal Robotik Çerçeve)

Transactional Business Process şablonu üzerine inşa edilmiştir
Otomasyon projesinin aşamaları için State Machine (Durum Makinesi) yapısını kullanır
Üst düzey loglama, istisna (exception) yönetimi ve kurtarma (recovery) sunar
Harici ayarları Config.xlsx dosyasında ve Orchestrator asset'lerinde tutar
Kimlik bilgilerini Orchestrator asset'lerinden ve Windows Credential Manager'dan alır
İşlem (transaction) verilerini Orchestrator kuyruğundan (queue) alır ve durumunu günceller
Sistem istisnaları (system exceptions) durumunda ekran görüntüsü alır

Nasıl Çalışır

SÜRECİ BAŞLAT (INITIALIZE PROCESS)


./Framework/InitiAllSettings - Config.xlsx dosyasından ve asset'lerden yapılandırma verilerini yükler
./Framework/GetAppCredential - Kimlik bilgilerini Orchestrator asset'lerinden veya yerel Windows Credential Manager'dan alır
./Framework/InitiAllApplications - Süreç boyunca kullanılan uygulamaları açar ve bunlara giriş yapar


İŞLEM VERİSİNİ AL (GET TRANSACTION DATA)


./Framework/GetTransactionData - Config("OrchestratorQueueName") ile tanımlanan Orchestrator kuyruğundan veya yapılandırılmış başka bir veri kaynağından işlemleri (transaction) çeker


İŞLEMİ İŞLE (PROCESS TRANSACTION)


Process - İşlemi işler ve otomasyonu yapılan sürece bağlı diğer workflow'ları çağırır (invoke eder)
./Framework/SetTransactionStatus - İşlenen işlemin durumunu günceller (varsayılan olarak Orchestrator işlemleri): Başarılı (Success), İş Kuralı İstisnası (Business Rule Exception) veya Sistem İstisnası (System Exception)


SÜRECİ SONLANDIR (END PROCESS)


./Framework/CloseAllApplications - Süreç boyunca kullanılan uygulamalardan çıkış yapar ve bunları kapatır

Yeni Proje İçin

Config.xlsx dosyasını kontrol edin ve gerekli alanları/değerleri ekleyin ya da özelleştirin
InitiAllApplications.xaml ve CloseAllApplications.xaml workflow'larını uygulayın ve bunları Config.xlsx alanlarına bağlayın
GetTransactionData.xaml ve SetTransactionStatus.xaml dosyalarını kullanılan işlem (transaction) türüne göre uygulayın (varsayılan olarak Orchestrator kuyrukları)
Process.xaml workflow'unu uygulayın ve otomasyonu yapılan sürece bağlı diğer workflow'ları çağırın (invoke edin)
