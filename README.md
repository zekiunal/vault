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

### Sunucunun Çalıştığını Doğrulayın

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

## İlk Gizli Veri

Şimdi geliştirci sunucumuz ayakta ve çalışıyor. İlk gizli verimizi yazıp okuyabiliriz.

Vault'un temel özelliklerinden birisi gizli verilermizi güvenli bir şekilde okuma ve yazma yeteneğidir. Bu sayfada, CLI kullanarak bunu yapacağız, ancak Vault'un yeteneklerinden faydalanabileciğimiz eksiksiz bir HTTP API'si olduğunu bilmekte fayda var.

Vault'a yazılan gizli bilgiler önce şifrelenir daha sonra depolama alanına yazılır. Geliştirici sunucusunda, depolama alanı bellektir, ancak gerçek ortamda büyük olasılıkla disk veya Consul olacaktı. Vault veriyi depolama sürücüsüne teslim edilmeden önce şifreler. Depolama mekanizması şifrelenmemiş veriyi görmez ve Vault olmadan şifresini çözmek için gerekli araçlara sahip değildir.

### Gizli Bir Veriyi Yazma

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

### Gizli Bilgileri Okuma

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

### Bir Gizli Veriyi Silme

Artık gizli bir veriyi okuma ve yazmayı öğrendiğimize göre, silme işlemine geçebiliriz. Bunu `vault delete` ile yapabiliriz:

```shell
$ vault delete secret/hello
Success! Deleted 'secret/hello' if it existed.
```

## Depolama Birimleri

Daha önce, gizli verileri Valut'a nasıl yazacağımızı ve okuyacağımızı gördük. Bunu yapmak için, `secret/` önekini kullandık. Bu önek hangi depolama biriminin kullanılacağını belirtir. Varsayılan olarak Vault, `generic` adlı bir depolama birimini `secret`'a bağlar. `generic` depolama birimi, ham veriyi disk üzerinden okur ve yazar.

Vault `generic depolama birimine` ek olarak farklı depolama birimlerini de desteklemektedir ve bu özellik Vault'u benzersiz kılan özelliktir. Örneğin, AWS depolama birimi, isteğe bağlı olarak AWS erişim anahtarlarını dinamik olarak üretir. Başka bir örnek - bu tür bir depolama birimi henüz mevcut değil - doğrudan bir [HSM](https://en.wikipedia.org/wiki/Hardware_security_module)'ye veri yazar ve okur. Vault olgunlaştıkça giderek daha fazla depolama birimi eklenecektir.

Vault Depolama Birimlerini Dosya Sistemine Çok Benzer Şekilde Kullanır: 

Depolama birimleri belirli yollar (path) yardımı ile tanımlanır. Örneğin `generic depolama birimi` `secret/` önekini alarak tanımlanır.

Bu sayfada, depolama birimlerinin tanımlanmasını ve depolama birimleri ile gerçekleştirilebilecek işlemler hakkında bilgi edineceğiz. İlerleyen bölümlerde dinamik olarak gizli veri oluşturacağımız işlemlerde buradaki bilgilerden faydalanacağız.

### Depolama Birimi Tanımlama

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

### Depolama Birimini Kaldırma

Bir depolama birimi kaldırıldığında, bütün gizli veriler iptal edilir ve silinir. Bu işlemlerden herhangi biri başarısız olursa, depolama birimi kaldırma işlemi iptal edilir.

```shell
$ vault unmount generic/
Successfully unmounted 'generic/' if it was mounted
```

Bir depolama birimini kaldırdığınızda, tekrar eklemeniz mümkündür. Depolama birimini tekrar ekleme, depolama tanımının/bağlantı noktasını değiştirir.  Bu operasyonda yıkıcıdır. Saklanan veriler korunsa da gizli veriler `secret/` yoluyla bağlantılı olduğu için iptal edilmiştir. 

### Depolama Birimi Nedir?

Artık bir depolama birimini ekdiğinize ve çıkardığınıza göre: Depolama Birimi nedir ve bu depolama birimi tanımlama sisteminin anlamı nedir?




