# Vault

Vault dökümantasyonuna hoş geldiniz! Bu dokümantasyon, Vault'un tüm özellikleri ve yetenekleri için bir referans kaynağıdır. Vault ile bilgi almaya, [giriş](https://www.vaultproject.io/intro/index.html) bölümüyle başlayın ve [Başlarken kılavuzuna](https://www.vaultproject.io/intro/getting-started/install.html) ile devam edin.


## Vault Başlangıç Kılavuzu

Bu kılavuz, Vault ile başlamak için en iyi yerdir. Vault'un ne olduğunu, hangi problemleri çözebileceğini ve nasıl hızla kullanmaya başlayacağınızı anlatmaktadır.

### Vault nedir?

Vault, gizli kalması gereken verilere(secrets) güvenli bir şekilde erişmek için kullanılan bir araçtır. Bu veriler, API anahtarları, şifreler, sertifikalar vb. erişimini sıkı bir şekilde kontrol etmek istediğiniz herhangi bir şey olabilir. Vault, sıkı erişim kontrolü sağlamak ve detaylı bir denetim günlüğü kaydetmek için bu gizli veri ile bütünleşik bir arayüze sahiptir.

Modern bir sistem, gizli tutulması gereken bir çok veriyi içerir (secrets): Veritabanı kimlik bilgileri, harici hizmetler için API anahtarları, hizmet odaklı mimari iletişimi için kimlik bilgileri gibi gizli bilgilere kimlerin eriştiğini platform spesifik olarak testpit etmek oldukça zordur. Rol tabanlı koruma, güvenli depolama, detaylı denetim günlüklerini yönetme gibi operesyonlar özel bir çözüm olmadan neredeyse imkansızdır. İşte Vault burada devreye girmektedir.  

Vault un temel özellikleri şunlardır:

#### Gizli Bilgileri Güvenli Şekilde Depolama: 
Gizli bilgiler anahtar/değer (key/value) olarak Vaultta saklanabilir. Vault, bu gizli bilgileri kalıcı depolamaya yazmadan önce şifreler; bu nedenle, Gizli verilere erişmek için ham depolamaya erişmek yeterli değildir. Vault verileri diske yazabildiği gibi, depolama için Consul gibi araçlardan da faydalanabilir.

#### Değişiklik Arz Eden Gizli Veriler: 
Vault, AWS veya SQL veritabanları gibi bazı sistemler için gizli verileri talep üzerine üretilebilir. Örneğin, uygulamanın bir S3 alanına erişmesi gerektiğinde Vaulta kimlik bilgilerini sorar ve Vault talep üzerine geçerli izinlere sahip bir AWS erişim anahtarı oluşturur. Vault Gizli bilgileri oluşturulduktan sonra, gerektiğinde (kullanım süresi dolduğunda) otomatik olarak iptal işlemini de gerçekleştirir.

#### Veri Şifreleme: 
Vault, verileri şifreleyebilir ve şifresini çözebilir. Bu, güvenlik ekiplerinin ve geliştiricelerin şifreleme operasyonlarını kendi şifreleme yöntemlerini tasarlamak zorunda kalmadan SQL gibi bir konuma depolamalarını sağlar.

#### Kullanım Süresi ve Yenileme: 
Vault üzerinde barıdırılan her gizli bilginin kullanım süresi vardır. Bu sürenin sonunda, Vault gizli bilgileri otomatik olarak iptal edecektir. Kullanıcılar, yerleşik yenileme API'ları aracılığı ile bu süreyi uzatabilir veya bu gizli erişim bilgilerini yenileyebilir.

#### İptal: 
Vault, gizli bilgilerin iptalini yerleşik olarak destekliyor. Vault sadece bir tek gizli veriyi değil, aynı zamanda bir gizli veri grubunu iptal edebilir, örneğin tüm Gizli veriler belirli bir kullanıcı tarafından okunabilir. Bu açıdan, iptal işlevi, sistemi kitlemede önemli rol oynamanın yanı sıra bir saldırı durumunda sistemleri güvence altına almaya da yardımcı olur.


### Kullanım Örnekleri

Vault'un kullanım alanlarını anlamadan önce, ne olduğunu bilmek faydalıdır. Bu sayfada Vault için bazı somut kullanım alanları listelenmiştir, ancak muhtemel kullanım alanları, burada anlatılanlardan çok daha fazladır.

#### Genel Depolama Alanı

Vault en azından gizli bilgilerin depolanması için kullanılabilir. Örneğin; Vault, hassas çevre değişkenleri, veritabanı kimlik bilgileri, API anahtarları vb. verileri depolamak için mükemmel bir araçtır.

Bunu dosyalarda, yapılandırma yönetiminde, veritabanında vb. düz metin olarak depolama yöntemleri ile karşılaştırıldığında,  Vault üzerinden okuma veya API'yi kullanarak bu verileri sorgulamak çok daha güvenli olacaktır. Ayrıca Vault bu verilerin düz metin sürümünü ve kayıt defteri erişimini denetim günlüğünde saklanmaktadır.

#### Kimlik Bilgileri Depolama Alanı

"Kimlik Bilgileri Depolama Alanı", "Genel Depolama Alanı" ile benzer özellikler barındırır.  Vault, çalışanların web hizmetlerine erişmek için paylaştıkları kimlik bilgilerini depolamak için iyi bir mekanizmadır. Denetim günlük mekanizması, çalışanların eriştikleri verilerin ne olduğunu ve bir çalışan ayrıldığında ne yaptığını bilmenizi sağlar ve hangi verilerin devreden çıkarılacağını anlamak daha kolaydır.

#### API Anahtar Üretimi

##### Vault yazılımının "dinamik" veri "özelliği kodlama için idealdir: 

Örneğin, Bir komut dizisinin çalışma süresi için, AWS erişim anahtarı oluşturulabilir ve daha sonra iptal edilir. Erişim anahtarları, komut dosyası çalıştırılmadan önce veya sonrasında var olmayacaktır, ancak anahtarların oluşturulması tamamen kayıt altına alınacak, denetim günlüğüne ()log dosyalarına) işlenecektir.

Bu özellik, Amazon IAM gibi bir şey kullanmanız söz konusu olduğunda; sınırlı erişim belgelerinin etkili bir şekilde oluşturmak ve kodunuz üzerinden kullanabilmenizi sağlar.

#### Veri şifreleme

Gizli bilgilerin depolanabilmesinin yanı sıra, Vault başka yerlerde depolanan verileri şifrelemek ve şifrelerin çözülmesi için kullanılabilir. Bu özellik, uygulamaların verilerini birincil veri deposunda sakladığı halde uygulamaların bu verilerini şifrelemesine izin vermektir.

Bunun faydası, geliştiricilerin verileri doğru şekilde nasıl şifreleme konusunda endişelenmeleri gerekmemesidir. Şifreleme sorumluluğu Vaultadır ve geliştiriciler sadece verileri gerektiği gibi şifrelemekte/şifresini çözmektedir.

### Kullanım

#### İlk Adım

Önce makinenize Vault kurulmalıdır. Vault, tüm desteklenen platformlar ve mimariler için  [ikili paket olarak](https://www.vaultproject.io/downloads.html) dağıtılır. Bu sayfada Vault kaynağından nasıl derleneceği anlatılmıyor, ancak dosyanın en son kaynak kodundan derendiğinden emin olmak isteyenler [bu dokümana](https://www.vaultproject.io/docs/install/index.html) göz atabilir.

#### Kurulum

Vault yazılımını yüklemek için, sisteminize [uygun paketi bulun](https://www.vaultproject.io/downloads.html) ve indirin. Vault bir zip dosyası olarak paketlenmiştir.

Vault dosyasını indirdikten sonra paketi açın. Vault, `vault` adlı tek bir dosya olarak çalışır. Pakette bulunan diğer dosyalar da güvenle silinebilir ve `vault`'un çalışmasını etkilemez.

Son adım, PATH ortam değişkeninde `vault` dosyasının adresinin mevcut olduğundan emin olmaktır. Linux ve Mac'te PATH ayarlama ile ilgili talimatlar için [bu sayfaya](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux) bakın. Windows'ta PATH ayarlama yönergeleri de [bu sayfada](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) bulunmaktadır.

#### Doğrulama

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

### Vault Sunucusunun Başlatılması

Vault kurulu olduğunda bir sonraki adım Vault sunucusunu başlatılmasıdır.

Vault istemci/sunucu uygulaması olarak çalışır. Vault sunucusu, veri deposu ve servislerle etkileşim kuran Vault mimarisinin tek parçasını oluşturur. Vault CLI aracılığıyla yapılan tüm işlemler, TLS bağlantısı üzerinden sunucu ile etkileşim kurar.

Bu sayfada, sunucunun nasıl başlatıldığını anlamak için Vault sunucusunu başlatacak ve etkileşimde bulunacağız.

#### Geliştirici Özellikleri İle Sunucuyu Başlatma

İlk olarak, Vault geliştirici sunucusu başlatacağız. Vault geliştirici sunucusu, yerel ortamda Vault ile oynamak için çok güvenli ancak kullanışlı olmayan, önceden yapılandırılmış bir sunucudur. Bu kılavuzun ilerleyen kısımlarında gerçek bir sunucuyu yapılandırıp başlatacağız.

Vault geliştirici sunucusunu başlatmak için şunu çalıştırın:

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

Yukarıdaki gibi bir çıktı görmelisiniz. Vault ön planda çalışmaya devam edecek; Daha sonra, farklı komutlar uygulamak için yeni bir terminal  açın.

Gördüğünüz gibi, bir geliştirici sunucusu başlattığınızda, Vault sizi yüksek sesle uyarır. Geliştirici sunucusu tüm verilerini belleğe kaydeder (ancak yine de şifrelenmiş), localhost üzerinde TLS dinler ve otomatik olarak açar ve size mühür açılış anahtarını (unseal key) ve kök erişim anahtarını gösterir. Bütün bunların ne anlama geldiğini birazdan gözden geçireceğiz.

Geliştirici sunucusu ile ilgili önemli nokta, yalnızca geliştirme amaçlı olmasıdır.

> Not: Geliştirici sunucusunu gerçek bir ortamda (production) çalıştırmayın.

Geliştirici sunucusu gerçek bir ortamda (production) çalıştırıldığında, verileri bellekte depolar ve her yeniden başlatmada tüm gizli verileriniz silineceğinden bunun size pek yararı olmaz.

Geliştirici sunucusunu çalışırken:

1. Yeni bir terminal oturumu açın.

2. `export VAULT_ADDR='....` komutunu terminal çıktısından kopyalayın ve çalıştırın. Vault istemcisini, geliştirici sunucumuzla konuşacak şekilde yapılandıracaktır.

3. Mühür açma anahtarını (unseal key) bir yere kaydedin. Bunu nasıl güvenli bir şekilde saklayacağınız konusunda endişelenmeyin. Şimdilik, herhangi bir yere kaydedin.

4. Adım 3 ile aynı işlemi yapın, ancak `root` anahtarı ile yapın. Bunu daha sonra kullanacağız.

#### Sunucunun Çalıştığını Doğrulayın

`vault status` komutunu çalıştırarak sunucunun çalıştığını doğrulayın. Bu başarılı sonuç almalı ve 0 çıkış kodu ile çıkmalıdır. Bağlantı açma konusunda bir hata görürseniz, yukarıdaki`export VAULT_ADDR='....` komutunu düzgün bir şekilde yürüttüğünüzden emin olun.

Başarılı olursa, çıktı aşağıdaki gibi görünmelidir:

```shell
$ vault status
Sealed: false
Key Shares: 1
Key Threshold: 1
Unseal Progress: 0

High-Availability Enabled: false
```

Çıktı farklı görünüyorsa, özellikle sayılar farklıysa veya Vault mühürlenmişse (sealed=true), geliştirici sunucusunu yeniden başlatın ve tekrar deneyin. Bunların farklı olmasının tek nedeni, daha önce bu rehberden bir geliştirici sunucusu çalıştırmamanızdan kaynaklanır.

Bu çıktının daha sonra rehberde ne anlama geldiğini anlatacağız.

Tebrik ederiz! İlk Vault sunucusunu başlattınız. Henüz herhangi bir gizli veri(secret) depolamadık, ancak bunu bir sonraki bölümde yapacağız.

### İlk Gizli Veri

Şimdi geliştirci sunucumuz ayakta ve çalışıyor. İlk gizli verimizi yazıp okuyabiliriz.

Vault'un temel özelliklerinden birisi gizli verilermizi güvenli bir şekilde okuma ve yazma yeteneğidir. Bu sayfada, CLI kullanarak bunu yapacağız, ancak Vault'un yeteneklerinden faydalanabileciğimiz eksiksiz bir HTTP API'si olduğunu bilmekte fayda var.

Vault'a yazılan gizli bilgiler önce şifrelenir daha sonra depolama alanına yazılır. Geliştirici sunucusunda, depolama alanı bellektir, ancak gerçek ortamda büyük olasılıkla disk veya Consul olacaktır. Vault veriyi depolama sürücüsüne teslim edilmeden önce şifreler. Depolama mekanizması şifrelenmemiş veriyi görmez ve Vault olmadan şifresini çözmek için gerekli araçlara sahip değildir.

#### Gizli Bir Veriyi Yazma

Hadi ilk gizli verimizi yazalım. Bu işlemi Aşağıda gösterildiği gibi `vault write` komutuyla yapıyoruz:

```shell
$ vault write secret/hello value=world
Success! Data written to: secret/hello
```

Bu, eşitlik `value=world` `secret/hello` yoluna yazılır. Yolları daha ayrıntılı olarak sonra ele alacağız, ancak şimdilik yolun `secret`  ön ekine sahip olması önemlidir, aksi takdirde bu örnek çalışmayacaktır. `secret/` öneki, gizli verilermizin okunup yazılabileceği yeri gösterir.

İsterseniz çoklu veri parçaları halinde yazabilirsiniz:

```shell
$ vault write secret/hello value=world excited=yes
Success! Data written to: secret/hello
```

`vault write` çok güçlü bir komutur. Komut satırından doğrudan veri yazmanın yanı sıra, değerler ve anahtar çiftlerini STDIN'den ve dosyalardan okuyabilir. Daha fazla bilgi edinmek için, [`vault write` belgelerine](https://www.vaultproject.io/docs/commands/read-write.html) bakın.

> Uyarı: Belgeler, `anahtar=değer` temelli girdiyi kullanır, ancak mümkünse dosyaları kullanmak daha güvenlidir. CLI yoluyla veri göndermek genellikle terminal geçmişine kaydedilir. Gerçekten gizli bilgilerinizin güvenliği için dosyaları kullanın. Daha fazla bilgi için STDIN'den okumaya ilişkin yukarıdaki bağlantıya bakın.

#### Gizli Bilgileri Okuma

Tahmin edebileceğiniz gibi gizli veriler `vault read` komutu ile okunabilir:

```shell
$ vault read secret/hello
Key                 Value
---                 -----
refresh_interval    768h0m0s
excited             yes
value               world
```

Gördüğünüz gibi, yazdığımız değerler bize geri veriliyor. Vault veriyi depodan okuyor ve bizim için çözümlüyor.

Çıktı biçimi, `awk` gibi bir araç ile Tabular olarak düzenlenmiş.

Tabular çıktı biçime ek olarak, eğer `jq` gibi bir araçla çalışıyorsanız, verileri JSON formatında çıktı alabilirsiniz:

```shell
$ vault read -format=json secret/hello
{
    "request_id": "68315073-6658-e3ff-2da7-67939fb91bbd",
    "lease_id": "",
    "lease_duration": 2764800,
    "renewable": false,
    "data": {
        "excited": "yes",
        "value": "world"
    },
    "warnings": null
}
```

Bu format, bazı ek bilgiler içermektedir. Birçok gizli veri yönetim servisi (backend), Gizli bilgiler için zamanaşımıyla diğer sistemlerin erişimini kısıtlayan sağlayan kullanım süresini destekler. Bu durumlarda lease_id bir kullanım süresi içerecek ve lease_duration saniye bazında kullanımının geçerli olduğu süreyi içerecektir.

Verilerimizin burada detaylı bir dökümünü görüyoruz. JSON çıktısı komut dosyaları için çok yararlıdır. Örneğin aşağıdaki örnekte `excited` Gizli verisinin değerini elde etmek için `jq` aracını kullanıyoruz:

```shell
$ vault read -format=json secret/hello | jq -r .data.excited
yes
```

#### Bir Gizli Veriyi Silme

Artık gizli bir veriyi okuma ve yazmayı öğrendiğimize göre, silme işlemine geçebiliriz. Bunu `vault delete` ile yapabiliriz:

```shell
$ vault delete secret/hello
Success! Deleted 'secret/hello' if it existed.
```

### Gizli Veri Yönetim Servisleri

Daha önce, gizli verileri Valut'a nasıl yazacağımızı ve okuyacağımızı gördük. Bunu yapmak için, `secret/` önekini kullandık. Bu önek hangi gizli veri yönetim servisinin kullanılacağını belirtir. Varsayılan olarak Vault, `generic` adlı bir gizli veri yönetim servisini `secret`'a bağlar. `generic` gizli veri yönetim servisi, ham veriyi disk üzerinden okur ve yazar.

Vault `generic gizli veri yönetim servisine` ek olarak farklı gizli veri yönetim servislerini de desteklemektedir ve bu özellik Vault'u benzersiz kılan özelliktir. Örneğin, AWS gizli veri yönetim servisi, isteğe bağlı olarak AWS erişim anahtarlarını dinamik olarak üretir. Başka bir örnek - bu tür bir gizli veri yönetim servisi henüz mevcut değil - doğrudan bir [HSM](https://en.wikipedia.org/wiki/Hardware_security_module)'ye veri yazar ve okur. Vault olgunlaştıkça giderek daha fazla gizli veri yönetim servisi eklenecektir.

Vault Gizli Veri Yönetim Servislerini Dosya Sistemine Çok Benzer Şekilde Kullanır: 

Depolama birimleri belirli yollar (path) yardımı ile tanımlanır. Örneğin `generic gizli veri yönetim servisi` `secret/` önekini alarak tanımlanır.

Bu sayfada, gizli veri yönetim servislerinin tanımlanmasını ve gizli veri yönetim servisleri ile gerçekleştirilebilecek işlemler hakkında bilgi edineceğiz. İlerleyen bölümlerde dinamik olarak gizli veri oluşturacağımız işlemlerde buradaki bilgilerden faydalanacağız.

#### Gizli Veri Yönetim Servisi Tanımlama

İlk başta, başka bir `generic`  gizli veri yönetim servisi elde edelim. Normal bir dosya sistemi gibi Vault da birden fazla gizli veri yönetim servisi tanımlanabilir. Farklı erişim denetimi ilkeleri veya farklı yollar için yapılandırmalar istiyorsanız bu özellik işinize yarayacaktır.

Gizli Veri Yönetim Servisi Tanımlama:

```shell
$ vault mount generic
Successfully mounted 'generic' at 'generic'!
```

Varsayılan olarak, depomalama tanımı ile gizli veri yönetim servisi aynı isimde olacaktır. Bunun nedeni, zamanın %99'unu depolama tanımını özelleştirmek için harcamamamızdır. Bu örnekte, `generic` gizli veri yönetim servisini `generic/` adresine kurduk.

Depolama Tanımlarını `vault mounts` ile inceleyebilirsiniz: 

```shell
$ vault mounts
Path      Type     Description
generic/  generic
secret/   generic  generic secret storage
sys/      system   system endpoints used for control, policy and debugging
```

Görüldüğü gibi `generic/` depolama tanımının yanı sıra `secret/` ve `sys/` yolunu da içermektedir. Bu konuya şimdilik değinmeyeceğiz. Bilgi vermek açısından `sys/` bağlantı noktası Vault ile iletişime geçmek için kullanılır.

Herşeyin yolunda olduğundan emin olmak için bazı gizli verileri yeni gizli veri yönetim servisine yazın ve okuyun. İlk olarak `secret/` erişim noktasına yazın ve `generic/` yolu ile bu değerleri okuyamadığınızı göreceksiniz: Aynı depolama alanını paylaşmalarına rağmen, hiçbir gizli veriyi paylaşmıyorlar. Buna ek olarak, (aynı türden veya farklı türden) gizli veri yönetim servisleri de diğer gizli veri yönetim servislerinin verilerine erişemez; Yalnızca bağlama noktası/servis tanımı içindeki verilere erişebilirler.

#### Gizli Veri Yönetim Servisini Kaldırma

Bir gizli veri yönetim servisi kaldırıldığında, bütün gizli veriler iptal edilir ve silinir. Bu işlemlerden herhangi biri başarısız olursa, gizli veri yönetim servisi kaldırma işlemi iptal edilir.

```shell
$ vault unmount generic/
Successfully unmounted 'generic/' if it was mounted
```

Bir gizli veri yönetim servisini kaldırdığınızda, tekrar eklemeniz mümkündür. Depolama birimini tekrar ekleme, depolama tanımının/bağlantı noktasını değiştirir.  Bu operasyonda yıkıcıdır. Saklanan veriler korunsa da gizli veriler `secret/` yoluyla bağlantılı olduğu için iptal edilmiştir. 

#### Gizli Veri Yönetim Servisi Nedir?

Artık bir gizli veri yönetim servisini ekdiğinize ve çıkardığınıza göre: Gizli Veri Yönetim Servisi nedir ve bu gizli veri yönetim servisi tanımlama sisteminin anlamı nedir?

Vault [sanal bir dosya sistemi](https://en.wikipedia.org/wiki/Virtual_file_system) gibi çalışır. Okuma, yazma ve silme işlemleri gizli veri yönetim servisine yönlendirilir ve gizli veri yönetim servisi bu işlemleri gerçekleştirir. Örneğin `generic` gizli veri yönetim servisi, basit bir şekilde verileri depolama alanına (Storage Backend) iletir. (şifreledikten sonra)

Bununla birlikte `AWS Gizli Veri Yönetim Servisi` (yakında göreceğiz), IAM ilkelerini okur, yazar ve erişim anahtarına erişebilir. Yani bu işlemleri `vault read aws/deploy` ile gerçekleştirken, okuma `aws/deploy` fiziksel yolu üzerinde gerçekleşmez. Buun yerine AWS Depolama birimi dağıtım ilkesine uygun bir erişim anahtarı üretir.

Bu soyutlama inanılmaz güçlüdür. Vault arayüzü fiziksel sistemler ile doğrudan bağlantı kurabilmenin yanı sıra SQL veritabanları, HSM'ler gibi sistemleride arayüze bağlar. Fakat bu fiziksel sistemlere ek olarak Vault, daha eşsiz ortamlarla etkileşim kurabilir: AWS IAM, dinamik SQL Kullanıcı yaratma vb hepsi aynı okuma/yazma arabirimini kullanmaktadır.

### Gizli Bilgi Üretme - Dinamik Gizli Veri

Vault'a gizli verilerimizi yazdık ve gizli veri yönetim servisi tanımlama gibi özelikleri anladık. Şimdi, Vault'un bir sonraki temel özelliği olan: Gizli veri üretme operesyonlarına geçeceğiz.

Gizli veri üretme yani dinamik gizli veriler, erişildikleri zaman üretilen gizli verilerdir ve `İlk Gizli Veri` bölümün deki gibi el yordamıyla yazılmamıştır. Bu sayfada AWS erişim anahtarlarını dinamik olarak oluşturmak için yerleşik AWS gizli veri yönetim servisini kullanacağız.

Dinamik Gizli Verinin gücü, sadece okunmadan önce var olmamalarıdır; bu nedenle, birinin bu gizli veriyi veya başka bir müşteri verisini çalma riski yoktur. Vault yerleşik iptal mekanizmalarına sahip olduğundan, dinamik gizli veri kullanımdan hemen sonra iptal edilebilir ve gizli verilerin var olduğu süreyi en aza indirebilir.

> Not: Bu sayfayı başlatmadan önce, lütfen bir AWS hesabı için [kayıt](https://aws.amazon.com/) olunuz. Maliyete neden olan hiçbir özelliği kullanmayacağız, bu nedenle herhangi bir şey için ücret ödememelisiniz. Bununla birlikte, doğabilecek herhangi bir masrafdan biz sorumlu değiliz.

#### AWS Gizli Veri Yönetim Servisini Tanımlama

İlk dinamik gizli verimizi üretelim. Dinamik olarak AWS erişim anahtarı çifti oluşturmak için AWS gizli veri yönetim servisini kullanacağız. İlk olarak, AWS gizli veri yönetim servisini tanımlayın:


```shell
$ vault mount aws
Successfully mounted 'aws' at 'aws'!
```

AWS gizli veri yönetim servisi `aws/` adresine monte edildi. Bir önceki bölümde değindiğimiz gibi, farklı gizli veri yönetim servisleri  farklı davranışlar sergiler ve bu örnekte AWS gizli veri yönetim servisi, AWS erişim kimlik bilgilerini oluşturmak için dinamik bir arayüz oluşturur.

#### AWS Gizli Veri Yönetim Servisini Yapılandırma

AWS gizli veri yönetim servisi tanımlandığında, ilk adım, onu diğer kimlik bilgilerini oluşturmak için kullanılacak AWS kimlik bilgileri ile yapılandırmaktır. Şimdilik, AWS hesabınız için `root` anahtarlarını kullanın.

Depolama birimini yapılandırmak için, `vault write` komutunu `aws/config/root` yolu üzerinde kullanacağız:

```shell
$ vault write aws/config/root \
    access_key=AKIAI4SGLQPBX6CSENIQ \
    secret_key=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
Success! Data written to: aws/config/root
```

Dinamik veri gizli veri yönetim servislerinin sadece okurken/yazarken veriyi oluşturduğunu unutmayın, tanımlı olan bu yoldaki yapılandırmayı saklayacaktır ancak daha sonra okunamıyacaktır:

```shell
$ vault read aws/config/root
Error reading aws/config/root: Error making API request.

URL: GET http://127.0.0.1:8200/v1/aws/config/root
Code: 405. Errors:

* unsupported operation
```

Kimlik bilgilerini güvenli tutmaya yardımcı olmak için AWS gizli veri yönetim servisi, kimlik bilgilerini `root` yetkisi kullansanız bile onları okumanıza izin vermez.

#### Rol Yaratmak

Bir sonraki adım AWS gizli veri yönetim servisini IAM ilkesiyle yapılandırmaktır. IAM, sınırlı API izinlerine sahip yeni kimlik bilgileri oluşturmak için AWS'nin kullandığı sistemdir.

AWS gizli veri yönetim servisi, oluşturulan kimlik bilgilerini ilişkilendirmek için IAM ilkesini gerektirir. Bu örnek için yalnızca bir politika yazacağız, ancak birçok politikayı arka uç ile ilişkilendirebilirsiniz. Aşağıdaki içeriğe sahip `policy.json` adlı bir dosya oluşturun:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1426528957000",
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

Bu, kullanıcının Amazon EC2 içinde herhangi bir işlem yapmasını sağlayan temel bir IAM politikasıdır. Kaydelien ilkeyi, Vault'a tanıtım ve yeni bir rol oluşturun:

```shell
$ vault write aws/roles/deploy policy=@policy.json
Success! Data written to: aws/roles/deploy
```

Vault'a bir IAM ilkesi yazmak için `aws/roles/<ADI>` gibi özel bir yol kullanıyoruz. Ayrıca, bir dosyanın içeriğini değer olarak yazmak için `vault write` komutunda özel bir parametre olarak `@filename` i kullandık.

#### Gizli Veri Üretme

AWS gizli veri yönetim servisini yapılandırdık ve bir rol oluşturduk, şimdi bu rol için bir erişim anahtarı çifti talep edebiliyoruz. Bunu yapmak için, `aws/creds/<ADI>` özel yolunu okuyun, burada ADI rolün adıdır:

```shell
$ vault read aws/creds/deploy
Key             Value
---             -----
lease_id        aws/creds/deploy/0d042c53-aa8a-7ce7-9dfd-310351c465e5
lease_duration  768h0m0s
lease_renewable true
access_key      AKIAJFN42DVCQWDHQYHQ
secret_key      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
security_token  <nil>
```

Harika! Artık `access_key` ve `secret_key` AWS içerisinde herhangi bir EC2 işlemini gerçekleştirmek için kullanılabilir. İsterseniz, çalıştığını doğrulayabilirsiniz. Ayrıca, bu anahtarların yeni olduğunu, daha önce girdiğiniz anahtarlar olmadığını farketmişsinizdir. 

Yukarıdaki lease_id, yenileme, iptal etme vb. operasyonlar için Vault tarafından kullanılan özel bir kimliktir. Şimdi Lease ID'nizi kopyalayın ve kaydedin.

#### Gizli Veriyi İptal Etme

Döngüyü tamamlayalım ve bu gizli veriyi şimdi iptal edelim, ve tamamen yok edelim. Gizli veri iptal edildikten sonra, erişim anahatarları artık çalışmayacaktır.

Gizli veriyi iptal etmek için, `vault revoke` çalıştırdığınızda okuduğunuz vault tan elde ettiğiniz dan çıkan LEASE ID yi kullanın :

```shell
$ vault revoke aws/creds/deploy/0d042c53-aa8a-7ce7-9dfd-310351c465e5
Success! Revoked the secret with ID 'aws/creds/deploy/0d042c53-aa8a-7ce7-9dfd-310351c465e5', if it existed.
```

Tamadır! AWS hesabınıza bakarsanız, hiçbir IAM kullanıcısı olmadığını farkedeceksiniz. Üretilen erişim anahtarlarını kullanmaya çalışırsanız, artık çalışmadığını göreceksiniz.

Dinamik gizli veri oluşturma ve iptal etme araçları yardımı ile dinamik gizli verilerle çalışmanın ne kadar kolay olduğunu görmeye başladık. Bu verilerin yalnızca ihtiyaç duydukları süre boyunca varolmalarını garantileyebiliyoruz.

### Vault Yardım Menusu

Şu ana kadar `vault write` ve `vault read` okuma/yazma pratikleri üzerine çalıştık: `secret/` yolu ile generic gizli veri yönetim servisini  ve `aws/` yolu ile AWS gizli veri yönetim servisi üzerinden dinamik AWS kimlik bilgileri oluşturduk. Her iki durumda da, her gizli veri yönetim servisinin yapısı ve kullanımı farklılıklar gösterdi; örneğin AWS gizli veri yönetim servisi, `aws/config` gibi özel yollara sahip olduğunu gördük.

Hangi yolları kullanacağınızı belirlemek için sürekli ezberlemek veya belgelere referans vermek zorunda kalmadan, doğrudan Vault ile çalışan bir yardım sistemi kurduk. Bu yardım sistemine API veya komut satırı üzerinden erişilebilir ve tanımlanmış herhangi bir gizli veri yönetim servisi için okunaklı bir yardım üretir.

Bu sayfada, bu yardım sistemini nasıl kullanacağınızı öğreneceğiz. Vault'u kullanırken çok değerli bir araçtır.

#### Gizli Veri Yönetim Servislerine Genel Bakış

Bunun için AWS gizli veri yönetim servisinin takılı olduğunu varsayacağız. Değilse, `vault mount aws` ile bağlayın. Bir AWS hesabınız olmasa bile yine de AWS gizli veri yönetim servisine bağlayabilirsiniz.

Depolama birimi tanımlıyken `thevault path-help` komutunu deneyelim:

```shell
$ vault path-help aws
## DESCRIPTION

The AWS backend dynamically generates AWS access keys for a set of
IAM policies. The AWS access keys have a configurable lease set and
are automatically revoked at the end of the lease.

After mounting this backend, credentials to generate IAM keys must
be configured with the "root" path and policies must be written using
the "roles/" endpoints before any access keys can be generated.

## PATHS

The following paths are supported by this backend. To view help for
any of the paths below, use the help command with any route matching
the path pattern. Note that depending on the policy of your auth token,
you may or may not be able to access certain paths.

    ^config/lease$
        Configure the default lease information for generated credentials.

    ^config/root$
        Configure the root credentials that are used to manage IAM.

    ^creds/(?P<name>\w+)$
        Generate an access key pair for a specific role.

    ^roles/(?P<name>\w+)$
        Read and write IAM policies that access keys can be made for.
```

`vault path-help` komutu olası yolları listeler.  Bir gizli veri yönetim servisi için temel adresini belirterek, bu gizli veri yönetim servisinin genel özelliklerini bize listeler. Yardımın sadece bir açıklama içerdiğini değil, aynı zamanda bu gizli veri yönetim servisi için güzergâhları eşleştirmek için kullanılan tam düzenli ifadelerin, güzergahın neyle ilgili olduğunu da söylemektedir.

#### PATH Yardımı

Genel bilgiyi aldıktan sonra, tek tek bir yol için yardım alarak daha derinlere dalmaya devam edebiliriz. Bunun için, `vault path-help` komutunu bilgi edinmek istediğiniz ifadeyle eşleşen bir yolla birlikte kullanın. Yolun aslında çalışması gerekmediğini unutmayın.

```shell
$ vault path-help aws/creds/operator
Request:        creds/operator
Matching Route: ^creds/(?P<name>\w+)$

Generate an access key pair for a specific role.

## PARAMETERS

    name (string)
        Name of the role

## DESCRIPTION

This path will generate a new, never before used key pair for
accessing AWS. The IAM policy used to back this key pair will be
the "name" parameter. For example, if this backend is mounted at "aws",
then "aws/creds/deploy" would generate access keys for the "deploy" role.

The access keys will have a lease associated with them. The access keys
can be revoked by using the lease ID.
```

Bir yol içinde, bu yolun gerektirdiği parametreleri içerir. Bazı parametreler adresin kendisinden gelmektedir. Bu durumda, `name` parametresi rota düzenli ifadesinin adlandırılmış bir eşleniğidir.

Bu yolun ne yaptığının bir açıklaması da var.

Daha fazla yol keşfedebilirsiniz! Diğer gizli veri yönetim servislerini kurun, yardım sistemlerini dolaşın ve yaptıklarını öğrenin. Örneğin,  `thesecret/` yolu ile `generic` gizli veri yönetim servisi hakkında bilgi edinin.

### Kimlik Doğrulama

Artık Vault'un temellerini ve nasıl kullanacağımızı bildiğimizden Vault'un kendisinin kimliğinin nasıl doğrulanacağını anlamak önemlidir. Bu noktaya kadar kimlik doğrulaması yapmamız gerekmiyordu çünkü geliştirici modunda Vault sunucusunun başlatılması bizi otomatik olarak `root` kullanıcısı olarak kaydediyordu. Gerçek kullanımda, neredeyse her zaman el yordamı ile kimliğinizi doğrulamanız gerekir.

Bu sayfada, kimlik doğrulama hakkında özel olarak konuşacağız. Bir sonraki sayfada yetkilendirme hakkında konuşacağız. Kimlik doğrulama, Vault kullanıcısına bir kimlik tahsis etme mekanizmasıdır. Bir kimlikle ilişkili erişim kontrolü, izinleri ve yetkileri bu sayfada ele alınmayacaktır.

Vault, kurulabilir kimlik doğrulama sistemlerine sahiptir ve kuruluşunuz için en iyi sistemi kullanarak Vault ile kimlik doğrulamasının etkisi artırlır. Bu sayfada, token backend'in yanı sıra GitHub sistemlerini kullanacağız.

#### Tokens - Erişim Anahtarları

Diğer kimlik doğrulama sistemlerini gözden geçirmeden önce `token` kimlik doğrulamasını inceleyeceğiz. Token kimlik doğrulaması Vault'da varsayılan olarak etkindir ve devre dışı bırakılamaz.

Vault geliştirici sunucusu -dev ile başlattığında, Vault `root` tokenı oluşturur. `root` token Vault'u yapılandırmak için ilk erişim anahtarıdır. `root` ayrıcalıklarına sahiptir, bu nedenle Vault içindeki herhangi bir işlemi gerçekleştirebilir. Bir sonraki bölümde ayrıcalıkları nasıl sınırlayacağımızı ele alacağız.

`vault token-create` komutunu kullanarak daha fazla `token` yaratabilirsiniz:

```shell
$ vault token-create
Key             Value
token           c2c2fbd5-2893-b385-6fa5-30050439f698
token_accessor  0c1c3317-3d58-17e5-c1a9-3f54fa26610e
token_duration  0
token_renewable true
token_policies  [root]
```

Varsayılan olarak, bu mevcut tüm erişim denetimi ilkelerini devralan geçerli erişim anahtarının alt üyesini oluşturur. Buradaki "alt üye" konsepti önemlidir: `Token`ların her zaman bir ebeveyni vardır ve ebeveyn işaretçisi iptal edildiğinde, `alt üyeler` de tek seferde tümüyle iptal edilebilir. Bu, bir kullanıcının erişimi kaldırırken, kullanıcının oluşturduğu tüm alt `tokenlar` için erişimi kaldırmasını kolaylaştırır.

Bir `token` yaratıldıktan sonra, `vault token-revoke` ile iptal edebilirsiniz:

```shell
$ vault token-revoke c2c2fbd5-2893-b385-6fa5-30050439f698
Success! Token revoked if it existed.
```


Bir önceki bölümde,`vault revoke` komutunu kullandık. Bu komut yalnızca gizli verileri iptal etmek için kullanılır. `Token` ları iptal etmek için `vault token-revoke` komutu kullanılmalıdır.

Bir `token` ile kimlik doğrulaması yapmak için `vault auth` komutunu kullanın:


```shell
$ vault auth d08e2bd5-ffb0-440d-6486-b8f650ec8c0c
Successfully authenticated! The policies that are associated
with this token are listed below:

root
```

Bu komut Vault kimliğini doğrular. Kimliğinizi doğrular ve `token` ile ilişkili erişim politikalarını size bildirir. Vault yetkilisini test etmek isterseniz, önce yeni bir `token` oluşturduğunuzdan emin olun.

#### Kimilik Doğrulama Sistemleri

Token kimlik doğrulama sistemine ek olarak, başka kimlik doğrulama veya yetkili sistemleri etkinleştirilebilir.  Vault ile tanımlanan Kimilik Doğrulama Sistemleri ile bazı alternatif yöntemlere sahip oluruz. Bu kimlikler, Token kimlik doğrulama sistemi gibi bir dizi erişim politikasına bağlıdır. Örneğin masaüstü ortamları için özel anahtar veya GitHub tabanlı kimlik doğrulama kullanılabilir. Sunucu ortamları için, paylaşılan gizli veriler en iyisi seçenek olabilir. Kimilik Doğrulama Sistemleri, bize farklı kimlik doğrulama yöntemleri arasında seçme esnekliği sağlar.

Örnek olarak, GitHub'ı kullanarak kimlik doğrulamasını yapalım. Önce, GitHub Kimilik Doğrulama Sistemini etkinleştirin:

```shell
$ vault auth-enable github
Successfully enabled 'github' at 'github'!
```

Kimilik Doğrulama Sistemleri, gizli veri gizli veri yönetim servisleri gibi tanıtılır; temel fark, Kimilik Doğrulama Sistemleri  her zaman `auth/` öneki alır. Bu yüzden, daha önce kurduğumuz GitHub Kimilik Doğrulama Sistemine `auth/github` adresinden erişilebilir. Bunun hakkında daha fazla bilgi edinmek için `vault path-help` yardımını kullanabilirsiniz.

GitHub Kimilik Doğrulama Sistemi etkinleştirildiğinde, öncelikle yapılandırmamız gerekir. GitHub için, kullanıcıların hangi organizasyonun parçası olması gerektiğini ve ekibin hangi politikayla eşleştiğini söylemeliyiz:

```shell
$ vault write auth/github/config organization=hashicorp
Success! Data written to: auth/github/config

$ vault write auth/github/map/teams/default value=default
Success! Data written to: auth/github/map/teams/default
```

Yukarıdaki bölüm, GitHub Kimilik Doğrulama Sistemizin yalnızca hashicorp kuruluşundaki kullanıcıları kabul etmek için (kendi organizasyonunu doldurmalısın) ve ekibi yerleşik bir politika olan varsayılan politikaya eşleştirmek için yapılandırdı.Şu anda bir sonraki bölüme geçebiliriz.

GitHub etkinleştirildiğine göre, artık `vault auth` kullanarak kimlik doğrulaması yapabiliriz:

```shell
$ vault auth -method=github token=e6919b17dd654f2b64e67b6369d61cddc0bcc7d5
Successfully authenticated! The policies that are associated
with this token are listed below:

default
```

İşte Oldu! GitHub'ı kullanarak kimlik doğrulamasını yaptık. Varsayılan ilke (default), benim kimliğiyle ilişkiliydi. `Erişim Anahtarı` (token) [kişisel `Erişim Anahtarı`](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) olmalıdır.

Farkettiyseniz, komutları çalıştırmak için daha önce var olan `root` erişim anahtarı ile yeniden kimlik doğrulaması yapıldı.

Herhangi bir Kimilik Doğrulama Sisteminin, kimlik doğrulamasını `vault token-revoke` komutunu kullanarak iptal edebilirsiniz; Bu işlem önek kullanılarak yapılır. Örneğin, tüm GitHub belirteçlerini iptal etmek için aşağıdakileri çalıştırabilirsiniz:

```shell
$ vault token-revoke -mode=path auth/github
```

İşiniz bittiğinde, Kimilik Doğrulama Sistemini `vault auth-disable` ile devre dışı bırakabilirsiniz. Kimilik Doğrulama Sistemi üzerinde doğrulanmış tüm kullanıcıları geçersiz kılacaktır.

```shell
$ vault auth-disable github
Disabled auth provider at path 'github'!
```

### Erişim Kontrol Politikaları (ACLs)

"Vault" daki erişim kontrol politikaları, bir kullanıcının erişim haklarını kontrol eder. Son bölümde kimlik doğrulamayı öğrendik. Bu bölüm yetkilendirme ile ilgilidir.

Vault Kimlik doğrulama için  etkinleştirilebilen ve kullanılabilen birden çok seçenek veya sistem içerir. Yetki ve erişim kontrolü politikaları için Vault daima aynı biçimi kullanır. Tüm kimlik doğrulama sistemleri, kimlikleri "theVault" ile yapılandırılan temel politikaları ile eşleştirmelidir.

Vault'u başlatıldığında, kaldırılamayan , önceden oluşturulmuş özel bir politikaya sahiptir: `root` erişim politikası. Bu politika Vault'taki her şeye süper kullanıcı erişimi sağlayan özel bir politikadır. `root` politikasıyla eşlenen bir kimlik ile her şeyi yapabilirsiniz.

#### Politika Formatı

Vault'daki politikalar [HCL](https://github.com/hashicorp/hcl) ile biçimlendirilir. HCL, JSON ile uyumlu, kolay okunabilen bir yapılandırma biçimidir, bu nedenle de JSON'u  da kullanabilirsiniz. Örnek bir politika aşağıda gösterilmektedir:


```hcl
path "secret/*" {
  policy = "write"
}

path "secret/foo" {
  policy = "read"
}

path "auth/token/lookup-self" {
  policy = "read"
}
```

Politika formatı, erişim denetimini belirlemek için API adresinde bir önek eşleştirme sistemi kullanır. Kesin tanımlanmış politika, tam eşleşme veya en uzun ön ek eşlemesi olabilir. Vault'daki herşeye API aracılığıyla erişilmesi gerektiğinden, Vault'un bütün özellikleri üzerinde, gizli veri yönetim servislerinin montajı, kimlik doğrulaması ve gizli bilgilere erişimi de dahil olmak üzere tüm yönleriyle sıkı bir denetim sağlar.

Yukarıdaki politikaya sahip bir kullanıcı, `secret/` yoluna herhangi bir gizli veri yazabilir, ancak sadece salt okunur `secret/foo`  verisini okuma erişimine sahiptir. Politikalar varsayılan olarak red eğilimdedir; dolayısıyla belirtilmemiş bir yola erişim mümkün değildir. Politika söz dizimi Vault 0.2 versiyonunda  biraz değişti, ayrıntılar için [bu sayfaya](https://www.vaultproject.io/docs/concepts/policies.html) bakın.

Yukarıdaki Politikayı acl.hcl adlı bir dosyaya kaydedin.

#### Erişim Politikası Yazmak

Bir politika yazmak için `vault policy-write` komutunu kullanın:

```shell
$ vault policy-write secret acl.hcl
Policy 'secret' written.
```

`vault policies` ile kullanılabilen politikaları listeleyebilir ve `vault policies <name>` ile varolan bir politikanın içeriğini görebilirsiniz. Yalnızca `root` erişime sahip kullanıcılar bunu yapabilir.

#### Politikaların Test Edilmesi

Politikayı kullanmak için, bir `token` oluşturup bu politikaya atayalım. Daha sonra bir `root` kullanıcıya kimlik doğrulaması yapabilmeniz için `root` erişim anahtarınızı başka bir yere kaydedin.

```shell
$ vault token-create -policy="secret"
Key             Value
token           d97ef000-48cf-45d9-1907-3ea6ce298a29
token_accessor  71770cc5-14da-f0af-c6ce-17a0ae398d67
token_duration  2764800
token_renewable true
token_policies  [default secret]

$ vault auth d97ef000-48cf-45d9-1907-3ea6ce298a29
Successfully authenticated!
token: d97ef000-48cf-45d9-1907-3ea6ce298a29
token_duration: 2591938
token_policies: [default, secret]
```

Artık, `secret/` için veri yazabildiğinizi test edebilirsiniz, tabi yalnızca `thesecret/foo` adresinden okuyabilirsiniz:

```shell
$ vault write secret/bar value=yes
Success! Data written to: secret/bar

$ vault write secret/foo value=yes
Error writing data to secret/foo: Error making API request.

URL: PUT http://127.0.0.1:8200/v1/secret/foo
Code: 403. Errors:

* permission denied
```

Aynı zamanda politikaya göre sys'e erişiminiz yok, bu nedenle `vault mounts` gibi komutlar da kullanılamaz.

#### Politikarı Kimlik Doğrulama Sistemleri İle Eşleştirme

Vault tekil politika sistemine sahiptir. Çoklu kimlik doğrulama sistemleri  bağlayabileceğiniz kimlik doğrulamadan farklıdır. Monte edilmiş herhangi bir kimlik doğrulama sistem kimliği bu temel ilkelerle eşlemelidir.

Her kimlik doğrulama sistemi kendine özgü olduğundan haritalamanın nasıl yapıldığını belirlemek için `vault path-help` ile yardım bölümnden destek alırız. Örneğin, GitHub ta, `map/teams/<team>` adresini kullanarak takıma eşleştirme yaparız:

```shell
$ vault write auth/github/map/teams/default value=secret
Success! Data written to: auth/github/map/teams/default
```

GitHub için varsayılan ekip, hangi takımdan olursa olsun herkesin atandığı varsayılan politikayı kullanır.

Diğer kimlik doğrulama sistemleri, politikaları kimlikle eşlemek için alternatif oluşturur, ancak benzer şekilde çalışır.

### Vault'u Gerçek Ortamda Çalıştırma

Bu noktaya kadar otomatik olarak kimlik doğrulaması yapan, bellek içi depolamayı kuran geliştirici sunucu ile çalıştık. Şimdi Vault'un temellerini bildiğinize göre, Vault'u gerçek ortamda nasıl yapılandırılacağını öğrenmek önemlidir.

Bu sayfada, Vault'u nasıl yapılandıracağınızı, nasıl başlatacağımızı, `seal/unseal` işlemini ve Vault'u nasıl ölçekleyeceğimizi ele alacağız.

#### Vault'un Yapılandırılması

Vault HCL dosyaları kullanılarak yapılandırılmıştır. Bir hatırlatma olarak, JSON dosyaları da tamamen HCL uyumludur; HCL, JSON'un üst kümesidir. Vault'un konfigürasyon dosyası nispeten basittir. Aşağıda bir örnek gösterilmiştir:

```hcl
backend "consul" {
  address = "127.0.0.1:8500"
  path = "vault"
}

listener "tcp" {
 address = "127.0.0.1:8200"
 tls_disable = 1
}
```

Bu yapılandırma dosyasında iki temel yapılandırma vardır:

backend - Vault'un depolama için kullandığı fiziksel gizli veri yönetim servisi. Bu noktaya kadar geliştirici sunucusu "bellekiçi" (bellekte) kullandı, ancak yukarıdaki örnekte gerçek ortam için çok daha uygun bir gizli veri yönetim servisi olan [Consul](https://www.consul.io) kullanıyoruz.

listener - Vault'un API isteklerini dinlemek için bir veya daha fazla dinleyici belirler. Yukarıdaki örnekte, TLS olmadan localhost 8200 numaralı bağlantı noktasını dinliyoruz. Ortam değilkeninizi `VAULT_ADDR=http://127.0.0.1:8200` olarak ayarlayın, böylece Vault istemcisi TLS olmadan bağlanır.

Şimdilik, yukarıdaki yapılandırmayı kopyalayıp example.hcl adlı bir dosyaya yapıştırın. Vault `Consul`'un yerel makinada çalıştığını varsayarak yapılandırılacaktır.

Yerel ortamda Consul'u başlatmak yalnızca birkaç dakika alır. Bu örnekte Consul ile çalışabilmek için [Consul Başlangıç Kılavuzunu](https://www.consul.io/intro/getting-started/install.html) takip etmeniz ve şu komutu kullanarak başlatmanız yeterlidir:

```shell
$ consul agent -server -bootstrap-expect 1 -data-dir /tmp/consul -bind 127.0.0.1
```


#### Sunucuyu Başlatma

Konfigürasyon şartlarının yerine getirilmesiyle, sunucunun başlatılması aşağıda gösterildiği gibi basittir. -config parametresini yukarıdaki yapılandırmayı kaydettiğiniz doğru yolu gösterecek şekilde değiştirin.

```shell
$ vault server -config=example.hcl
==> Vault server configuration:

         Log Level: info
           Backend: consul
        Listener 1: tcp (addr: "127.0.0.1:8200", tls: "disabled")

==> Vault server started! Log data will stream in below:
```

Vault, konfigürasyonu hakkında bazı bilgiler verir ve sonra bloklar. Bu işlem, systemd veya upstart gibi bir kaynak yöneticisi kullanılarak çalıştırılmalıdır.

Hiçbir komut yürütemediğinizi fark edeceksiniz. Herhangi bir yetkilendirme bilgimiz yok! İlk olarak Vault sunucusunu kurduğunuzda, sunucuyu önyükleme ayarları ile birlikte başlatmanız gerekir.

Linux'ta Vault aşağıdaki hatayla başlamayabilir:

```shell
$ vault server -config=example.hcl
Error initializing core: Failed to lock memory: cannot allocate memory

This usually means that the mlock syscall is not available.
Vault uses mlock to prevent memory from being swapped to
disk. This requires root privileges as well as a machine
that supports mlock. Please enable mlock on your system or
disable Vault from using it. To disable Vault from using it,
set the `disable_mlock` configuration option in your configuration
file.
```

Bu konuyla ilgili daha detaylı yönergeler için, Sunucu Yapılandırması'ndaki [disable_mlock tartışmasına](https://www.vaultproject.io/docs/configuration/index.html) bakın.

#### Vault Önyükleme

Önyükleme,Vault'u ilk yapılandırma işlemidir. Bu, Vault tarafından daha önce kullanılmayan yeni bir gizli veri yönetim servisi ile başlatıldığında bir kez olur.

Önyükleme sırasında şifreleme anahtarları ve mühür açıcı (unseal) anahtarlar oluşturulur ve ilk `root` erişim anahtarı (token) üretilir. Vault'u başlatmak için `vault init` komutunu kullanın. Bu kimliği doğrulanmamış bir istektir, ancak yalnızca herhangi bir veri içermeyen yepyeni Vault üzerinde çalışır:

```shell
$ vault init
Key 1: 427cd2c310be3b84fe69372e683a790e01
Key 2: 0e2b8f3555b42a232f7ace6fe0e68eaf02
Key 3: 37837e5559b322d0585a6e411614695403
Key 4: 8dd72fd7d1af254de5f82d1270fd87ab04
Key 5: b47fdeb7dda82dbe92d88d3c860f605005
Initial Root Token: eaf5cc32-b48f-7785-5c94-90b5ce300e9b

Vault initialized with 5 keys and a key threshold of 3!
...
```

Önyükleme, inanılmaz derecede önemli iki bilgiyi bize verir: mühür açıcılar (unseals) ve ilk `root` erişim anahtarı. Bu an, verilerin Vault tarafından bilinen tek anı ve mühürleri gördüğümüz tek zamandır.

Bu başlangıç kılavuzu amacına yönelik olarak, tüm bu anahtarları bir yere kaydedin ve devam edin. Gerçek bir dağıtım senaryosunda, bu anahtarları birlikte asla kaydetmezsiniz. Bunun yerine, muhtemelen Vault'un PGP ve keybase.io desteğini kullanarak bu anahtarların her birini kullanıcıların PGP anahtarlarıyla şifrelemektesiniz. Bu, tek bir kişinin tüm mühür açıcıları almasını önler. Daha fazla bilgi için [PGP, GPG ve Keybase'yi](https://www.vaultproject.io/docs/concepts/pgp-gpg-keybase.html) kullanma ile ilgili dokümanlara bakın.

#### Seal/Unseal - Mühürleme/Mühür Açma

Bütün Vault sunucuları mühürlü (sealed) durumda başlar. Vault yapılandırmadan  fiziksel depolama alanına erişebilir ancak herhangi bir dosyayı okuyamaz, çünkü şifrenizi nasıl çözeceğini bilmemektedir. Verilerin şifresinin nasıl çözüleceğini Vault'a öğretme işlemine, "mühür açmak" (unsealing) olarak adlandırılır.


Vault her başladığında mühür açıcı olmalı. Bu API aracılığıyla ve komut satırı aracılığıyla yapılabilir. Vault'u mühürlemek için, mühür açıcı anahtarlarının eşik sayısına sahip olmalısınız. Yukarıdaki çıktıda, "anahtar eşiğinin" 3 olduğuna dikkat edin. Bu, Vault'un kapanması için üretilen 5 anahtarın 3'üne ihtiyacınız olduğu anlamına gelir.

Not: Vault, açılmamış anahtar parçalarından hiçbirini saklamaz. Vault, ana anahtarın kırıklara bölünmesi için [`Shamir's Secret Sharing`](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing) olarak bilinen bir algoritma kullanıyor. Sadece anahtar eşik sayısı ile yeniden yapılandırabilir ve verilerinize böylece erişebilirsiniz.

Mühür Açma işlemine "vault unseal" ile başlayın:

```shell
$ vault unseal
Key (will be hidden):
Sealed: true
Key Shares: 5
Key Threshold: 3
Unseal Progress: 1
```

Geçerli bir anahtarı doğruladıktan sonra, Vault'un hala mühürlü olduğunu görürsünüz, ancak ilerleme kaydedilir. Vault, 3 anahtarın 1 olduğunu bilir. Algoritmanın niteliğinden dolayı, Vault, eşiğe ulaşılıncaya dek doğru anahtara sahip olup olmadığınızı bilmiyor.

Ayrıca işe yaramayan işlemlerin durum bilgisinin tutulduğuna dikkat edin. Başka bir bilgisayara gidebilir, `vault unseal` komutunu kullanabilirsiniz ve aynı sunucuya işaret ettiği sürece diğer bilgisayar açılış işlemine devam edebilir. Bu, işe yaramayan işlemin tasarımı için inanılmaz derecede önemlidir: Vault'un kapanması için birden çok anahtara sahip birden çok kişiye ihtiyaç vardır. Vault birden fazla bilgisayardan mühürlenebilir ve anahtarlar birlikte olmamalıdır. Tek bir kötü niyetli operatörün kötü amaçlı olması için yeterli sayıda anahtars sahip olmamalıdır.

Vault'un mühürünü açmak için `vault unseal` işlemine devam edin. Vault'u açmak için üç farklı tuş kullanmanız gerekir; aynı tuş tekrarlanmaz. Anahtarları doğru kullandıkça, yakında şu şekilde çıktığını görmelisin:

```shell
$ vault unseal
Key (will be hidden):
Sealed: false
Key Shares: 5
Key Threshold: 3
Unseal Progress: 0
```

`Sealed:false` Vault'un mühürlü olduğu anlamına gelir!

`The Sealed:false` :  Vault'un mühürlü olduğu anlamına gelir!

Mühür açma işlemini anlamak için geçersiz anahtarınların kullanılması gibi farklı denemeler yapmaktan çekinmeyin. Devam etmeye hazır olduğunuzda, `root` token ile kimliğinizi doğrulamak için `vault auth` komutunu kullanın.

`root` yetkilerinde bir kullanıcı ile olarak `vault seal` komutu yardımı ile Vault'u tekrar mühürleyebilirsiniz. `root` yetkilerine sahip Tek bir operatöre bu izin verilir. Bu, tek bir operatörün, acil bir durumda, Vault'un diğer operatörlere danışmadan kilitlemesini sağlar.

Vault tekrar mühürlendiğinde, mevcut durum bilgisi (şifreleme anahtarı da dahil olmak üzere) bellekten temizlenir. Vault güvene alınmış ve erişimden kapatılmıştır.

### HTTP API üzerinde Kimlik Doğrulama

Vault'un tüm yeteneklerine CLI'ye ek olarak HTTP API aracılığıyla da erişilebilir. Aslında, CLI'den gelen çoğu çağrı HTTP API'yi çağırır. Bazı durumlarda, Vault'un özelliklerine CLI üzerinden erişmek mümkün değildir ve yalnızca HTTP API üzerinden erişilebilir.

Vault sunucusunu başlattıktan sonra, API çağrıları yapmak için curl veya başka bir http istemcisini kullanabilirsiniz. Örneğin, Vault sunucusunu geliştirme modunda başlattıysanız, başlatma durumunu şu şekilde doğrulayabilirsiniz:

```shell
$ curl http://127.0.0.1:8200/v1/sys/init
```

Bu istek bir JSON yanıtı döndürür:

```shell
{ "initialized": true }
```

#### Gizli Verilere REST API aracılığı ile Erişmek:

Vault'da saklanan bilgilere erişmek isteyen makineler, muhtemelen Vault aygıtına REST API'sini kullanarak erişebilirler. Örneğin, bir makine kimlik doğrulama için [AppRole](https://www.vaultproject.io/docs/auth/approle.html) kullanıyorsa, uygulama ilk olarak Vault için kimlik doğrulaması yapıp Vault API erişim anahtarı döndürür. Uygulama, bu anahtarı Vault ile gelecekte iletişim kurarken kullanacaktır.

Bu kılavuzun amacı doğrultusunda, TLS'yi devre dışı bırakan, dosya tabanlı gizli veri yönetim servisi kullanan aşağıdaki yapılandırmayı kullanacağız. TLS burada yalnızca örnek amaçlı olarak devre dışı bırakılmıştır ve gerçek ortamda hiçbir zaman devre dışı bırakılmamalıdır.

```shell
backend "file" {
  path = "vault"
}

listener "tcp" {
  tls_disable = 1
}
```

Bu dosyayı diske kaydedin ve vault sunucusunu şu komutu kullanarak çalıştırın:

```shell
$ vault server -config=/etc/vault.conf
```

Bu noktada, tüm etkileşimlerimiz için vault API'yı kullanabiliriz. Örneğin, Vault'u şöyle başlatabiliriz:

```shell
$ curl \
  -X PUT \
  -d "{\"secret_shares\":1, \"secret_threshold\":1}" \
  http://127.0.0.1:8200/v1/sys/init
```

Yanıt aşağıdaki JSON çıktısına benziyor olmalı:

```shell
{
  "root_token": "4f66bdfa-f5e4-209f-096c-6e01d863c145",
  "keys_base64": [
    "FwwsSzMysLgYAvJFrs+q5UfLMKIxC+dDFbP6YzyjzvQ="
  ],
  "keys": [
    "170c2c4b3332b0b81802f245aecfaae547cb30a2310be74315b3fa633ca3cef4"
  ]
}
```

Bu yanıt `root` erişim anahtarımızı içeriyor. Ayrıca mühür açma anahtarı da yanıtla birlikte gelir. Vaultu açmak için mühür açma anahtarı kullanabilir ve `root` erişim anahtarını kullanarak Vault'da kimlik doğrulaması gerektiren diğer işlemleri gerçekleştirebilirsiniz.

Bu kılavuzda anlatılan operasyonları rahatlıkla uygulamanız ve `root` erişim anahtarını depolamak için `$VAULT_TOKEN` çevre değişkeni kullanacağız. Vault erişim anahtarını terminalde şu şekilde dışa aktarabilirsiniz:

```shell
$ export VAULT_TOKEN=4f66bdfa-f5e4-209f-096c-6e01d863c145
```

Mühür açma anahtarını (root erişim anahtarını içermez) kullanarak yukarıdan Vault'u HTTP API aracılığı açabilirsiniz:

```shell
$ curl \
    -X PUT \
    -d '{"key": "FwwsSzMysLgYAvJFrs+q5UfLMKIxC+dDFbP6YzyjzvQ="}' \
    http://127.0.0.1:8200/v1/sys/unseal
```

`FwwsSzM ... `'yi sizin elde ettiğiniz anahtarla değiştirmeniz gerektiğini unutmayın. Bu komut JSON yanıtı döndürür:

```shell
{
  "cluster_id": "1c2523c9-adc2-7f3a-399f-7032da2b9faf",
  "cluster_name": "vault-cluster-9ac82317",
  "version": "0.6.2",
  "progress": 0,
  "n": 1,
  "t": 1,
  "sealed": false
}
```

Artık kimlik doğrulama sistemlerinden herhangi birini etkinleştirilebilir ve yapılandırılabilirsiniz. Bu kılavuzun amacı doğrultusunda [AppRole](https://www.vaultproject.io/docs/auth/approle.html) kimlik doğrulamasını etkinleştireceğiz.

AppRole kimlik doğrulamasını etkinleştirin:

```shell
$ curl -X POST -H "X-Vault-Token:$VAULT_TOKEN" -d '{"type":"approle"}' http://127.0.0.1:8200/v1/sys/auth/approle
```

AppRole etkinleştirmek için yapılan istekte bir kimlik doğrulama erişim anahtarı gerektiğine dikkat edin. Bu örnekte Vault sunucusunu başlattığımızda oluşturulan `root` erişim anahtarını geçiyoruz. Ayrıca, diğer kimlik doğrulama mekanizmalarını kullanarak başka erişim anahtarı üretebiliriz, ancak şimdilik basitlik için `root` erişim anahtarını kullandık.

Şimdi, istenen ACL politikaları kümesine sahip bir istektete bulunun. Aşağıdaki komutta, AppRole `testrole` rolününe verilen erişim anahtarının dev-policy ve test-policy ile ilişkilendirilmesi gerektiği belirtilmektedir.

```shell
$ curl -X POST -H "X-Vault-Token:$VAULT_TOKEN" -d '{"policies":"dev-policy,test-policy"}' http://127.0.0.1:8200/v1/auth/approle/role/testrole
```

Varsayılan yapılandırmasında AppRole kimlik doğrulama sistemi, `role_id` ve `secret_id` adıyla iki adet kimlik bilgisine ihtiyaç duyar. Bu komut, `testrole` rol kimliğini (`role_id`) alır.

```shell
$ curl -X GET -H "X-Vault-Token:$VAULT_TOKEN" http://127.0.0.1:8200/v1/auth/approle/role/testrole/role-id | jq .
```

```shell
{
  "auth": null,
  "warnings": null,
  "wrap_info": null,
  "data": {
    "role_id": "988a9dfd-ea69-4a53-6cb6-9d6b86474bba"
  },
  "lease_duration": 0,
  "renewable": false,
  "lease_id": "",
  "request_id": "ef5c9b3f-e15e-0527-5457-79b4ecfe7b60"
}
```

Bu komut, `testrole`'un altında yeni bir `secret ID` oluşturur.

```shell
$ curl -X POST -H "X-Vault-Token:$VAULT_TOKEN" http://127.0.0.1:8200/v1/auth/approle/role/testrole/secret-id | jq .
```

```shell
{
  "auth": null,
  "warnings": null,
  "wrap_info": null,
  "data": {
    "secret_id_accessor": "45946873-1d96-a9d4-678c-9229f74386a5",
    "secret_id": "37b74931-c4cd-d49a-9246-ccc62d682a25"
  },
  "lease_duration": 0,
  "renewable": false,
  "lease_id": "",
  "request_id": "c98fa1c2-7565-fd45-d9de-0b43c307f2aa"
}
```

Bu iki kimlik bilgisi, yeni bir Vault erişim anahtarı elde etmek için oturum açma adresi (`auth/approle/login`) üzerinde kullanılır.

```shell
$ curl -X POST \
     -d '{"role_id":"988a9dfd-ea69-4a53-6cb6-9d6b86474bba","secret_id":"37b74931-c4cd-d49a-9246-ccc62d682a25"}' \
     http://127.0.0.1:8200/v1/auth/approle/login | jq .
```

```shell
{
  "auth": {
    "renewable": true,
    "lease_duration": 2764800,
    "metadata": {},
    "policies": [
      "default",
      "dev-policy",
      "test-policy"
    ],
    "accessor": "5d7fb475-07cb-4060-c2de-1ca3fcbf0c56",
    "client_token": "98a4c7ab-b1fe-361b-ba0b-e307aacfd587"
  },
  "warnings": null,
  "wrap_info": null,
  "data": null,
  "lease_duration": 0,
  "renewable": false,
  "lease_id": "",
  "request_id": "988fb8db-ce3b-0167-0ac7-1a568b902d75"
}
```

Döndürülen istemci erişim anahtarı (client token) (98a4c7ab-b1fe-361b-ba0b-e307aacfd587) artık Vault ile kimlik doğrulaması yapmak için kullanılabilir. Bu erişim anahtarı, varsayılan, `dev-policy` ve `test-policy` politikaları tarafından kapsanan tüm kaynaklar üzerinde belirli operasyonları gerçekleştirebilir.

Yeni edinilen erişim anahtarı yeni bir `VAULT_TOKEN` olarak dışa aktarılabilir ve Vault isteklerinin kimliğini doğrulamak için bu değişken kullanabilir.

```shell
$ export VAULT_TOKEN="98a4c7ab-b1fe-361b-ba0b-e307aacfd587"
$ curl -X POST -H "X-Vault-Token:$VAULT_TOKEN" -d '{"bar":"baz"}' http://127.0.0.1:8200/v1/secret/foo
```

Bu verilen JSON içeriğiyle "foo" adlı yeni bir gizli veri oluşturacaktır. Bu değeri aynı erşiim anahtarı ile okuyabiliriz:

```shell
$ curl -X GET -H "X-"thesVassult"-Token:$VAULT_TOKEN" http://127.0.0.1:8200/v1/secret/foo | jq .
```

Aşağıdaki gibi bir JSON yanıtı dönecektir:

```shell
{
  "auth": null,
  "warnings": null,
  "wrap_info": null,
  "data": {
    "bar": "baz"
  },
  "lease_duration": 2764800,
  "renewable": false,
  "lease_id": "",
  "request_id": "5e246671-ec05-6fc8-9f93-4fe4512f34ab"
}
```

Diğer kullanılabilir API talep adresleri ile ilgili daha fazla ayrıntı bilgi için [HTTP API' belgelerini](https://www.vaultproject.io/api/index.html) inceleyebilirsiniz.

Tebrikler! Artık Vault ile çalışmak için gereken tüm temel bilgilere sahipsiniz.

Vault için başlangıç kılavuzunun sonuna geildik. Umarım Vault'un yetenekleri seni heyecanlandırmıştır ve buradaki bilgiler süreçlerini iyileştirmek için yeterlidir.

Bu kılavuzda Vault'un tüm özelliklerinin temellerini anlattık. Gizli bilgilerin güvence altına alınmasının öneminden dolayı, aşağıdaki yönergeleri okumanızı öneririz.

[Dokümantasyon](https://www.vaultproject.io/docs/index.html) - Dokümantasyon,Vault'un tüm özelliklerine ilişkin ayrıntılı bir referans kaynağıdır.

## Vault Dokümantasyon

### Vault'u Kurun

Vault kurulumu oldukça basittir. Vault kurulumunda iki yaklaşım vardır:

1. [Önceden Derlenmiş Dosya Kullanma](https://www.vaultproject.io/docs/install/index.html#precompiled-binaries)

2. [Kaynaktan yükleme](https://www.vaultproject.io/docs/install/index.html#compiling-from-source)

Hazır derlenmiş bir dosyayı indirmek en kolayıdır ve dosyayı  TLS üzerinden SHA256 doğrulaması ile indiririz. Doğrulanabilen SHA256 ile birlikte PGP imzası da dağıtıyoruz.

#### Önceden derlenmiş ikili dosyalar

Önceden derlenmiş bir dosyayı yüklemek için, sisteminiz için uygun paketi [indirin.](https://www.vaultproject.io/downloads.html) Vault bir zip dosyası olarak paketlenmiştir. Farklı türde sistem paketleri sağlamak için kısa vadeli bir planımız yok.

Zip indirildikten sonra, herhangi bir dizine unzip edin. vault dosyası (Windows için vault.exe), Vault'u  çalıştırmak için gerekli olan tek şeydir. Diğer dosyalar Vault'u çalıştırmak için gerekli değildir.

Dosyayı sisteminizdeki herhangi bir yere kopyalayın. Komut satırından erişmeyi düşünüyorsanız, PATH ortam değişkenine, bulunduğu dizini eklediğinizden emin olun.

#### Kaynaktan Derleme

Kaynaklardan derlemek için [Git](https://www.git-scm.com/)'in ve [Go](https://golang.org/)'nun yüklü olması ve düzgün bir yapılandırmaya sahip olmanız gerekir. (bir GOPATH ortam değişkeni seti de dahil olmak üzere).

1. Vault deposunu GitHub'dan GOPATH'a kopyalayın:

```shell
$ mkdir -p $GOPATH/src/github.com/hashicorp && cd $!
$ git clone https://github.com/hashicorp/vault.git
$ cd vault
```

2. Projeyi önyükleyin. Bu operasyon, Vault dosyasını derlemek için gerekli kütüphaneleri ve araçları indirecek ardından derleyecektir:

```shell
$ make bootstrap
```

3. Geçerli sisteminiz için Vault dosyasını oluşturun ve dosyayı `./bin/` dizinine yerleştirin. `make dev`  yerel  ortamınız için `vault`'u oluşturan bir kısayoldur (çapraz derleme yapmaz).

```shell
$ make dev
```

#### Kurulumun Doğrulanması

Vault yazılımının doğru şekilde kurulduğunu doğrulamak için sisteminizde `vault -v` komutunu çalıştırın. Yardım çıktısını görmelisiniz. Komut satırından çalıştırıyorsanız, Vault'un bulunduğu adresin PATH'te tanımlı olduğundan emin olun. Aksi taktirde hata alabilirsiniz.

```shell
$ vault -v
```

### Vault'un Derinlikleri

Bu bölüm Vault'u daha derinlemesine ele alır ve Vault'un işlevlerinin, mimarisinin ve güvenlik özelliklerinin teknik ayrıntılarını açıklar.

> Not: Vault'u ile ilgili derinlemesine bilgi edinmek, Vault'u kullanabilmek için gerekli değildir. Vault'un işleyişi hakkında derinlemesine bilgi almak istemiyorsanız bu bölümü güvenle atlayabilirsiniz. Vault'u hali hazırda kullanan deneyimli bir kullanıcıysanız, buradaki detaylı bilgilerden faydalanmanızı öneririz.


#### Mimari

Vault çok farklı parçalara sahip karmaşık bir sistemdir. Bu sayfada , hem kullanıcıların hem de geliştiricilerin Vault'un nasıl çalıştığına ilişkin zihinsel bir model oluşturmasına yardımcı olmak amacıyla sistem mimarisini anlatacağız.

> Zor Bir Konu! Bu sayfa Vault'un teknik ayrıntılarını kapsar. Vault'u etkin bir şekilde kullanmak için bu ayrıntıları anlamanıza gerek yoktur. Detaylar, kaynak kodu üzerinden inceleme yaparak öğrenmek zorunda kalmadan, arkada neler döndüğünü öğrenmek isteyenler için anlatılmıştır. Bununla birlikte, Vault operatörü iseniz, Vault'un öneminden dolayı mimari hakkında bilgi edinmenizi öneririz.

#### Sözlük

Mimariyi tanımlamadan önce, tartışılanları netleştirmek için terimler sözlüğü faydalı olacaktır:

* **Depolama Birimi (Storage Backend)** - Depolama birimi, şifreli verilerin kalıcı olarak depolanmasından sorumludur. Yaklaşım olarak Vault, Depolama Birimlerine güvenilmez ve yalnızca hizmet devamlılığı beklenir. Depolama birimleri Vault sunucusu başlatılırken yapılandırılır.

* **Bariyer (Barrier)** - Bariyer, Vault etrafındaki kriptografik çelik zırhtır. Vault ve Depolama Birimi arasında akan tüm veriler bariyerin içinden geçer. Bariyer, yalnızca şifrelenmiş verilerin yazılmasını sağlar ve veri bu yolla doğrulanır ve çözülür. Vault bir banka gibi, içerideki herhangi birşeye erişmeden bariyer "mühürlenmiş" (unsealed) olmalıdır.

* **Gizli Veri Yönetim Servisi (Secret Backend)** - Gizli veri yönetim servisi, gizli verilerin yönetiminden sorumludur. `generic` gizli veri yönetim servisi gibi basit gizli veri servisleri, sorulduğunda aynı veriyi döndürür. Bazı servisler, sorgulanırlarken her zaman bir Gizli Veri üreten politikaları kullanmayı destekler. Bu, Vault'un kendine has gizli veriler için gelişmiş iptal operasyonları ve politika güncellemeleri yapmasına olanak sağlar. Örnek olarak, bir MySQL hizmeti bir "web" politikasıyla yapılandırılabilir. "Web" politikası üzerinden çalıştığınızda, web sunucusu için sınırlı bir ayrıcalık grubu ile yeni bir MySQL kullanıcı/şifre çifti oluşturulabilirsiniz.

* **Denetim Servisi (Audit Backend)** - Bir denetim servisi denetim günlüklerini yönetmekten sorumludur. Vault'a yapılan her talep ve Vault'dan gelen yanıt, yapılandırılmış denetim servisleri üzerinden geçer. Bu, Vault'u farklı türden birden çok denetim günlüğü ile entegre etmek için basit bir yol sağlar.

* **Kimlik Doğrulama Servisi (Credential Backend)** - Vault'a bağlanan kullanıcıların veya uygulamaların kimlik doğrulamasında Kimlik Doğrulama Servisi kullanılır. Kimlik doğrulaması yapıldıktan sonra servis, uygulanabilir politikaların listesini döndürür. Vault, kimliği doğrulanmış bir kullanıcıyı alır ve gelecekteki istekler için kullanılabilecek bir istemci erişim anahtarı (client token) döndürür. Örnek olarak, `user-password` servisi, kullanıcının kimliğini doğrulamak için bir kullanıcı adı ve parola kullanır. Alternatif olarak, `github` kimlik doğrulama servisi, kullanıcıların GitHub ile kimlik doğrulamasını sağlar.

* **İstemci Erişim Anahtarı (client token)** - Bir istemci erişim anahtarı, kavramsal olarak bir web sitesindendeki oturum çerezine benzemektedir. Bir kullanıcı kimliği doğruladıktan sonra, Vault gelecekteki istekler için kullanılan bir istemci erişim anahtarı döndürür. Anahtar, Vault tarafından istemcinin kimliğini doğrulamak ve uygulanabilir ACL (Erişim kontrol politikası) ilkelerini uygulamak için kullanılır. Bu anahtar, HTTP üstbilgileri yoluyla iletilir.

* **Gizli Veri (secret)** - Gizli veri, Vault tarafından geri gönderilen, gizli veya kriptografik materyal içeren bir terimdir. Vault tarafından döndürülen her şey bir gizli veri değildir. Örneğin sistem yapılandırması, durum bilgisi veya servis ilkeleri gizli veri olarak kabul edilmez. Gizli verilerin her zaman kendileri ile ilişkili bir kullanım süresi içerir. Bu, müşterilerin Gizli Verinin içeriğinin süresiz olarak kullanamayacağı anlamına gelir. Vault, süre bitiminde gizli veriyi iptal eder veya bir operatör, süre bitmeden önce Gizi Veriyi iptal etmek için müdahale edebilir. Anahtarlar ve politikalar üzerinde elle müdahale olmaksızın değişikliklere izin verdiği için Vault ve müşterileri arasındaki bu sözleşme önemlidir.

* **Sunucu** - Vault, sunucu tabanlı bir mimariye sahiptir. Vault sunucusu, müşterilerine; tüm servisler arasındaki etkileşimi, ACL ve gizli veri yönetemi gibi operasyonaları yürütmeye olanak veren bir API sağlar. Sunucu tabanlı bir mimariye sahip olmak müşterileri güvenlik anahtarları ve politikalarından koparır, merkezi denetim günlüğü sağlar ve operatörler için yönetimi kolaylaştırır.

#### Üst Düzey Genel Bakış

Vault hakkında yüksek düzeyde bir genel bakış şöyle görünür:

![Architecture Overview](https://www.vaultproject.io/assets/images/layers-368ccce4.png "Architecture Overview")

Hadi, Bu resimi parçamaya başlayalım. Güvenlik bariyerinin içinde veya dışında bulunan bileşenler birbirinden net bir şekilde ayrılır. Sadece depolama birimi ve HTTP API dışında, tüm diğer bileşenler bariyerin içindedir.

Depolama birimine güvenilmez, bu yüzden sadece şifreli verileri saklamaya elverişlidir. Vault sunucusu başlatıldığında, herhangi bir yeniden başlatma operasyonunda verilerin tekrar  erişilebilir olması için depolama birimini kullanılır. Benzer şekilde, HTTP API'si başlangıçta Vault sunucusu tarafından başlatılmalıdır, böylece müşteriler istemci ile etkileşimde bulunabilir.

Vault ilk başladığına mühürlenmiş (sealed) durumdadır. Vault üzerinde herhangi bir işlem yapılmadan önce mühür açılmalıdır. Bu, mühür açma anahtarlarını kullanılarak yapılır. Vault başlatıldığında, tüm verileri korumak için kullanılan bir şifreleme anahtarı üretir. Bu anahtar bir ana anahtar tarafından korunmaktadır. Varsayılan olarak, Vault, ana anahtarın 5 parçaya bölünmesi için [`Shamir's secret sharing algorithm`](https://en.wikipedia.org/wiki/Shamir's_Secret_Sharing) olarak bilinen bir teknik kullanır; bu 5 parçadan 3'ü ana anahtarın yeniden yapılandırılması için gereklidir.

![Keys](https://www.vaultproject.io/assets/images/keys-468a5903.png "Keys")

Parça sayısı ve minimum eşik değeri kullanıcı tarafından belirtilebilir. Shamir'in tekniği devre dışı bırakılabilir ve ana anahtar, mühür açma için doğrudan kullanılır. Vault şifreleme anahtarını alır almaz, depolama birimindeki verilerin şifresini çözebilir ve mühürlüsüz duruma geçer. Mühürsüz durumda, Vault, yapılandırılmış tüm denetim, kimlik bilgisi ve gizli veri yönetim servislerini yükler.

Bu hizmetlerin konfigürasyonu, güvenlik açısından hassas olduklarından Valut'da saklanmalıdır. Yalnızca doğru izinlere sahip kullanıcılar bunları değiştirebilmelidir, yani bariyerin dışında tanımlanamazlar. Onları Vault'da depolamak, ACL'deki tüm değişikliklerin ACL sistemi tarafından korunmasını ve denetim günlükleri tarafından izlenebilmesini sağlar.

Vault'un mührünün açılmasından sonra, talepler HTTP API üzerinden Sisteme gönderilebilir. Sistem, isteklerin akışını yönetmek, ACL'leri uygulamak ve denetim günlüğünün yapılmasını sağlamak için kullanılır.

Bir müşteri ilk önce Vaulta bağlandığında, kimlik doğrulaması yapması gerekir. Vault , kullanılan kimlik doğrulama mekanizmasında esneklik sağlayan kimlik bilgisi servislerini kullanır. `username/password` veya `GitHub` gibi insancıl mekanizmalar operatörler için kullanılabilirken, uygulamalarda kimliği doğrulamak için `public/private` anahtarları veya erişim anahtarları kullanabilir. Bir kimlik doğrulama isteği, sistem aracılığıyla, isteğin geçerli olup olmadığını belirleyen ilişkili poliçelerin listesini kimlik doğrulama servisine aktarır.

Politikalar sadece adlandırılmış bir ACL kuralıdır. Örneğin, "root" politikası yerleşik olarak bulunur ve tüm kaynaklara erişime izin verir. Sistem üzerinde gelişmiş denetime sahip istediğiniz sayıda politika oluşturabilirsiniz. Vault sadece beyaz liste modunda çalışıyor, yani bir politika yoluyla erişim açıkça verilmediği sürece bu işleme izin verilmiyor. Bir kullanıcının ilişkili birden çok politikası olabileceğinden, herhangi bir politika ona izin veriyorsa bir işleme izin verilir. Politikalar, bir iç politika deposu tarafından saklanır ve yönetilir. Bu dahili birim, her zaman `sys/` adresine tekabül eden sistem hizmetleriyle yürütülür.

Kimlik doğrulaması gerçekleştiğinde ve Kimlik Doğrulama Servisi uygulanabilir bir dizi politika sağladığında, yeni bir müşteri erişim anahtarı, erişim anahtarı birimi tarafından oluşturulur ve yönetilir. Bu istemci erişim anahtarı, müşteriye geri gönderilir ve gelecekte istekte bulunmak için kullanılır. Bu, bir kullanıcı oturum açtıktan sonra bir web sitesi tarafından gönderilen bir çereze benzer. İstemci erişim anahtarı, Kimlik Doğrulama Servisi yapılandırmasına bağlı olarak onunla ilişkilendirilmiş bir kullanım süresine sahip olabilir. Bu durum, erişimi geçersiz kılmayı önlemek için istemci erişim anahtarının periyodik olarak yenilenmesi gerekebileceği anlamına gelir.

Kimlik doğrulaması gerçekleştikten sonra istemci erişim anahtarı sağlayan istekler yapılır. Erişim anahtarı, müşterinin yetkili olduğunu doğrulamak ve ilgili politikaları yüklemek için kullanılır. İlkeler, istemci isteğini yetkilendirmek için kullanılır. Ardından, talep, servis türüne bağlı olarak işlenen Gizli Veri Yönetim Servislerine yönlendirilir. Servis bir gizli veri döndürürse, sistem onu son kullanma yöneticisine kaydeder ve bir kira kimliği (lease_id) ekler. Kira kimliği, müşterileri tarafından gizli veriyi yenilemek veya iptal etmek için kullanır. Bir müşteri kira kimliğinin geçerliliğini yitirmesine izin verirse, Vault gizli veriyi otomatik olarak iptal eder.

Sistem, taleplerin günlüğe kaydedilmesini ve denetçinin verdiği yanıtları değerlendirir ve yapılandırılmış denetim servislerine yönlendirir. İstek akışı dışında, sistem belirli görevleri gerçekleştirir. Kiralama yönetimi kritik önem taşır çünkü zaman aşımına uğramış müşteri erişim anahtarıları ya da gizli verilerin otomatik olarak iptal edilmesine olanak sağlar. Buna ek olarak, Vault kısmi hata durumlarını, geri alma (rollback manager) yöneticisi ile günlüğe  işler (kayıt altına alır). Bu günlük sistem içinde şeffaf bir şekilde yönetilir ve günlük kullanıcı tarafından görüntülenemez.

#### Derinlemesine Bakış

Az önce Vault mimarisine kısa bir genel bakış yaptık. Sistemlerin her biri için daha ayrıntılı konuşabiliriz.

Diğer ayrıntılar için, kaynak kodu inceleyebilir, IRC'ye sorabilir veya posta listesine ulaşabilirsiniz.

