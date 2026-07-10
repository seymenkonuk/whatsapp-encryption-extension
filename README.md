# Whatsapp Encryption Extension
> WhatsApp Web mesajlarını ve dosyalarını istemci tarafında şifreleyen Chromium tarayıcı eklentisi.

## Açıklama

WhatsApp Web üzerinden gönderilen mesaj ve dosyalara, platformun mevcut uçtan uca şifrelemesine ek olarak istemci tarafında ikinci bir şifreleme katmanı uygulayan tarayıcı eklentisidir.

Kullanıcılar ortak sohbet anahtarını manuel olarak tanımlayabilir veya ECDH anahtar değişimiyle oluşturabilir. `Ctrl + Enter` kullanılarak gönderilen mesajlar bu anahtarla şifrelenir; karşı tarafta aynı anahtar tanımlıysa içerik otomatik olarak çözülerek sohbet ekranında gösterilir. Dosyalar, özgün ad ve tür bilgileri korunarak şifreli metin dosyalarına dönüştürülür ve WhatsApp Web üzerinden gönderime hazırlanır. 

Sohbet anahtarları tarayıcı depolamasında şifreli biçimde saklanır. Şifreli veriler dosya olarak dışarı aktarılabilir ve sürükle-bırak yöntemiyle yeniden içeri alınabilir.

## Özellikler

- WhatsApp Web mesajlarını AES ile şifreleme ve çözme
- Dosyaları şifreli `.txt` dosyalarına dönüştürme
- Şifreli dosyaları özgün adı ve türüyle geri oluşturma
- Sohbet anahtarlarını kişi adına göre saklama
- Ortak anahtarı manuel olarak tanımlama
- ECDH ile ortak anahtar oluşturma
- Anahtar bulunmadığında bildirim gösterme
- Alıntılanan ve yanıtlanan mesajları çözme
- Anahtarları tarayıcı depolamasında şifreli olarak saklama
- Şifreli anahtar verisini dosya olarak dışarı aktarma
- Yedek dosyasını sürükle-bırak yöntemiyle içeri aktarma
- Google Chrome ve Microsoft Edge desteği

## Kurulum

Projeyi klonlayın:

```bash
git clone https://github.com/seymenkonuk/whatsapp-encryption-extension.git
cd whatsapp-encryption-extension
```

Eklentiyi Google Chrome'a yüklemek için:

1. `chrome://extensions/` adresini açın.
2. **Geliştirici modu** seçeneğini etkinleştirin.
3. **Paketlenmemiş öğe yükle** seçeneğine tıklayın.
4. Projedeki `build/` dizinini seçin.
5. WhatsApp Web'i açın veya açıksa sayfayı yenileyin.

Microsoft Edge için aynı işlem `edge://extensions/` sayfasından yapılabilir.

> 💡 **Not:** Eklenti yalnızca `https://web.whatsapp.com/` adresinde çalışır.

## Yapılandırma

Temel uygulama ayarları `build/src/config.min.js` dosyasında bulunur:

```js
export const key="test_key";
export const app_name="SYChat";
```

- `key`: Tarayıcı depolamasındaki verilerin şifrelenmesinde kullanılan anahtar
- `app_name`: Eklenti adı

Eklentinin görünen adını tamamen değiştirmek için `build/manifest.json` içindeki `name` ve `description` alanlarını da güncelleyin.

> ⚠️ **Uyarı:** Eklentiyi kullanmadan önce güvenli bir `key` değeri belirleyiniz.

> ⚠️ **Uyarı:** `key` değeri değiştirildiğinde önceki anahtarla şifrelenmiş veriler ve yedek dosyaları okunamaz.

> ⚠️ **Uyarı:** `app_name` değeri açık anahtar mesajlarının başlangıç ve bitiş işaretleyicilerinde kullanılır. İki kullanıcının farklı `app_name` değerleri kullanması otomatik anahtar değişimini engeller.

## Kullanım

### Manuel Sohbet Anahtarı Tanımlama

İki kullanıcının da aynı anahtarı kendi eklentisine kaydetmesi gerekir.

1. Eklenti simgesine tıklayın.
2. WhatsApp sohbet başlığında görünen kişi adını eksiksiz biçimde girin.
3. **Kişi Oluştur** düğmesine tıklayın.
4. Oluşturulan kaydın düzenleme düğmesine tıklayın.
5. Ortak anahtarı girin veya rastgele anahtar oluşturma düğmesini kullanın.
6. Değişikliği kaydedin.
7. Karşı tarafın eklentisinde de aynı anahtar tanımlanmalıdır.

> ⚠️ **Uyarı:** Anahtar kayıtları WhatsApp'ta görünen sohbet adına göre eşleştirilir. Kişinin görünen adı değişirse veya birden fazla sohbet aynı ada sahipse yanlış anahtar kullanılabilir.

> ⚠️ **Uyarı:** Manuel anahtarı WhatsApp sohbeti üzerinden açık biçimde göndermeyin. Anahtarı güvenilir, ayrı bir iletişim kanalıyla paylaşın.

### ECDH ile Ortak Anahtar Oluşturma

Ortak anahtar, açık anahtarların WhatsApp sohbeti üzerinden paylaşılmasıyla oluşturulabilir.

1. İlgili bire bir sohbeti açın.
2. Henüz sohbet anahtarı yokken bir mesajı `Ctrl + Enter` ile göndermeyi deneyin.
3. Anahtar bulunamadığına ilişkin bildirimden sonra açık anahtarınızı paylaşmayı onaylayın.
4. Karşı tarafın da aynı işlemi yaparak kendi açık anahtarını göndermesini bekleyin.
5. İki taraf da açık anahtarını gönderdiğinde ortak anahtar ECDH ile otomatik olarak oluşturulur ve saklanır.

> 💡 **Not:** ECDH anahtar değişimi bire bir sohbetler için tasarlanmıştır. Grup sohbetlerinde birden fazla katılımcının açık anahtarları aynı sohbet kaydı altında karışabilir.

> ⚠️ **Uyarı:** Eklenti, oluşturulan açık anahtarlar için parmak izi doğrulaması veya kimlik onayı sunmaz.

### Şifreli Mesaj Gönderme

1. İlgili sohbet için ortak anahtarın tanımlandığından emin olun.
2. Mesajı WhatsApp Web mesaj alanına yazın.
3. Mesajı `Ctrl + Enter` ile gönderin.

Eklenti mesajı şifreleyerek WhatsApp Web üzerinden gönderir. Aynı anahtara sahip karşı tarafın eklentisi şifreli içeriği otomatik olarak çözer.

> ⚠️ **Uyarı:** Normal `Enter` tuşu veya WhatsApp'ın gönder düğmesi kullanılırsa mesaj eklenti tarafından şifrelenmez.

> 💡 **Not:** Alıntılanan ve yanıtlanan şifreli mesajlar da mümkün olduğunda otomatik olarak çözülür.

### Dosya Şifreleme ve Gönderme

1. İlgili sohbet için ortak anahtarın tanımlandığından emin olun.
2. Dosyayı WhatsApp Web'in sol tarafına eklenen mavi yükleme alanına sürükleyip bırakın.
3. Eklenti dosyayı şifreler ve rastgele adlandırılmış bir `.txt` dosyası olarak WhatsApp'ın dosya gönderme alanına aktarır.
4. WhatsApp Web üzerinden gönderimi tamamlayın.

Özgün dosyanın:

- adı,
- MIME türü,
- boyutu,
- içeriği

şifreli veri içinde saklanır.

### Şifreli Dosyayı Çözme

1. İlgili sohbet için ortak anahtarın tanımlandığından emin olun.
2. Şifreli `.txt` dosyasını WhatsApp Web üzerinden indirin.
3. Dosyayı aynı mavi yükleme alanına sürükleyip bırakın.
4. Dosya mevcut sohbet anahtarıyla çözülebilirse özgün adı ve dosya türüyle otomatik olarak indirilir.

> 💡 **Not:** Eklenti, sürüklenen içeriği önce şifreli dosya olarak çözmeyi dener. Geçerli bir şifreli dosya değilse yeni bir dosya olarak şifreleyip gönderime hazırlar.

## Yedekleme

### Dışarı Aktarma

1. Eklenti açılır menüsündeki ayarlar düğmesine tıklayın.
2. **Ayarları Dışarı Aktar** düğmesine tıklayın.

Şifreli anahtar verisi aşağıdaki adla indirilir:

```text
SYChat.conf.aes
```

### İçeri Aktarma

Daha önce dışarı aktarılan dosyayı ayarlar sayfasındaki alana sürükleyip bırakın.

> ⚠️ **Uyarı:** İçe aktarma işlemi tarayıcıda bulunan mevcut şifreli verinin üzerine yazar.

> 💡 **Not:** Yedek dosyası yalnızca aynı şifreleme anahtarını kullanan eklenti sürümleri tarafından okunabilir.

## Galeri

https://github.com/user-attachments/assets/55979807-d6a3-412f-9cf0-d9f0a3e9688f

## Lisans

Bu proje [MIT Lisansı](https://github.com/seymenkonuk/whatsapp-encryption-extension/blob/main/LICENSE) ile lisanslanmıştır.
