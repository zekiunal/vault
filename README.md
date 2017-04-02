# Vault

Vault dökümantasyonuna hoş geldiniz! Bu dokümantasyon, Vault'un tüm özellikleri ve yetenekleri için bir referans kaynağıdır. Vault ile bilgi almaya, [giriş](https://www.vaultproject.io/intro/index.html) bölümüyle başlayın ve [Başlarken kılavuzuna](https://www.vaultproject.io/intro/getting-started/install.html) kadar devam edin.


## Vault Başlangıç Kılavuzu

Bu kılavuz, Vault ile başlamak için en iyi yerdir. Vault'un ne olduğunu, hangi problemleri çözebileceğini ve nasıl hızla kullanmaya başlayacağınızı anlatmaktadır.

### Vault nedir?

Vault, gizli kalması gereken verilere(secrets) güvenli bir şekilde erişmek için kullanılan bir araçtır. Bu veri, API anahtarları, şifreler, sertifikalar vb. erişimini sıkı bir şekilde kontrol etmek istediğiniz herhangi bir şeydir. Vault, sıkı erişim kontrolü sağlamak ve detaylı bir denetim günlüğü kaydetmek için bu gizli veri ile bütünleşik bir arabirim sağlar.

Modern bir sistem gizli tutulması gereken bir çok veriyi içerir (secrets): Veritabanı kimlik bilgileri, harici hizmetler için API anahtarları, hizmet odaklı mimari iletişimi için kimlik bilgileri gibi gizli bilgilere kimlerin eriştiğini platform spesifik olarak testpit etmek oldukça zordur. Rol tabanlı koruma, güvenli depolama, detaylı denetim günlüklerini yönetme gibi operesyonlar özel bir çözüm olmadan neredeyse imkansızdır. İşte Vault burada devreye girmektedir.  

Vault un temel özellikleri şunlardır:

#### Gizli Bilgileri Güvenli Şekilde Depolama: 
Gizli bilgiler anahtar/değer (key/value) olarak Vaultta saklanabilir. Vault, bu gizli bilgileri kalıcı depolamaya yazmadan önce şifreler; bu nedenle, Gizli verilere erişmek için ham depolamaya erişmek yeterli değildir. Vault verileri diske yazabildiği gibi, depolama için Consul gibi araçlardan da faydalanabilir.

#### Değişiklik Arz Eden Gizli Veriler: 
Vault, AWS veya SQL veritabanları gibi bazı sistemler için gizli verileri talep üzerine üretilebilir. Örneğin, uygulamanın bir S3 alanına erişmesi gerektiğinde Vaulta kimlik bilgilerini sorar ve Vault talep üzerine geçerli izinlere sahip bir AWS erişim anahtarı oluşturur. Vault Gizli bilgileri oluşturulduktan sonra,  gerektiğinde (kullanım süresi dolduğunda) otomatik olarak iptal işlemini de gerçekleştirir.

#### Veri Şifreleme: 
Vault, verileri şifreleyebilir ve şifresini çözebilir. Bu, güvenlik ekiplerinin ve geliştiricelerin şifreleme operasyonlarını kendi şifreleme yöntemlerini tasarlamak zorunda kalmadan SQL gibi bir konuma depolamalarını sağlar.

#### Kullanım Süresi ve Yenileme: 
Vault üzerinde barıdırılan her gizli bilginin kullanım süresi vardır. Bu sürenin sonunda, Vault gizli bilgileri otomatik olarak iptal edecektir. Kullanıcılar, yerleşik yenileme API'leri aracılığı ile bu süreyi uzatabilir veya bu gizli erişim bilgilerini yenileyebilir.

#### İptal: 
Vault, gizli bilgilerin iptalini yerleşik olarak destekliyor. Vault sadece bir tek gizli veriyi değil, aynı zamanda bir gizli veri grubunu iptal edebilir, örneğin tüm Gizli veriler belirli bir kullanıcı tarafından okunabilir veya belirli bir türdeki gizli veriler bir bir grup olarak okunabilir. Bu açıdan, iptal işlevi, sistemi kitlemede önemli rol oynamanın yanı sıra bir saldırı durumunda sistemleri güvence altına almaya da yardımcı olur.


### Kullanım Örnekleri

Vault'un kullanım alanlarını anlamadan önce, ne olduğunu bilmek faydalıdır. Bu sayfada Vault için bazı somut kullanım örnekleri listelenmiştir, ancak muhtemel kullanım örnekleri, burada anlatılanlardan çok daha fazladır.

#### Genel Depolama Alanı

En azından Vault, gizli bilgilerin depolanması için kullanılabilir. Örneğin, Vault, hassas çevre değişkenleri, veritabanı kimlik bilgileri, API anahtarları vb. verileri depolamak için mükemmel bir araçtır.

Bunu dosyalarda, yapılandırma yönetiminde, veritabanında vb. düz metin olarak depolama yöntemleri ile karşılaştırıldığında,  Vault üzeriden okuma veya API'yi kullanarak bu verileri sorgulamak çok daha güvenli olacaktır. Ayrıca bu verilerin düz metin sürümünü ve kayıt defteri erişimini Vault denetim günlüğünde saklanmaktadır.

#### Çalışan Kimlik Bilgileri Depolama Alanı

"Çalışan Kimlik Bilgileri Depolama Alanı", "Genel Depolama Alanı" ile benzer özellikler barındırır.  Vault, çalışanların web hizmetlerine erişmek için paylaştıkları kimlik bilgilerini depolamak için iyi bir mekanizmadır. Denetim günlük mekanizması, çalışanların eriştikleri verilerin ne olduğunu ve bir çalışan ayrıldığında ne yaptığını bilmenizi sağlar ve hangi verilerin devreden çıkarılacağını anlamak daha kolaydır.

#### API Anahtar Üretimi

##### Vault yazılımının "dinamik" veri "özelliği kodlama için idealdir: 

Örneğin, Bir komut dizisinin çalışma süresi için, AWS erişim anahtarı oluşturulabilir ve daha sonra iptal edilir. Erişim anahtarları, komut dosyası çalıştırılmadan önce veya sonrasında var olmayacaktır, ancak anahtarların oluşturulması tamamen kayıt altına alınacak, log dosyalarına işlenecektir.

Bu özellik, Amazon IAM gibi bir şey kullanmanız söz konusu olduğunda; sınırlı erişim belgelerinin etkili bir şekilde oluşturmak ve kodunuz üzerinden kullanabilmenizi sağlar.

#### Veri şifreleme

Gizli bilgilerin depolanabilmesinin yanı sıra, Vault başka yerlerde depolanan verileri şifrelemek ve şifrelerin çözülmesi için kullanılabilir. Bunun birincil kullanımı, uygulamaların birincil veri deposunda sakladığı halde uygulamaların verilerini şifrelemesine izin vermektir.

Bunun faydası, geliştiricilerin verileri doğru şekilde nasıl şifreleme konusunda endişelenmeleri gerekmemesidir. Şifreleme sorumluluğu Vaultadır ve geliştiriciler sadece verileri gerektiği gibi şifrelemekte / şifresini çözmektedir.

### Kullanım

#### İlk Adım

Önce makinenize Vault kurulmalıdır. Vault, tüm desteklenen platformlar ve mimariler için bir [ikili paket olarak](https://www.vaultproject.io/downloads.html) dağıtılır. Bu sayfada Vault kaynağından nasıl derleneceği anlatılmıyor, ancak ikili dosyanın en son kaynak kodundan derendiğinden emin olmak isteyenleri [bu dokümana](https://www.vaultproject.io/docs/install/index.html) göz atabilir.

#### Kurulum

Vault yazılımını yüklemek için, sisteminiz için [uygun paketi bulun](https://www.vaultproject.io/downloads.html) ve indirin. Vault bir zip dosyası olarak paketlenmiştir.

Vault dosyasını indirdikten sonra paketi açın. Vault, `vault` adlı tek bir dosya olarak çalışır. Pakette bulunan diğer dosyalar da güvenle silinebilir ve `vault`'un çalışmasını etkilemez.

Son adım, PATH ortam değişkeninde `vault` dosyasının mevcut olduğundan emin olmaktır. Linux ve Mac'te PATH ayarlama ile ilgili talimatlar için [bu sayfaya](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux) bakın. [Bu sayfada](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) da PATH'ı Windows'ta ayarlama yönergeleri bulunmaktadır.

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

Vault kurulu olduğunda bir sonraki adım Vault sunucusunu başlatmaktır.

Vault istemci/sunucu uygulaması olarak çalışır. Vault sunucusu, veri deposu ve arka uçlarla etkileşim kuran Vault mimarisinin tek parçasını oluşturur. Vault CLI aracılığıyla yapılan tüm işlemler TLS bağlantısı üzerinden sunucu ile etkileşim kurar.

Bu sayfada, sunucunun nasıl başlatıldığını anlamak için Vault sunucusunu başlatacak ve etkileşimde bulunacağız.

#### Geliştirici Özellikleri İle Sunucuyu Başlatma

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

Yukarıdaki gibi bir çıktı görmelisiniz. Vault ön planda çalışmaya devam edecek; Daha sonra, farklı komutlar uygulamak için yeni bir terminal  açın.

Gördüğünüz gibi, bir geliştirici sunucusu başlattığınızda, Vault sizi yüksek sesle uyarır. Geliştirici sunucusu tüm verilerini belleğe kaydeder (ancak yine de şifrelenmiş), localhost üzerinde TLS dinler ve otomatik olarak açar ve siz açılış anahtarı ve kök erişim anahtarını gösterir. Bütün bunların ne anlama geldiğini birazdan gözden geçireceğiz.

Geliştirici sunucusu ile ilgili önemli nokta, yalnızca geliştirme amaçlı olmasıdır.

> Not: Geliştirici sunucusunu gerçek bir ortamda (production) çalıştırmayın.

Geliştirici sunucusu gerçek bir ortamda (production) çalıştırıldığında, verileri bellekte depolar ve her yeniden başlatma tüm gizli verilerinizi sileceğinden bunun size pek yararı olmaz.

Geliştirici sunucusunu çalışırken:

1. Yeni bir terminal oturumu açın.

2. `export VAULT_ADDR='....` komutunu terminal çıktısından kopyalayın ve çalıştırın. Vault istemcisini, geliştirici sunucumuzla konuşacak şekilde yapılandıracaktır.

3. Anahtarı bir yere kaydedin. Bunu nasıl güvenli bir şekilde saklayacağınız konusunda endişelenmeyin. Şimdilik, herhangi bir yere kaydedin.

4. Adım 3 ile aynı işlemi yapın, ancak `root` anahtarı ile yapın. Bunu daha sonra kullanacağız.

#### Sunucunun Çalıştığını Doğrulayın

`vault status` komutunu çalıştırarak sunucunun çalıştığını doğrulayın. Bu başarılı olur ve çıkış kodu 0 ile çıkar. Bir bağlantı açma konusunda bir hata görürseniz, yukarıdaki`export VAULT_ADDR='....` komutunu düzgün bir şekilde yürüttüğünüzden emin olun.

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

Tebrik ederiz! İlk TheVault sunucusunu başlattınız. Henüz herhangi bir gizli veri(secret) depolamadık, ancak bunu bir sonraki bölümde yapacağız.

### İlk Gizli Veri

Şimdi geliştirci sunucumuz ayakta ve çalışıyor. İlk gizli verimizi yazıp okuyabiliriz.

Vault'un temel özelliklerinden birisi gizli verilermizi güvenli bir şekilde okuma ve yazma yeteneğidir. Bu sayfada, CLI kullanarak bunu yapacağız, ancak Vault'un yeteneklerinden faydalanabileciğimiz eksiksiz bir HTTP API'si olduğunu bilmekte fayda var.

Vault'a yazılan gizli bilgiler önce şifrelenir daha sonra depolama alanına yazılır. Geliştirici sunucusunda, depolama alanı bellektir, ancak gerçek ortamda büyük olasılıkla disk veya Consul olacaktı. Vault veriyi depolama sürücüsüne teslim edilmeden önce şifreler. Depolama mekanizması şifrelenmemiş veriyi görmez ve Vault olmadan şifresini çözmek için gerekli araçlara sahip değildir.

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

Çıktı biçimi, `awk` gibi bir araç ile düzenlenmiş.

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

Bu format, bazı ek bilgiler içermektedir. Birçok depolama birimi (backend), Gizli bilgiler için zamanaşımıyla diğer sistemlerin erişimini kısıtlayan sağlayan kullanım süresini destekler. Bu durumlarda lease_id bir kullanım süresi içerecek ve lease_duration saniye bazında kullanımının geçerli olduğu süreyi içerecektir.

Verilerimizin detaylı bir dökümünü görüyoruz. The JSON output is very useful for scripts. For example below we use the jq tool to extract the value of the excited "theSecret":

Verilerimizi burada detaylı bir dökümünü görüyoruz. JSON çıktısı komut dosyaları için çok yararlıdır. Örneğin aşağıdaki örnekte `excited` Gizli verisinin değerini elde etmek için `jq` aracını kullanıyoruz:

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

### Depolama Birimleri

Daha önce, gizli verileri Valut'a nasıl yazacağımızı ve okuyacağımızı gördük. Bunu yapmak için, `secret/` önekini kullandık. Bu önek hangi depolama biriminin kullanılacağını belirtir. Varsayılan olarak Vault, `generic` adlı bir depolama birimini `secret`'a bağlar. `generic` depolama birimi, ham veriyi disk üzerinden okur ve yazar.

Vault `generic depolama birimine` ek olarak farklı depolama birimlerini de desteklemektedir ve bu özellik Vault'u benzersiz kılan özelliktir. Örneğin, AWS depolama birimi, isteğe bağlı olarak AWS erişim anahtarlarını dinamik olarak üretir. Başka bir örnek - bu tür bir depolama birimi henüz mevcut değil - doğrudan bir [HSM](https://en.wikipedia.org/wiki/Hardware_security_module)'ye veri yazar ve okur. Vault olgunlaştıkça giderek daha fazla depolama birimi eklenecektir.

Vault Depolama Birimlerini Dosya Sistemine Çok Benzer Şekilde Kullanır: 

Depolama birimleri belirli yollar (path) yardımı ile tanımlanır. Örneğin `generic depolama birimi` `secret/` önekini alarak tanımlanır.

Bu sayfada, depolama birimlerinin tanımlanmasını ve depolama birimleri ile gerçekleştirilebilecek işlemler hakkında bilgi edineceğiz. İlerleyen bölümlerde dinamik olarak gizli veri oluşturacağımız işlemlerde buradaki bilgilerden faydalanacağız.

#### Depolama Birimi Tanımlama

İlk başta, başka bir `generic`  depolama birimi elde edelim. Normal bir dosya sistemi gibi Vault da birden fazla depolama birimi tanımlanabilir. Farklı erişim denetimi ilkeleri (covered later) veya farklı yollar için yapılandırmalar istiyorsanız bu özellik işinize yarayacaktır.

Depolama Birimi Tanımlama:

```shell
$ vault mount generic
Successfully mounted 'generic' at 'generic'!
```

Varsayılan olarak, depomalama tanımı ile depolama birimi aynı isimde olacaktır. Bunun nedeni, zamanın %99'unu depolama tanımını özelleştirmek için harcamamamızdır. Bu örnekte, `generic` depolama birimini `generic/` adresine kurduk.

Depolama Tanımlarını `vault mounts` ile inceleyebilirsiniz: 

```shell
$ vault mounts
Path      Type     Description
generic/  generic
secret/   generic  generic secret storage
sys/      system   system endpoints used for control, policy and debugging
```

Görüldüğü gibi `generic/` depolama tanımının yanı sıra `secret/` ve `sys/` yolunu da içermektedir. Bu konuya şimdilik değinmeyeceğiz. Bilgi vermek açısından `sys/` bağlantı noktası Vault ile iletişime geçmek için kullanılır.

Herşeyin yolunda olduğundan emin olmak için bazı gizli verileri yeni depolama birimine yazın ve okuyun. İlk olarak `secret/` erişim noktasına yazın ve `generic/` yolu ile bu değerleri okuyamadığınızı göreceksiniz: Aynı depolama birimini paylaşmalarına rağmen, hiçbir gizli veriyi paylaşmıyorlar. Buna ek olarak, (aynı türden veya farklı türden) depolama birimleri de diğer depolama birimlerinin verilerine erişemez; Yalnızca bağlama noktası/depolama tanımı içinde verilere erişebilirler.

#### Depolama Birimini Kaldırma

Bir depolama birimi kaldırıldığında, bütün gizli veriler iptal edilir ve silinir. Bu işlemlerden herhangi biri başarısız olursa, depolama birimi kaldırma işlemi iptal edilir.

```shell
$ vault unmount generic/
Successfully unmounted 'generic/' if it was mounted
```

Bir depolama birimini kaldırdığınızda, tekrar eklemeniz mümkündür. Depolama birimini tekrar ekleme, depolama tanımının/bağlantı noktasını değiştirir.  Bu operasyonda yıkıcıdır. Saklanan veriler korunsa da gizli veriler `secret/` yoluyla bağlantılı olduğu için iptal edilmiştir. 

#### Depolama Birimi Nedir?

Artık bir depolama birimini ekdiğinize ve çıkardığınıza göre: Depolama Birimi nedir ve bu depolama birimi tanımlama sisteminin anlamı nedir?

Vault [sanal bir dosya sistemi](https://en.wikipedia.org/wiki/Virtual_file_system) gibi çalışır. Okuma, yazma ve silme işlemleri depolama birimine yönlendirilir ve depolama birimi bu işlemleri gerçekleştirir. Örneğin `generic` depolama birimi, basit bir şekilde verileri depolama alanına iletir. (şifreledikten sonra)

Bununla birlikte `AWS Depolama Birimi` (yakında göreceğiz), IAM ilkelerini okur, yazar ve erişim anahtarına erişebilir. Yani bu işlemleri `vault read aws/deploy` ile gerçekleştirken, okuma `aws/deploy` fiziksel yolu üzerinde gerçekleşmez. Buun yerine AWS Depolama birimi dağıtım ilkesine uygun bir erişim anahtarı üretir.

Bu soyutlama inanılmaz güçlüdür. Vault arayüzü fiziksel sistemler ile doğrudan bağlantı kurabilmenin yanı sıra SQL veritabanları, HSM'ler gibi sistemleride arayüze bağlar. Fakat bu fiziksel sistemlere ek olarak Vault, daha eşsiz ortamlarla etkileşim kurabilir: AWS IAM, dinamik SQL Kullanıcı yaratma vb hepsi aynı okuma/yazma arabirimini kullanmaktadır.

### Gizli Bilgi Üretme - Dinamik Gizli Veri

Vault'a gizli verilerimizi yazdık ve depolama birimi tanımlama gibi özelikleri anladık. Şimdi, Vault'un bir sonraki temel özelliği olan: Gizli veri üretme operesyonlarına geçeceğiz.

Gizli bilgi üretme yani dinamik gizli veriler, erişildikleri zaman üretilen gizli verilerdir ve `İlk Gizli Veri` bölümün deki gibi el yordamıyla yazılmamıştır.  Bu sayfada AWS erişim anahtarlarını dinamik olarak oluşturmak için yerleşik AWS depolama birimini kullanacağız.

Dinamik Gizli Verinin gücü, sadece okunmadan önce var olmamalarıdır; bu nedenle, birinin bu gizli veriyi veya başka bir müşteri verisini çalma riski yoktur. Vault yerleşik iptal mekanizmalarına (daha sonra kapsanır) sahip olduğundan, dinamik gizli veri kullanımdan hemen sonra iptal edilebilir ve gizli verilerin var olduğu süreyi en aza indirebilir.

> Not: Bu sayfayı başlatmadan önce, lütfen bir AWS hesabı için [kayıt](https://aws.amazon.com/) olunuz. Maliyete neden olan hiçbir özelliği kullanmayacağız, bu nedenle herhangi bir şey için ücret ödememelisiniz. Bununla birlikte, doğabilecek herhangi bir masrafdan biz sorumlu değiliz.

#### AWS Depolama Birimini Tanımlama

İlk dinamik gizli verimizi üretelim. Dinamik olarak AWS erişim anahtarı çifti oluşturmak için AWS depolama birimini kullanacağız. İlk olarak, AWS depolama birimini tanımlayın:


```shell
$ vault mount aws
Successfully mounted 'aws' at 'aws'!
```

AWS depolama birimi `aws/` adresine monte edildi. Bir önceki bölümde değindiğimiz gibi, farklı gizli veri depolama birimleri  farklı davranışlar sergiler ve bu örnekte AWS depolama birimi, AWS erişim kimlik bilgilerini oluşturmak için dinamik bir arayüz oluşturur.

#### AWS Depolama Birimini Yapılandırma

AWS depolama birimi tanımlandığında, ilk adım, onu diğer kimlik bilgilerini oluşturmak için kullanılacak AWS kimlik bilgileri ile yapılandırmaktır. Şimdilik, AWS hesabınız için `root` anahtarlarını kullanın.

Depolama birimini yapılandırmak için, `vault write` komutunu `aws/config/root` yolu üzerinde kullanacağız:

```shell
$ vault write aws/config/root \
    access_key=AKIAI4SGLQPBX6CSENIQ \
    secret_key=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
Success! Data written to: aws/config/root
```

Dinamik veri depolama birimlerinin sadece okurken/yazarken veriyi oluşturduğunu unutmayın, tanımlı olan bu yoldaki yapılandırmayı saklayacaktır ancak daha sonra okunamıyacaktır:

```shell
$ vault read aws/config/root
Error reading aws/config/root: Error making API request.

URL: GET http://127.0.0.1:8200/v1/aws/config/root
Code: 405. Errors:

* unsupported operation
```

Kimlik bilgilerini güvenli tutmaya yardımcı olmak için AWS depolama birimi, kimlik bilgilerini `root` yetkisi kullansanız bile onları okumanıza izin vermez.

#### Rol Yaratmak

Bir sonraki adım AWS depolama birimini IAM ilkesiyle yapılandırmaktır. IAM, sınırlı API izinlerine sahip yeni kimlik bilgileri oluşturmak için AWS'nin kullandığı sistemdir.

AWS depolama birimi, oluşturulan kimlik bilgilerini ilişkilendirmek için IAM ilkesini gerektirir. Bu örnek için yalnızca bir politika yazacağız, ancak birçok politikayı arka uç ile ilişkilendirebilirsiniz. Aşağıdaki içeriğe sahip `policy.json` adlı bir dosya oluşturun:

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

AWS depolama birimini yapılandırdık ve bir rol oluşturduk, şimdi bu rol için bir erişim anahtarı çifti talep edebiliyoruz. Bunu yapmak için, `aws/creds/<ADI>` özel yolunu okuyun, burada ADI rolün adıdır:

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

Şu ana kadar `vault write` ve `vault read` okuma/yazma pratikleri üzerine çalıştık: `secret/` yolu ile generic depolama birimini  ve `aws/` yolu ile AWS depolama birimi üzerinden dinamik AWS kimlik bilgileri oluşturduk. Her iki durumda da, her depolama biriminin yapısı ve kullanımı farklılıklar gösterdi; örneğin AWS depolama birimi, `aws/config` gibi özel yollara sahip olduğunu gördük.

Hangi yolları kullanacağınızı belirlemek için sürekli ezberlemek veya belgelere referans vermek zorunda kalmadan, doğrudan Vault ile çalışan bir yardım sistemi kurduk. Bu yardım sistemine API veya komut satırı üzerinden erişilebilir ve tanımlanmış herhangi bir depolama birimi için okunaklı bir yardım üretir.

Bu sayfada, bu yardım sistemini nasıl kullanacağınızı öğreneceğiz. Vault'u kullanırken çok değerli bir araçtır.

#### Depolama Birimlerine Genel Bakış

Bunun için AWS depolama biriminin takılı olduğunu varsayacağız. Değilse, `vault mount aws` ile bağlayın. Bir AWS hesabınız olmasa bile yine de AWS depolama birimine bağlayabilirsiniz.

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

`vault path-help` komutu olası yolları listeler.  Bir depolama birimi için temel adresini belirterek, bu depolama biriminin genel özelliklerini bize listeler. Yardımın sadece bir açıklama içerdiğini değil, aynı zamanda bu depolama birimi için güzergâhları eşleştirmek için kullanılan tam düzenli ifadelerin, güzergahın neyle ilgili olduğunu da söylemektedir.

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

Daha fazla yol keşfedebilirsiniz! Diğer depolama birimlerini kurun, yardım sistemlerini dolaşın ve yaptıklarını öğrenin. Örneğin,  `thesecret/` yolu ile `generic` depolama birimi hakkında bilgi edinin.

### Kimlik Doğrulama

Artık Vault'un temellerini ve nasıl kullanacağımızı bildiğimizden Vault'un kendisinin kimliğinin nasıl doğrulanacağını anlamak önemlidir. Bu noktaya kadar kimlik doğrulaması yapmamız gerekmiyordu çünkü geliştirici modunda Vault sunucusunun başlatılması bizi otomatik olarak `root` kullanıcısı olarak kaydediyordu. Gerçek kullanımda, neredeyse her zaman el yordamı ile kimliğinizi doğrulamanız gerekir.

Bu sayfada, kimlik doğrulama hakkında özel olarak konuşacağız. Bir sonraki sayfada yetkilendirme hakkında konuşacağız. Kimlik doğrulama, Vault kullanıcısına bir kimlik tahsis etme mekanizmasıdır. Bir kimlikle ilişkili erişim kontrolü, izinleri ve yetkileri bu sayfada ele alınmayacaktır.

Vault, kurulabilir kimlik doğrulama sistemlerine sahiptir ve kuruluşunuz için en iyi sistemi kullanarak Vault ile kimlik doğrulamasının etkisi artırlır. Bu sayfada, token backend'in yanı sıra GitHub sistemlerini kullanacağız.

#### Tokens

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

### Kimilik Doğrulama Sistemleri

Token kimlik doğrulama sistemine ek olarak, başka kimlik doğrulama veya yetkili sistemleri etkinleştirilebilir.  Vault ile tanımlanan Kimilik Doğrulama Sistemleri ile bazı alternatif yöntemlere sahip oluruz. Bu kimlikler, Token kimlik doğrulama sistemi gibi bir dizi erişim politikasına bağlıdır. Örneğin masaüstü ortamları için özel anahtar veya GitHub tabanlı kimlik doğrulama kullanılabilir. Sunucu ortamları için, paylaşılan gizli veriler en iyisi seçenek olabilir. Kimilik Doğrulama Sistemleri, bize farklı kimlik doğrulama yöntemleri arasında seçme esnekliği sağlar.

Örnek olarak, GitHub'ı kullanarak kimlik doğrulamasını yapalım. Önce, GitHub Kimilik Doğrulama Sistemini etkinleştirin:

```shell
$ vault auth-enable github
Successfully enabled 'github' at 'github'!
```

Kimilik Doğrulama Sistemleri, gizli veri depolama birimleri gibi tanıtılır; temel fark, Kimilik Doğrulama Sistemleri  her zaman `auth/` öneki alır. Bu yüzden, daha önce kurduğumuz GitHub Kimilik Doğrulama Sistemine `auth/github` adresinden erişilebilir. Bunun hakkında daha fazla bilgi edinmek için `vault path-help` yardımını kullanabilirsiniz.

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

## Erişim Kontrol Politikaları (ACLs)

"Vault" daki erişim kontrol politikaları, bir kullanıcının erişim haklarını kontrol eder. Son bölümde kimlik doğrulamayı öğrendik. Bu bölüm yetkilendirme ile ilgilidir.

Vault Kimlik doğrulama için  etkinleştirilebilen ve kullanılabilen birden çok seçenek veya sistem içerir. Yetki ve erişim kontrolü politikaları için Vault daima aynı biçimi kullanır. Tüm kimlik doğrulama sistemleri, kimlikleri "theVault" ile yapılandırılan temel politikaları ile eşleştirmelidir.

Vault'u başlatıldığında, kaldırılamayan , önceden oluşturulmuş özel bir politikaya sahiptir: `root` erişim politikası. Bu politika Vault'taki her şeye süper kullanıcı erişimi sağlayan özel bir politikadır. `root` politikasıyla eşlenen bir kimlik ile her şeyi yapabilirsiniz.

### Politika Formatı

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

Politika formatı, erişim denetimini belirlemek için API adresinde bir önek eşleştirme sistemi kullanır. Kesin tanımlanmış politika, tam eşleşme veya en uzun ön ek eşlemesi olabilir. Vault'daki herşeye API aracılığıyla erişilmesi gerektiğinden, Vault'un bütün özellikleri üzerinde, depolama birimlerinin montajı, kimlik doğrulaması ve gizli bilgilere erişimi de dahil olmak üzere tüm yönleriyle sıkı bir denetim sağlar.

Yukarıdaki politikaya sahip bir kullanıcı, `secret/` yoluna herhangi bir gizli veri yazabilir, ancak sadece salt okunur `secret/foo`  verisini okuma erişimine sahiptir. Politikalar varsayılan olarak red eğilimdedir; dolayısıyla belirtilmemiş bir yola erişim mümkün değildir. Politika söz dizimi Vault 0.2 versiyonunda  biraz değişti, ayrıntılar için [bu sayfaya](https://www.vaultproject.io/docs/concepts/policies.html) bakın.

Yukarıdaki Politikayı acl.hcl adlı bir dosyaya kaydedin.

