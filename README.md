# Vault

## Vault Dökümantasyon

Vault dökümantasyonuna hoş geldiniz! Bu dokümantasyon, Vault'un tüm özellikleri ve yetenekleri için bir referans kaynağıdır. Vault ile bilgi almaya, [giriş](https://www.vaultproject.io/intro/index.html) bölümüyle başlayın ve [Başlarken kılavuzuna](https://www.vaultproject.io/intro/getting-started/install.html) kadar devam edin.

## Vault Hakkında

Bu kılavuz, Vault ile başlamak için en iyi yerdir. Vault'un ne olduğunu, hangi problemleri çözebileceğini ve nasıl hızla kullanmaya başlayacağınızı anlatmaktadır.

## Vault nedir?

Vault, gizli kalması gereken verilere(secrets) güvenli bir şekilde erişmek için kullanılan bir araçtır. Bu veri, API anahtarları, şifreler, sertifikalar vb. erişimini sıkı bir şekilde kontrol etmek istediğiniz herhangi bir şeydir. Vault, sıkı erişim kontrolü sağlamak ve detaylı bir denetim günlüğü kaydetmek için bu gizli veri ile bütünleşik bir arabirim sağlar.

### Modern bir sistem gizli tutulması gereken bir çok veriyi içerir (secrets): 

Veritabanı kimlik bilgileri, harici hizmetler için API anahtarları, hizmet odaklı mimari iletişimi için kimlik bilgileri gibi gizli bilgilere kimlerin eriştiğini platform spesifik olarak testpit etmek oldukça zordur. Rol tabanlı koruma, güvenli depolama, detaylı denetim günlüklerini yönetme gibi operesyonlar özel bir çözüm olmadan neredeyse imkansızdır. İşte Vault burada devreye girmektedir.  

### Vault un temel özellikleri şunlardır:

#### Gizli Bilgileri Güvenli Şekilde Depolama: 
Gizli bilgiler anahtar/değer (key/value) olarak Vaultta saklanabilir. Vault, bu gizli bilgileri kalıcı depolamaya yazmadan önce şifreler; bu nedenle, Gizli verilere erişmek için ham depolamaya erişmek yeterli değildir. Vault verileri diske yazabildiği gibi, depolama için Consul gibi araçlardan da faydalanabilir.

#### Değişiklik Arz Eden Gizli Veriler: 
Vault, AWS veya SQL veritabanları gibi bazı sistemler için gizli verileri talep üzerine üretilebilir. Örneğin, uygulamanın bir S3 alanına erişmesi gerektiğinde Vaulta kimlik bilgilerini sorar ve Vault talep üzerine geçerli izinlere sahip bir AWS erişim anahtarı oluşturur. Vault Gizli bilgileri oluşturulduktan sonra,  gerektiğinde (kullanım süresi dolduğunda) otomatik olarak iptal işlemini de gerçekleştirir.

#### Veri Şifreleme: 
Vault, verileri şifreleyebilir ve şifresini çözebilir. Bu, güvenlik ekiplerinin ve geliştiricelerin şifreleme operasyonlarını kendi şifreleme yöntemlerini tasarlamak zorunda kalmadan SQL gibi bir konuma depolamalarını sağlar.

#### Kullanım Süresi ve Yenileme: 
Vault üzerinde barıdırılan her gizli bilginin kullanım süresi vardır. Bu sürenin sonunda, Vault gizli bilgileri otomatik olarak iptal edecektir. Kullanıcılar, yerleşik yenileme API'leri aracılığı ile bu süreyi uzatabilir veya bu gizli erişim bilgilerini yenileyebilir.

### İptal: 
Vault, gizli bilgilerin iptalini yerleşik olarak destekliyor. Vault sadece bir tek gizli veriyi değil, aynı zamanda bir gizli veri grubunu iptal edebilir, örneğin tüm Gizli veriler belirli bir kullanıcı tarafından okunabilir veya belirli bir türdeki gizli veriler bir bir grup olarak okunabilir. Bu açıdan, iptal işlevi, sistemi kitlemede önemli rol oynamanın yanı sıra bir saldırı durumunda sistemleri güvence altına almaya da yardımcı olur.


## Kullanım Örnekleri

Vault'un kullanım alanlarını anlamadan önce, ne olduğunu bilmek faydalıdır. Bu sayfada Vault için bazı somut kullanım örnekleri listelenmiştir, ancak muhtemel kullanım örnekleri, burada anlatılanlardan çok daha fazladır.

### Genel Depolama Alanı

En azından Vault, gizli bilgilerin depolanması için kullanılabilir. Örneğin, Vault, hassas çevre değişkenleri, veritabanı kimlik bilgileri, API anahtarları vb. verileri depolamak için mükemmel bir araçtır.

Bunu dosyalarda, yapılandırma yönetiminde, veritabanında vb. düz metin olarak depolama yöntemleri ile karşılaştırıldığında,  Vault üzeriden okuma veya API'yi kullanarak bu verileri sorgulamak çok daha güvenli olacaktır. Ayrıca bu verilerin düz metin sürümünü ve kayıt defteri erişimini Vault denetim günlüğünde saklanmaktadır.

### Çalışan Kimlik Bilgileri Depolama Alanı

"Çalışan Kimlik Bilgileri Depolama Alanı", "Genel Depolama Alanı" ile benzer özellikler barındırır.  Vault, çalışanların web hizmetlerine erişmek için paylaştıkları kimlik bilgilerini depolamak için iyi bir mekanizmadır. Denetim günlük mekanizması, çalışanların eriştikleri verilerin ne olduğunu ve bir çalışan ayrıldığında ne yaptığını bilmenizi sağlar ve hangi verilerin devreden çıkarılacağını anlamak daha kolaydır.

### API Anahtar Üretimi

#### Vault yazılımının "dinamik" veri "özelliği kodlama için idealdir: 

Örneğin, Bir komut dizisinin çalışma süresi için, AWS erişim anahtarı oluşturulabilir ve daha sonra iptal edilir. Erişim anahtarları, komut dosyası çalıştırılmadan önce veya sonrasında var olmayacaktır, ancak anahtarların oluşturulması tamamen kayıt altına alınacak, log dosyalarına işlenecektir.

Bu özellik, Amazon IAM gibi bir şey kullanmanız söz konusu olduğunda; sınırlı erişim belgelerinin etkili bir şekilde oluşturmak ve kodunuz üzerinden kullanabilmenizi sağlar.

### Veri şifreleme

Gizli bilgilerin depolanabilmesinin yanı sıra, Vault başka yerlerde depolanan verileri şifrelemek ve şifrelerin çözülmesi için kullanılabilir. Bunun birincil kullanımı, uygulamaların birincil veri deposunda sakladığı halde uygulamaların verilerini şifrelemesine izin vermektir.

Bunun faydası, geliştiricilerin verileri doğru şekilde nasıl şifreleme konusunda endişelenmeleri gerekmemesidir. Şifreleme sorumluluğu Vaultadır ve geliştiriciler sadece verileri gerektiği gibi şifrelemekte / şifresini çözmektedir.

## Kullanım

### Vault İlk Adım

Önce makinenize Vault kurulmalıdır. Vault, tüm desteklenen platformlar ve mimariler için bir [ikili paket olarak](https://www.vaultproject.io/downloads.html) dağıtılır. Bu sayfada Vault kaynağından nasıl derleneceği anlatılmıyor, ancak ikili dosyanın en son kaynak kodundan derendiğinden emin olmak isteyenleri [bu dokümana](https://www.vaultproject.io/docs/install/index.html) göz atabilir.

### Vault kurulumu

Vault yazılımını yüklemek için, sisteminiz için [uygun paketi bulun](https://www.vaultproject.io/downloads.html) ve indirin. Vault bir zip dosyası olarak paketlenmiştir.

Vault dosyasını indirdikten sonra paketi açın. Vault, `vault` adlı tek bir dosya olarak çalışır. Pakette bulunan diğer dosyalar da güvenle silinebilir ve `vault`'un çalışmasını etkilemez.

Son adım, PATH ortam değişkeninde `vault` dosyasının mevcut olduğundan emin olmaktır. Linux ve Mac'te PATH ayarlama ile ilgili talimatlar için [bu sayfaya](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux) bakın. [Bu sayfada](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) da PATH'ı Windows'ta ayarlama yönergeleri bulunmaktadır.

#### Yüklemeyi Doğrulama

Vault'u kurduktan sonra, yeni bir terminal oturumu açarak ve `vault` dosyasının mevcut olup olmadığını kontrol ederek kurulumun çalıştığını doğrulayın. Vault'u çalıştırarak, aşağıdakine benzer bir yardım çıktısı görmelisiniz:

```shell
$ vault
usage: vault [-version] [-help] <command> [args]

Common commands:
    delete           Delete operation on secrets in Vault
    path-help        Look up the help for a path
    read             Read data or secrets from Vault
    renew            Renew the lease of a secret
    revoke           Revoke a secret.
    server           Start a Vault server
    status           Outputs status of whether Vault is sealed and if HA mode is enabled
    write            Write secrets or configuration into Vault

All other commands:
    audit-disable    Disable an audit backend
    audit-enable     Enable an audit backend
    audit-list       Lists enabled audit backends in Vault
    auth             Prints information about how to authenticate with Vault
    auth-disable     Disable an auth provider
    auth-enable      Enable a new auth provider
    init             Initialize a new Vault server
    key-status       Provides information about the active encryption key
    mount            Mount a logical backend
    mount-tune       Tune mount configuration parameters
    mounts           Lists mounted backends in Vault
    policies         List the policies on the server
    policy-delete    Delete a policy from the server
    policy-write     Write a policy to the server
    rekey            Rekeys Vault to generate new unseal keys
    remount          Remount a secret backend to a new path
    rotate           Rotates the backend encryption key used to persist data
    seal             Seals the vault server
    ssh              Initiate a SSH session
    token-create     Create a new auth token
    token-renew      Renew an auth token if there is an associated lease
    token-revoke     Revoke one or more auth tokens
    unmount          Unmount a secret backend
    unseal           Unseals the vault server
    version          Prints the Vault version
```

Eğer dosyanın bulunamadığını belirten bir hata alırsanız, PATH ortam değişkeniniz düzgün kurulmamıştır. Lütfen önceki adıma dönün ve PATH değişkeninizin Vault kurulu olduğu dizini içerdiğinden emin olun.

Aksi takdirde, Vault kurulu ve çalışmaya hazır!

## Vault Sunucusunun Başlatılması

Vault kurulu olduğunda bir sonraki adım Vault sunucusunu başlatmaktır.

Vault istemci/sunucu uygulaması olarak çalışır. Vault sunucusu, veri deposu ve arka uçlarla etkileşim kuran Vault mimarisinin tek parçasını oluşturur. Vault CLI aracılığıyla yapılan tüm işlemler TLS bağlantısı üzerinden sunucu ile etkileşim kurar.

Bu sayfada, sunucunun nasıl başlatıldığını anlamak için Vault sunucusunu başlatacak ve etkileşimde bulunacağız.

### Geliştirici Özellikleri İle Sunucuyu Başlatma

İlk olarak, Vault geliştirici sunucusu başlatacağız. Vault Geliştirici sunucusu, yerel ortamda Vault ile oynamak için çok güvenli ancak kullanışlı olmayan, önceden yapılandırılmış bir sunucudur. Bu kılavuzun ilerleyen kısımlarında gerçek bir sunucuyu yapılandırıp başlatacağız.

Vault Geliştirici sunucusunu başlatmak için şunu çalıştırın:

```shell
$ vault server -dev
WARNING: Dev mode is enabled!

In this mode, Vault is completely in-memory and unsealed.
Vault is configured to only have a single unseal key. The root
token has already been authenticated with the CLI, so you can
immediately begin using the Vault CLI.

The only step you need to take is to set the following
environment variable since Vault will be talking without TLS:

    export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are reproduced below in case you
want to seal/unseal the Vault or play with authentication.

Unseal Key: 2252546b1a8551e8411502501719c4b3
Root Token: 79bd8011-af5a-f147-557e-c58be4fedf6c

==> Vault server configuration:

         Log Level: info
           Backend: inmem
        Listener 1: tcp (addr: "127.0.0.1:8200", tls: "disabled")

...
```
