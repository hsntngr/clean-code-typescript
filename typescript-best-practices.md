## İçindekiler

  1. [Değişkenler](#deikenler)
  2. [Fonksiyonlar](#fonksiyonlar)
  3. [Objeler ve Veri Yapıları](#objeler-object-ve-veri-yaplar-data-structures)
  4. [Sınıflar (Nesneler)](#snflarnesneler-class)
  5. [SOLID](#solid)
  6. [Testing](#testing)
  7. [Asenkron İşlemler](#asenkron-ilemler)
  8. [Hata Yönetimi](#hata-ynetimi-error-handling)
  9. [Yazım Kuralları](#yazm-kurallar-formatting)

## Değişkenler

### Bir anlam ifade eden değişken isimleri kullanın

Değişkenlerinizi ve parametrelerinizi isimlendirirken hem sizin hem de yazdığınız kodu okuyacak kimselerin anlayabilecekleri şekilde,
yazdığınız kodun içeriği ile ilgili olarak değişkenlerinizi anlamlı bir şekilde isimlendirin.

**Yanlış:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}

```

**Doğru:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ başa dön](#iindekiler)**

### Teleffauz edilebilir değişken isimleri kullanım

Telaffuz edilemeyen veya bir anlam ifade etmeyen isimler, kısaltmalardan oluşan üstü kapalı ifadeler hem akılda kalıcı
olmayacağı gibi hem kodunuzu sonradan inceleyen kimselerce anlaşılmayacaktır. Yazdığınız kodlara ilişkin ekip arkadaşlarınız ile
yaptığınız değerlendirmelerde yazılan kodları tartışmak, değişkenleri telaffuz etmek karmaşık bir hal alacaktır. 

Ayrıca kısaltmalardan oluşan, telaffuzu ve hatırlaması zor değişkenleri kullanmak istediğiniz bir çok zaman
geriye dönük olarak değişken ismini aramak zorunda kalacak ve zaman kaybedeceksiniz.

**Yanlış:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
}
```

**Doğru:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
}
```

**[⬆ başa dön](#iindekiler)**

### Belirlediğinize tipe `type` uygun olacak şekilde isimlendirme yapın.

Eğer yazdığınız bir fonksiyon bir servisten spesifik bir tipe `type` ait bir verinin getirilmesini sağlıyor
ve ya spesifik bir verinin düzenlenmesi işlevini getiriyorsa bu veri tipine `type` uygun isimlendirmeler yapmaya özen gösterin.

**Yanlış:**

```ts
// Burada aldığımız User adlı bir model olduğuna göre
// fonksiyona eklediğim ekstra ifadeler anlam kargaşasına yol açacaktır.
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**Doğru:**

```ts
function getUser(): User;
```

**[⬆ başa dön](#iindekiler)**

### Aramaya uygun ifadeler kullanın.

Bir kodu bir kez yazıyorsak defalarca kez okuruz. Bu yüzden okunabilir ve aranabilir kod yazmak önem arz ediyor.
Salt sayısal ifadeler, bir anlam ifade etmeyen sabitler hem sizin için hem de kodunuzu inceleyen ekip arkadaşlarınız için
hem anlaması hem de araştırması kısıtlı bir imkan sunacaktır. Bu nedenle yazdığınız sayısal ifadeler için anlamlı sabitler kullanın.

**Doğru:**

```ts
// 86400000 değeri neyi ifade ediyor?
setTimeout(restart, 86400000);
```

**Yanlış:**

```ts
// Sabitler tanımlarken büyük harf kullanın
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

**[⬆ başa dön](#iindekiler)**

### Açıklayıcı ifadeler kullanın

Bir değişken, parametre, fonksiyon veya sabit tanımlarken olabildiğince açıklayıcı ifadeler kullanın.

**Yanlış:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
  // ...
}
```

**Doğru:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // ...
}
```

**[⬆ başa dön](#iindekiler)**

### Sadece sizin değil, yazdıklarınızı okuyan herkesin anlayacağı ifadeler kullanın

Daha hızlı kod yazmak, daha az yer kaplayan kod yazmak ve sair maksatlarla kısa ifadelerden ve kısaltmalardan kaçının. 
Hem yazdığınız kodu sonradan okuyan sizin hem de ekip arkadaşlarınızın yazdığı kodu anlayabilmesi için açıklayıcı ifadeler
kullanın  

**Yanlış:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Doğru:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ başa dön](#iindekiler)**

### Gereksiz ifadelerden kaçının

Eğer değişkeniniz üst bir yapıya bağlı ise (sınıf `(class)`, type `(type)`) halihazırda bu üst yapı ile ilişki içerisinde bulunduğundan
bubu yapılara bağlı isimlendirme yaparken, bağlı olduğu üst yapıya ilişkin ifadeleri kullanmaktan kaçının. Daha yalın ifadeler kullanın

**Yanlış:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Doğru:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

**[⬆ başa dön](#iindekiler)**

### Varsayılan değerleri kullanmaya özen gösterin.

Bir fonksiyon veya metod içerisinde şarta bağlı yapılar veya devreler kullanmak yerine varsayılan `default` değerler kullanarak
bu sorunu çözebilirsiniz.

**Yanlış:**

```ts
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Doğru:**

```ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ başa dön](#iindekiler)**

### İlişkisel bir gruba ait değerleri bir enum altında tanımlayın

Enums can help you document the intent of the code. For example when we are concerned about values being
different rather than the exact value of those.

**Yanlış:**

```ts
projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case "horror":
        // ... 
      case "sci-fi":    
        // ...
    }
  }
}
```

**Doğru:**

```ts
enum GENRE {
  HORROR,
  SCIFI,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.HORROR:
        //...
      case GENRE.SCIFI:    
      //...    
    }
  }
}
```

**[⬆ başa dön](#iindekiler)**

## Fonksiyonlar

### Fonksiyon parametreleri (İdeal olarak 2 veya daha az parametre kullanın)

Fonksiyon kullanırken olabildiğince az sayıda parametre kullanmak hem yazdığınız fonksiyonları kullanırken hem de test 
sırasında büyük kolaylık sağlar.

Bir veya iki parametre kullanmak ideal iken, zorunluluk arz eden bir durum söz konusu olmadıkça üçüncü bir parametre
kullanılmaktan kaçınılmalıdır. Eğer fonksiyonunuz fazla sayıda parametreye ihtiyaç duyuyorsa, bu parametreleri, obje `{}`
yardımı ile gruplayarak göndermeniz daha sağlıklıdır.

Parametreleri obje yardımıyla gruplayarak göndermek bize bir takım faydalar sağlamaktadır. Bunlar;


1. Daha temiz bir syntax sunar. `function createUser(user: User)`

2. c# ve benzer dillerde olan isimlendirilmiş değişkenler `named parameters` benzeri bir kullanım kolaylığı sağlar.

3. Obje `{}` içerisinde yer alan parametreleri `destructing` yöntemiyle kolayca alabiliriz. Burada avantajlar olduğu gibi dezavantajlar da söz konusudur.
İlkel veri tipleri `primitives` obje içerisine kopyalanırken, obje `object - {}` ve diziler `array - []` referans yoluyla geçeceği için
obje içerisinden aldığınız obje ve diziler üzerinde yaptığınız bir düzenleme bir üst `scope`dan gönderilen objeyi de etkleyecektir.

Örneğin;

```ts
interface User {
  name: string;
  profession: {
    id: number;
    name: string;
  }
}

const name = "Teoman";
const user = {name, profession: {id: 4, name: 'Frontend Developer'}};

createUser(user);

console.log(user.name) // "Teoman";

console.log(user.profession) // {id: 4, name: 'Backend Developer'};

function createUser(user: User){
  let {name, profession} = user;
  name = "Kemal";
  profession.name = "Backend Developer"
}
```
Objeler ve diziler referans ile birlikte geçtiği için fonksiyon içinde yer alan obje ile fonksiyon dışında yer alan obje aynıdır.
Ram'de `memory` aynı noktayı işaret etmektedir. Bu nedenle fonksiyon içerisinde obje içerisinde yapılan bir değişiklik, 
fonksiyonun dışında yer alan objeyi de değitirecektir.

Ancak ilkel veri tipleri `primitives` fonksiyon içerisinde gönderilirken veya `destructing` yoluyla obje içerisinden dışarı çekilirken
klonlanacağı için yeni bir referansa sahip olur ve ram'de `memory` yeni bir noktayı işaret eder. Bu veri tiplerinin düzenlenmesi fonksiyon 
dışında bir sonuç doğurmaz.

**Yanlış:**

```ts
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Doğru:**

```ts
function createMenu(options: { title: string, body: string, buttonText: string, cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

Daha okunabilir daha temiz bir syntax ile kod yazmak için [type aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases) veya arayüz `Interface`
kullanılabilir

```ts

type MenuOptions = { title: string, body: string, buttonText: string, cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ başa dön](#iindekiler)**

### Her fonksiyon tek bir işlemi yerine getirmelidir.

Bu sadece typescript için değil, yazılım mühendisliğinin altın kurallarından biridir. Bir fonksiyon birden fazla görev yapmaya 
başladığı anda bu fonksiyonu farklı farklı yerlerde kullanmak veya testlerini gerçekleştirmek çok daha zor olacaktır.

Ancak eğer sadece tek bir işlemin gerçekleştirilmesinden sorumlu izole bir fonksiyon yaratırsak, hem bu fonksiyonu, gereken parametreleri
kendisine sağlamak suretiyle, istediğimiz yerde kullanabileceğimiz gibi hem de kolayca testlerini gerçekleştirebiliriz

**Yanlış:**

```ts
function emailClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Doğru:**

```ts
function emailClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ başa dön](#iindekiler)**

### Fonksiyonun işlevi isminde açıkça belirtilmelidir

**Yanlış:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// Tarihe eklenen gün mü, ay mı, yıl mı belli değil
// fonksiyonu kullanan kişininin fonksiyonun tanımına bakması gerekli
addToDate(date, 1);
```

**Doğru:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
// Şimdi tarihe eklenenin ay olduğunu kesin olarak biliyoruz
addMonthToDate(date, 1);
```

**[⬆ başa dön](#iindekiler)**

### Kod tekrarından uzak durun

Kod tekrarı yapmamak için olabildiğince özen gösterin. Peki neden? Uygulama içerisinde aynı işlevi yerine getiren onlarca kod bloğunun olması
demek en basit haliyle, değiştirmeniz gereken bir durum olduğunda, düzenlemeniz gereken onlarca kod bloğu olacak demektir. 

Çoğu zaman benzer bir mantalite ile çalışan ancak farklı işlevleri yerine getiren durumlara ilişkin kod yazarken, kod tekrarına
düştüğümüzün farkında olmayız. Benzer çalışan kod blokları farklı parametrelere faklı nesnelere bağlı çalışıyor olabilir. Bu nedenle
kodların birbirinden farklı olduğunu düşünebiliriz.

Ancak doğru bir soyutlama `(abstraction)` işlemi yapılarak, benzer mantıkta çalışan ancak farklı bağımlılıkları olan (farklı parametreler, farklı nesneler)
bu kod blokları bir fonksiyon, metod veya bir sınıf `(class)` altında toplanabilir ve bu değişkenler parametre olarak sağlanabilir. Bu sayede aynı yapıyı
sadece gereken değişkenler parametre olarak sağlanmak suretiyle ihtiyaç duyduğumuz her yerde kullanabiliriz. 

Soyutlama `(abstraction)` işlemi son derece önemlidir. Doğru bir soyutlama işlemi kurabilmek için [SOLID](#solid) prensibleri takip etmenizde fayda 
vardır. Yapılan kötü bir soyutlama işlemi `(abstraction)` yapılan kod tekrarıdan dahi daha kötü sonuçlar doğurabilir. Bu nedenle
kod yazmaya başlamadan önce olası senarylar düşünülmeli ve buna göre bir soyutlama işlemi gerçekleştirilmelidir.

**Yanlış:**

```ts
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Doğru:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

Kod tekrarı konusunda dikkat edilmesi gereken bir diğer nokta ise farklı kütüphaneler veya modüller içerisinde çalışırken, eğer bu modüller
birbirinden bağımsız, izole bir şekilde çalışıyor ise, yapılan bir soyutlama işlemi kod tekrarından daha olumsuz sonuçlar doğurabilir.
Bir modül diğer modüle bağımlı hale getirilebilir. Bu husus özellikle eager loading durumlarında ortaya çıkar, ufak bir kod parçası
yüzünden bütün bir modülün yüklenmesine sebebiyet verebilir ve kod tekrarından çok daha olumsuz bir sonuç ortaya çıkabilir.

**[⬆ başa dön](#iindekiler)**

### Destruction veya Object.assign kullanarak varsayılan (default) parametreler oluşturun

**Yanlış:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**Doğru:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // ...
}

createMenu({ body: 'Bar' });
```

Alternatif olarak parametrelere destructing yöntemi ile default değerler atayabiliriz.

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

// Burada doğrudan config parametresi demek yerine config parametrelerini destructing ile alabiliriz
// Eğer parametre sağlanmamış ise destructing sırasında tanımladığımız varsayılan değerleri kullanabiliriz
function createMenu({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) {
  // ...
}

createMenu({ body: 'Bar' });
```

`undefined` veya `null` değerlerden kaynaklanan istenmeyen durumların oluşmasını önlemek için typescript compiler ayarlarına
`"strictNullChecks": true` ekleyebilirsiniz.
Detaylı bilgi için [`strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks)

**[⬆ başa dön](#iindekiler)**

### Fonksiyon içerisinde şarta bağlı devreler oluşturmak için boolean parametreler `(flag)` göndermeyin

`Flag`leri kullanarak fonksiyon içerisinde şartı bağlı durumlar oluşturabilir ve farklı senaryoları tek bir fonksiyon
içerisinde değerlendirebiliriz. Ancak yukarıda belirttiğimiz üzere biz bir fonksiyonun tek bir işlemi yerine getirmesini 
istiyoruz. Farklı senaryolar söz konusu olduğu durumlarda, bu senaryoları değerlendirecek yeni fonksiyonlar oluşturulmalıdır. 

**Yanlış:**

```ts
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Doğru:**

```ts
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

**[⬆ başa dön](#iindekiler)**

### İstenmeyen durumlardan `(Side Effects)` kaçınmak (Birinci Bölüm)

İstenmeyen durum `(Side Effect)` ile belirtilmek istenen fonksiyonun kapsamını aşan, kendisinden beklemediğimiz işlemleri
gerçekleştirmesidir. Her fonksiyonun kendisine özel tek bir işlevi olup, bu işlevin kapsamının aşılması durumunda İstemeyen Durum
`(Side Effect)` oluşur. 

Mesela görevi konsola yazdırmaktan ibaret olan bir metodun global bir değişkene ait değeri değiştirdiğini veya kendisine gönderilen 
objenin içerisinde yer alan bilgileri değiştirdiğini düşünelim. Fonksiyonun görevi sadece bir yazdırma işleminden ibaretken, kendisinden
beklenmeyen (boyunu aşan) bir takım işlemler gerçekleştirmektedir. Yapılan bu kapsam dışı, yetkisiz işlemler `İstenmeyen Durum 
(Side Effect)` olarak adlandırılır.

**Yanlış:**

```ts
// Fonksiyon içerisinde yer alan değişken parametre olmayıp bir üst scope'ta yer alan 
// global değişkene bağlıdır..
let name = 'Robert C. Martin';

function toBase64() {
  name = btoa(name);
}

toBase64();
// Eğer name değişkenini kullanan başka bir değişken varsa 'Robert C. Martin' değerini değil
// base64 encode edilmiş değerini alacaklardır

console.log(name); // 'Um9iZXJ0IEMuIE1hcnRpbg=='
```

**Doğru:**

```ts
const name = 'Robert C. Martin';

function toBase64(text: string): string {
  return btoa(text);
}

const encodedName = toBase64(name);
console.log(name);
```

**[⬆ başa dön](#iindekiler)**

### İstenmeyen durumlardan `(Side Effects)` kaçınmak (İkinci Bölüm)

Javascript dilinde ve doğal olarak Javascript için yazılmış bir süper set olan Typescript'te ilkel veri tipleri `primitives` değerleri
ile geçerken, Objeler `(Object)` ve diziler `(Array)` referens yoluyla geçer. 

Burada muhtemel aklınızda oluşan iki soru var. Birincisi 'geçmek' ile kast edilen nedir ? Bir ikincisi değer yoluyla veya referans
yoluyla geçmek ne anlam ifade ediyor. Öncelikle aşağıda anlatacaklarımıza geçmeden önce bu iki hususa değinmekte fayda var.

1. Geçmek ile kast edilen bir veri tipinden kalıtım alınması demektedir. Kalıtım alınması fonksiyona parametre olarak geçiş sırasında
gerçekleşebileceği gibi, başka bir değişkene tanımlama yoluyla da gerçekleşebilir. 

```ts
// Burada b değerini a dan kalıtım almakta, a ilkel bir veritipi olduğu için değer yoluyla geçmektedir.
const a = 10;
const b = a;
```

2.Peki değer yoluyla veya referans yoluyla geçme ne anlam ifade ediyor? Kod yazarken tanımladığımız her veri ram'de `(memory)` de belirli
bir alanda saklanmaktadır. Ancak ilkel veriler `(primitives)` geçerken (kalıtım verirken) klonlanarak geçiş yapar. Yani kalıtım alan
ilkel veri ile kalıtım veren ilkel veri ram'de `(memory)` aynı noktada saklanmaz, kalıtım alan klonlandığı için yeni bir alanda saklanır.

```ts
let a = 47;
// b nin değeri a ya eşittir ancak ram (memory) içerisinde farklı noktalarda saklanırlar
let b = a;

// a üzerinde yapılan bir değişim b yi etkilemez
a = 1;

console.log(a) // 1
console.log(b) // 47
```

Referans yoluyla geçişlerde ise klonlama yoktur, kalıtım verenin kendisi kalıtım alana geçer. Ram üzerinde yeni bir alan oluşturulmaz. 
Hem kalıtım alan hem de kalıtım veren aynı noktada saklanır, her ikiside aynı veriye bağlıdır. Javascript dilinde objeler `(object)` ve diziler
`(array)` referans yoluyla geçerler.

```ts
const user = {name: "Hasan Teoman Tıngır"};
// user ile person artık aynı objeye bağlıdır
const person = user;

// a üzerinde yapılan bir değişim b yi etkiler
user.name = "Kemal Gözler";

console.log(user) // {name: "Kemal Gözler"}
console.log(person) // {name: "Kemal Gözler"}
```
Objeler veya diziler `(Array)` referans yoluyla geçtiği için ve bu veriler üzerinde yapılan işlemler doğrudan objenin ve dizinin
kendisini değiştirdiği için, uygulamada bir takım istenmeyen sonuçların `(side effects)` doğmasın sebebiyet vermektedir. Bu nedenle
state yönetim sistemlerinde obje ve dizilerden oluşan state dondurularak `(Object.freeze)` state üzerinde kalıcı işlemlerin yapılması
bir nebze engellenmeye çalışılmaktadır. Ancak state yönetimi kullanılmayan uygulamalarda ise bu işin sorumluluğu geliştiricilerin 
ellerine kalmaktadır. 

İstenmeyen bir sonucun oluşmasını engellemek amacıyla obje ve diziler `(array)` ile çalışırken doğrudan bu verileri düzenlemek yerine
bu veriler içerisinde yer alan bilgileri `destructing` yöntemi ile elde edip kullanmak ve bu verileri doğrudan değiştirmemek son derece
önem arz etmektedir. 

Konuyu daha iyi anlamak için örnek bir senaryo üzerinden gidelim. Bir e-ticaret uygulamamız olduğunu düşünelim, kullanıcının sepet ikonuna
tıkladığında seçtiği ürünü `cart` dizisine eklediğimizi alış verişi tamamla dediğinde `cart` içerisinde yer alan bütün ürünleri ise ajax ile
gönderdiğimizi düşünelim. 

Uygulamamızda her isteğin bir hata durumunda 3 kez tekrarlandığı bir senaryoda, ilk isteğin sunucunun boot zamanına denk geldiğini ve
ikinci isteğin 5 sn sonra yeniden gönderileceği bir durumda, kullanıcının yanlışlıkla sepete ürün eklediğini düşünelim. 

Eğer doğrudan `cart` dizisine bu ürünleri ekliyorsak, her ürün eklediğimizde yeni bir dizi oluşturmuyorsak, kullanıcının son yaptığı
gerek hatalı gerek kasıtlı bir işlem sonucu sonradan eklediği ürünü ikinci istek ile `(retry)` sunucuya gönderdiğimizi ve kullanıcının
öncesinde tamamladığı ödemeye eklediğimizi düşünelim. Kullanıcı sonrasında faturaya baktığında boom!! sepete sonradan eklediği ürünün
de satın alındığını görecek. Bu hem kullanıcının hem de bizim istemediğimiz bir sonuç. 

İşte bu gibi istenmeyen durumların (fonksiyonun uygulamanın `state`'ini değiştirmesi gibi) `(Side Effect)` oluşmasını engellemek için, bilhassa obje ve diziler `(array)` üzerinde işlem 
yaparkendaha temkinli hareket etmeli, uygulama genelinde kullanılan verimiz `(state)` üzerinde kalıcı değişiklikler yapmadan işlemleri
gerçekleştirmeliyiz. 

**Yanlış:**

```ts
function addItemToCart(cart: CartItem[], item: Item): void {
  cart.push({ item, date: Date.now() });
};
```

**Doğru:**

```ts
function addItemToCart(cart: CartItem[], item: Item): CartItem[] {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ başa dön](#iindekiler)**

### Global fonksiyonlar yazmayın.
Javascript ile çalışırken hiçbirimiz pure javascript ile sıfırdan bir uygulama yazmaya çalışmıyoruz. Bu hem ciddi miktarda zaman hem de 
ciddi miktarda emek demek. Bunun yerine daha öncesinde yazılmış olan kütüphaneleri sık sık kullanıyoruz. 

Eğer global bir fonksiyon tanımlıyorsak, yazdığımız bu global fonksiyonun başka bir kütüphaneye ait fonksiyonu `override` etme riski
ile karşı karşıya kalmamız gibi tehlikeli bir olasık soz konusu olacaktır. 

Örneğin native `Array` sınıfını genişlettiğimizi `(extend)` ve `diff` adlı, iki dizi `(array)` arasındaki farklı itemleri gösteren bir 
metod eklediğimizi düşünelim. Aynı metodu kullandığımız bir kütüphanenin, farklı bir syntax veya farklı bir işlevle tanımlamış durumunda
kütüphanin işleyişine kalıcı olarak hasar vermek gibi sonuç ile karşı karşıya kalacağız. 

Bu gibi durumlardan kaçınmak için doğrudan global fonksiyonlar oluşturmak ve native sınıfları genişletmek `extend` yerine bu sınıflardan
miras alan `inheritance` özel sınıflar (collections vs) oluşturabilir ve yardımcı metodlarımızı bu sınıflar üzerinde tanımlayabiliriz. 
    
**Yanlış:**

```ts
declare global {
  interface Array<T> {
    diff(other: T[]): Array<T>;
  }
}

Array.prototype.diff = function <T>(other: T[]): T[] {
  const hash = new Set(other);
  return this.filter(elem => !hash.has(elem));
};
```

**Doğru:**

```ts
class MyArray<T> extends Array<T> {
  diff(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**[⬆ başa dön](#iindekiler)**

### Olabildiğince fonksiyonel bir şekilde kod yazmaya özen gösterin.

Javascript her an her saniye gelişen canlı bir dil. Daha rahat ve esnek bir geliştirme ortamı sağlamak amacıyla TC39 ve bir çok 
gönüllü geliştirici ciddi bir çaba sarf ediyor.

Bu gelişmeleri takip etmek, yeniliklere hakim olmak ve eski alışkanlıkları terk edip olabildiğince bu yenilikleri uygulamak gerekiyor.
Bu yenilikler hem kod yazımında büyük kolaylıklar sunarken, okunurluk `(readibilty)` açısından da daha temiz bir syntax içeriyor.

**Yanlış:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < contributions.length; i++) {
  totalOutput += contributions[i].linesOfCode;
}
```

**Doğru:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = contributions
  .reduce((totalLines, output) => totalLines + output.linesOfCode, 0);
```

**[⬆ başa dön](#iindekiler)**

### Koşulları fonksiyon veya metodlar içerise taşıyın
`Encapsulation` işlemi gerçekleştirerek, yani var olan kodumuzu bir fonksiyon veya bir metod yardımıyla bütünleşik bir yapı içerisine
taşıyarak okunabilirliği `(readibility)` artırmaya özen göstermeliyiz.

**Yanlış:**

```ts
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**Doğru:**

```ts
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

**[⬆ başa dön](#iindekiler)**

### Olumsuz koşullardan kaçının

**Yanlış:**

```ts
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**Doğru:**

```ts
function isEmailUsed(email: string): boolean {
  // ...
}

if (!isEmailUsed(node)) {
  // ...
}
```

**[⬆ başa dön](#iindekiler)**

### Koşul oluşturmaktan kaçının

Evet ilk bakışta kulağa biraz saçma geliyor olabilir ancak olabildiğince şarta bağlı yapılardan uzak durmalıyız. Elbette `if` kullanmamız
gereken şarta bağlı işlemler gerçekleştireceğimiz yerler olacaktır. Ancak bunu yaparken nesne tabanlı programlamanın bütün gücünden
faydalanmalı, son çare olarak koşul oluşturma yoluna başvurmalıyız.

Mesela aşağıda örnekte olduğu gibi polimorfik `(polymorphic)` bir yapı kurarak üstesinen gelebileceğimiz bir durumun söz konusu
olduğunda her olası senaryo için ayrı bir koşul oluşturmak hem kullanım `(usability)` hem de okunurluk `(readbility)` açısından olumsuz
sonuçlar doğuracaktır

**Yanlış:**

```ts
class Airplane {
  private type: string;
  // ...

  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
      default:
        throw new Error('Unknown airplane type.');
    }
  }

  private getMaxAltitude(): number {
    // ...
  }
}
```

**Doğru:**

```ts
abstract class Airplane {
  protected getMaxAltitude(): number {
    // shared logic with subclasses ...
  }

  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ başa dön](#iindekiler)**

### Kodunuzu gereğinden fazla optimize etmeyin

Javascript web programlamada mihenk taşlarından biri halinden geldiğinden beridir modern tarayıcılar artık bir çok optimizasyonu kendileri
yapmaktadır. Keza artık geliştiriciler esnext ile gelen her yeniliği yakından takip ettiği ve kullandığı için bir çok üst seviye
javascript kodu tarayıcı desteğinden yoksun olup Babel ve benzeri `transpiler` yardımı ile alt seviye javascript koduna dönüştürmekte
ve gerekli olan optimizasyonların bir çoğunu bu `transpiler` dediğimiz kod dönüştürücüler gerçekleştirmektedir.

**Yanlış:**

```ts
// Eski tarayıcılar önbellekleme özelliğini etkin kullanmadığı için her iteration sırasında 
// list dizisinin item sayısını tekrar tekrar tekrar hesaplamaktadır
// Ancak artık modern tarayıcılar sadece ilk iteration sırasında bu değeri hesaplamakta
// ve sonraki iterationlarda bu değeri kullanmaktadır
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Doğru:**

```ts
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ başa dön](#iindekiler)**

### Kullanılmayan kodları (dead code) kaldırın.

Kullanımı bırakılan komple terkedilen veya yerine yenileri getirilen `(deprecated)` kodları Kullanılmayan kod/Ölü kod `(dead code)`.
olarak adlandırıyoruz. 

Çalışma alanınızda, eğer varsa, kullanımdan kaldırılan bu kodları temizleyin. Bu hem daha temiz bir çalışma ortamı hem de daha küçük
`bundle` boyutu sağlar.

**Yanlış:**

```ts
function oldRequestModule(url: string) {
  // ...
}

function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Doğru:**

```ts
function requestModule(url: string) {

}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**[⬆ başa dön](#iindekiler)**

### `Iterables` ve `generator`leri kullanın

Yüksek sayıda item içeren veri yığınları ile çalışırken `iterator` ve `generator`leri kullanmanın bir takım avantajları vardır

- Hangi itemlerin erişilebilir olduğunu manuel olarak belirleyebilirsiniz
- Daha az işlem gücü gerektirir. 
- Iterator ile birlikte `for..of ` syntaxı kullanılabilir
- Özel iteratorler yazıbilir, farklı veri yapılarına entegre edilebilir böylece daha yüksek performans elde edilebilir. (Örn. Binary Tree)

**Yanlış:**

```ts
function fibonacci(n: number): number[] {
  if (n === 1) return [0];
  if (n === 2) return [0, 1];

  const items: number[] = [0, 1];
  while (items.length < n) {
    items.push(items[items.length - 2] + items[items.length - 1]);
  }

  return items;
}

function print(n: number) {
  fibonacci(n).forEach(fib => console.log(fib));
}

// İlk 10 fibonnaci sayılarını yazdır.
print(10);
```

**Doğru:**

```ts

// Sınırsız sayıda fibonacci sayıları üretir.
// Generator bütün sayılara ait rakamları tutmaz. Yani sadece o anki dizi ile ilgilenir. Önceki ve sonraki diziye ait bilgileri
// biriktirmez. Bu sayede kullanımı tamamlanan diziler Garbage Collectorun sularına doğru yol alır
function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function print(n: number) {
  let i = 0;
  for (const fib of fibonacci()) {
    if (i++ === n) break;  
    console.log(fib);
  }  
}

// İlk 10 fibonnaci sayılarını yazdır.
print(10);
```
`Iterables` metodların aynı native diziler `(Array)` gibi kullanılmasını sağlamak amacıyla çeşitli açık kaynak kodlu kütüphaneler
mevcut. Bu kütüphaneler sayesinde dizilere ait `map`, `slice`, `forEach` gibi metodları `Iterables` için kullanabilirsiniz. 

1. [itiriri](https://www.npmjs.com/package/itiriri)
2. [itiriri-async](https://www.npmjs.com/package/itiriri-async)

```ts
import itiriri from 'itiriri';

function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];
 
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

itiriri(fibonacci())
  .take(10)
  .forEach(fib => console.log(fib));
```

**[⬆ başa dön](#iindekiler)**

## Objeler `(Object)` ve Veri Yapıları `(Data Structures)`

### `getter` ve `setter` kullanın

Typescript `getter/setter` kullanımını desteklemektedir.. Bu `notation`'ları kullanarak verinin obje içerisine eklenmesi veya getirilmesi
sırasında obje üzerinde çeşitli manipülasyonlar yapabilirsiniz. Bu `notation`'lar bize bir takım kullanım kolaylıkları sağlamaktadır

- Sadece bir `property` nin obje içerisinde varlığını veya yokluğunu aşan bir durumda, belli bir durumun varlığını veya kontrol etmek
için `getter` kullanabilirsiniz. Bu aynı zamanda obje çapında bir soyutlama `(Abstraction)` yapmanızı da sağlar.
**Yanlış:**
```ts
const user = {
  name: "Kemal Gözler",
  emailVerifiedAt: "12-04-2020",
  status: "Approved"
}

if (user.emailVerifiedAt && status === "Approved") {
  //...
}
```
**Doğru:**
```ts
const user = {
  name: "Kemal Gözler",
  emailVerifiedAt: "12-04-2020",
  status: "Approved",

  get approved(){
    return user.emailVerifiedAt && user.status === "Approved"
  } 
}

if (user.approved) {
  //...
}
```

- `set` işlemi sırasında validasyon işlemlerini yapabilirsiniz.
- Obje içerisindeki `property`ler obje dışında çağrılırken veya set edilirken manipüle edilebilir.
- `set` ve `get` kullanımı sayesinde hata yönetimi ve hata loglama işlemleri daha zarif bir şekilde halledilebilir.
- Eğer *remote* bir veri söz konusu ise bu veri obje oluşturulduğu sırada değil, `propert` çağrıldığı sırada `lazy loading` ile
çağrılabilir.

**Yanlış:**

```ts
type BankAccount = {
  balance: number;
  // ...
}

const value = 100;
const account: BankAccount = {
  balance: 0,
  // ...
};

if (value < 0) {
  throw new Error('Cannot set negative balance.');
}

account.balance = value;
```

**Doğru:**

```ts
class BankAccount {
  private accountBalance: number = 0;

  get balance(): number {
    return this.accountBalance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error('Cannot set negative balance.');
    }

    this.accountBalance = value;
  }

  // ...
}

// Burada BankAccount sınıfı validasyon işlemi ile de tek elden ilgilenmektedir.
// Eğer ileride yeni bir validasyon eklenecek ise tek yapılması gereken setter uygulanımını güncellemek.
// Getirilen yeni validasyon uygulama genelinde geçerli olacaktır. Bu aynı zamanda güçlü bir soyutlama (abstraction)
// imkanı sunar bize
const account = new BankAccount();
account.balance = 100;
```

**[⬆ başa dön](#iindekiler)**

### Private ve protected metodlar oluşturun

Typescript `public` *(default)*, `protected` ve `private` gibi çeşitli erişim kurallarını `(accessors)` desteklemektedir. 
Bu kuralları kullanarak tanımladığınız metodun veya `property`'nin hangi erişim izinlerine sahip olduğunu tanımlayabilirsiniz 

**Public:** (halka açık, kamu) erişim kuralı ile tanımladığımız metotlara ve verilere her nesneden erişebiliriz.
**Private:** (özel, gizli) erişim kuralı ile tanımladığımız bir metoda sadece bu metodun tanımlandığı sınıfdan örneklendirilmiş nesnelerin içinden erişim hakkına sahip oluruz.
**Protected:** (korumalı) erişim kuralı ile tanımladığımız bir metoda ise bu metodun tanımlandığı sınıfdan örneklendirilmiş nesnelerin içinden ve bu sınıfdan türetilmiş olan alt nesnelerden erişebiliriz.

[Kaynak: Kapsülleme](https://www.wikiwand.com/tr/Kaps%C3%BClleme)

**Yanlış:**

```ts
class Circle {
  radius: number;
  
  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**Doğru:**

```ts
class Circle {
  constructor(private readonly radius: number) {
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**[⬆ başa dön](#iindekiler)**

### Değiştirilemez veriler ile çalışmaya özen gösterin.

Typescript ile *readonly* notasyonunu kullanarak istediğiniz bir `property`i değiştirilemez hale getirebilirsiniz. Bu sayede istenmeyen
sonuçlardan `(side effects)` bir nebze olsun kaçınabiliriz.

Daha gelişmiş senaryolar için `generic` bir `type` olan `Readonly<T>` `type`ını kullanabilirsiniz. Bu sayede bir sınıfa ait bütün
`property`ler readonly olarak işaretlenir ve dışarıdan gelen bütün müdahelelere kapatılır.
 
**Yanlış:**

```ts
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**Doğru:**

```ts
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```
Verinin bir dizi `(Array)` olması durumunda ise `ReadonlyArray<T>` `type`ını kullanabilirsiniz. Readonly olarak işaretlenmiş olan bir dizi
içerisinde yer alan itemle artık değiştirilemez ve bu diziye yeni bir item eklenemez. Dolayısıyle `push()` ve `fill()` gibi native
metodlar artık kullanılamaz. Ancak dizi içerisinde yer alan itemleri etkilemeyen ve tamamen yeni bir dizi döndüren `concat()` ve
`slice()` gibi metodlar ise kullanılabilir.

**Yanlış:**

```ts
const array: number[] = [ 1, 3, 5 ];
array = []; // error
array.push(100); // array will updated
```

**Doğru:**

```ts
const array: ReadonlyArray<number> = [ 1, 3, 5 ];
array = []; // error
array.push(100); // error
```

3.4 ve üzeri sürümlerde `readonly` notasyonu kullanılabilir

```ts
function hoge(args: readonly string[]) {
  args.push(1); // error
}
```
`const` notasyonu ile sabit değişkenler oluşturun.

Detaylı bilgi için; [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions)

**Yanlış:**

```ts
const config = {
  hello: 'world'
};
config.hello = 'world'; // değer değişti

const array  = [ 1, 3, 5 ];
array[0] = 10; // değer değişti

function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // değer değişti
```

**Doğru:**

```ts
// obje artık değştirilemez.
const config = {
  hello: 'world'
} as const;
config.hello = 'world'; // hata verir

// dizi değiştirilemez
const array  = [ 1, 3, 5 ] as const;
array[0] = 10; // hata verir

// obje değiştirilemez
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // hata verir
```

**[⬆ başa dön](#iindekiler)**

### Tip vs arayüz `(type vs interface)`

Ne zaman `type` ne zaman `interface` kullanmalıyız. Bunu anlamak için bu kavramlara iyice hakim olmak gerekiyor. Öncesinde bazı
fundamental kavramlara hakim olmakta fayda var.

[Basic Types](https://www.typescriptlang.org/docs/handbook/basic-types.html)
[Advanced Types](https://www.typescriptlang.org/docs/handbook/advanced-types.html)
[Interface](https://www.typescriptlang.org/docs/handbook/interfaces.html)

Genel olarak eğer birden fazla tanımlamayı birlikte veya alternatif olarak kullanmak gerekiyorsa yani `union` veya `intersect` olarak
tanımlanan işlemler ile ihtiyacımızı karşılayabiliyorsak `type` oluşturmak ihtiyacımızı görecektir.

Ancak yeri geldiğinde genişletmek `(extends)` yeri geldiğinde ise bir nesneye `(class)` uygulamak `(implement)` istediğimiz bir
tanımlama söz konusu ise bu tanımlama için arayüz `(interface)` oluşturmak daha yerinde olacaktır.

**Yanlış:**

```ts
interface EmailConfig {
  // ...
}

interface DbConfig {
  // ...
}

interface Config {
  // ...
}

//...

type Shape = {
  // ...
}
```

**Doğru:**

```ts

type EmailConfig = {
  // ...
}

type DbConfig = {
  // ...
}

type Config  = EmailConfig | DbConfig;

// ...

interface Shape {
  // ...
}

class Circle implements Shape {
  // ...
}

class Square implements Shape {
  // ...
}
```

**[⬆ başa dön](#iindekiler)**

## Sınıflar/Nesneler `(class)`

### Sınıflar olabildiğince minimal olmalıdır
Bir sınıf oluştururken ilk dikkat edilmesi gereken bir SOLID prensini olan *Single Responsibility* prensibidir. Yani bir nesne sadece
tek bir amaç doğrultusunda oluşturulmalı ve sadece o amaca hizmet etmelidir. Fonksiyonlara benzer bir şekilde nesnelerde amacı dışında 
çıkmamalı, nesnelere kapsamını aşan görevler yüklenmemelidir.

Eğer benim kendisine sağlanan hayvanın kollarını ve bacaklarını saymaktan sorumlu bir nesnem varsa bu nesne aynı zamanda hayvanın ortalama
yaşam süresini de hesaplıyorsa burada kapsam dışı, sınıfın amacını aşan bir kullanım söz konusu olup `Single Responsibility` prensibine
aykırı bir durum vardır.

**Yanlış:**

```ts
class Dashboard {
  getLanguage(): string { /* ... */ }
  setLanguage(language: string): void { /* ... */ }
  showProgress(): void { /* ... */ }
  hideProgress(): void { /* ... */ }
  isDirty(): boolean { /* ... */ }
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  addSubscription(subscription: Subscription): void { /* ... */ }
  removeSubscription(subscription: Subscription): void { /* ... */ }
  addUser(user: User): void { /* ... */ }
  removeUser(user: User): void { /* ... */ }
  goToHomePage(): void { /* ... */ }
  updateProfile(details: UserDetails): void { /* ... */ }
  getVersion(): string { /* ... */ }
  // ...
}

```

**Doğru:**

```ts
class Dashboard {
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  getVersion(): string { /* ... */ }
}

// ...
```

**[⬆ başa dön](#iindekiler)**

### Yüksek bütünleşme ve düşük bağlanma `(High cohesion ve low coupling)`
Bu kavramları doğrudan türkçe aktarmak güç. Ancak dilin elverdiği ölçüde açıklamaya çalışacağım.

Bütünleşme yani `cohesion` ile ifade edilen bir nesnenin `(class)` yönettiği veri `(data)` ile bu veriyi yönetmesini sağlayan metodların uyum içerisinde çalışması
metodlarının görevinin bu verinin veya nesnenin yapması gereken işlevin dışına çıkmaması, bütünleşik bir şekilde çalışması demektedir.  

**Yanlış:**
```ts
declare class User{
  constructor(private name: string, private email: string) 
  //... getters and setters
  sendEmail(): void
  approveUser(): void
}
```
Yukarıda gereğinden fazla görevi yerine getiren, sınırlarını zorlayan bir model söz konusu. Eğer model sadece getter ve setter metodlarından oluşsa
sendEmail gibi işlemler için ayrı bir `Mailer` nesnesi oluşturulsa daha sağlıklı bir yapı kurulabilecekken, işlemler tek bir sınıf altında toplanmış,
model konsepti ile *bütünleşik* olmayan bir nesne oluşturulmuştur.
**Doğru:**
```ts
declare interface CanReceiveMail {
  setEmail(): string 
  getEmail(): string 
}

declare class User extends Model implements CanReceiveMail {
  constructor(private name: string, private email: string) 
  setName(): string
  getName(): string 
  setEmail(): string 
  getEmail(): string 
}

declare class Mail {
  //... diğer mail metodları
  to(to: string | CanReceiveMail): Mail
}

declare class Mailer {
  send(mail: Mail): void
}

declare class UserService {
  approveUser(user: User): boolean
}
```
Yukarıda gerçekleşmesini istediğiniz işlemleri farklı sınıflara bölerek hem `Single Responsiblity` prensibine uygun hem de amacına uygun, yönettiği veri ile
*bütünleşik* `(cohesional)`, nesne tabanlı programlamada amaç edinilen **yüksek bütünleşme** prensibine uygun bir yapı oluşturduk.

**Düşük bağlanma** ise nesnelerin olabildiğine izole olarak oluşturulması, oluşturulan bir modül içerisinde diğer nesneler ile arasındaki ilişkinin olabildiğince
az ve belli koşullar altında gerçekleşmesi. Belirli bir nesnenin varlığından bağımsız olarak çalışabilmesi demektedir. 

Bir nesne, component veya modül her ne kadar çevresinden bağımsız `(decoupled)` ise kullanım açısından, özellikle de tekrar tekrar `(reusability)` farklı çalışma
ortamlarında kullanılması açısından bir o kadar iyidir.

Yukarıda yer alan örneği sürdürsek `Mail` sınıfı hem bir mail adresine hem de `CanReceiveMail` sınıfına ait özellikleri yani içerisinde email adresi olan herhangi
bir sınıf ile birlikte çalışabilecektir. Ancak `CanReceiveMail` yerine `User` demiş olsaydık bu maili ancak bir email adresine veya sadece user modeline
gönderebilecektik. Ancak artık biz bu maili `Model` olsun olmasın, `User` olsun olmasın `CanReceiveMail` arayüzünü uygulayan `(implements)` her nesneyi mail gönderirken
kullanabiliriz.

**Yanlış:**

```ts
class UserManager {
  // Bu örnekte private property iki veya daha fazla sayıda method tarafından kullanılmaktadır.
  // Bu örnek ile daha açık bir şekilde anlaşılacağı üzere bu sınıf kapasitesini aşan işlerden sorumludur.
  // Örnekte sadece temel bilgiler almak, kullanıcı iblgilerini görüntülemek için bile olsa `EmailSender`,
  // sınıfı sağlanmak zorundadır. Kullanıcı modeli, bir modelin amacını aşan görevleri gerçekleştirmektedir.
  constructor(
    private readonly db: Database,
    private readonly emailSender: EmailSender) {
  }

  async getUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await db.transactions.find({ userId });
  }

  async sendGreeting(): Promise<void> {
    await emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**Doğru:**

```ts
class UserService {
  constructor(private readonly db: Database) {
  }

  async getUser(id: number): Promise<User> {
    return await this.db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await this.db.transactions.find({ userId });
  }
}

class UserNotifier {
  constructor(private readonly emailSender: EmailSender) {
  }

  async sendGreeting(): Promise<void> {
    await this.emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await this.emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**[⬆ başa dön](#iindekiler)**

### Bir nesneden miras almak `(inheritence)` yerine `Composition` kullanın.

Öncelikle `Composition` kavramının ne olduğu üzerinde durmakta fayda var. En basit haliyle, nesnelerin birbiri içerisinde kullanılması demektir. Ancak yukarıda
nesneleri olabildiğince izole bir şekilde çalışmasından, diğer nesnelerden bağımsız bir şekilde çalışmasından bahsetmişken peki neden şimdi bir öyle bir böyle 
konuşuyoruz.

Nesnelerin izole olmasını ve birbirinden bağımsız çalışmasını istiyoruz ancak bu nesnelerin kendi işi olmayan görevleri de yapmasını istemiyoruz. Dolayısıyla bir
noktada farklı nesnelerin varlığına ihtiyaç duyuyoruz. Ancak bu ihtiyacımızı belirlerken yaptığımız soyutlama önem arz ediyor. Yani kalıcı olarak spesifik bir 
nesneye değil, arayüzler `(interface)` kullanarak ihtiyacımız olan şartları sağlayan herhangi bir nesne ile çalışabilecek şekilde bir yapı kurmalıyız.

İşte `Composition` tam bu noktada devreye giriyor. Ben kullanıcılara mail göndermek için, `Mail` sınıfında sadece kullanıcının mail adresine ulaşmak için, 
koca bir modeli miras almak yerine, oluşturduğum Mail sınıfında `CanReceiveMail` arayüzünü uygulayan `(implements)` `User` modelimi kullanabilirim. 
Bu sayede hem Mail nesnemi, mail adresi dışında hiç bir ortak noktasının olmadığı `Model` sınıfından ayırmış olurum hem de `CanReceiveMail` arayüzünü uygulamış
`(implements)` bütün nesnelerin sahip olduğu mail adreslerine mail gönderebilirim.

**Yanlış:**

```ts
class Employee {
  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  // ...
}

// Burada yaptığımız işlem oldukça kötü bir pratik çünkü 
// EmployeeTaxData doğrudan Employee modeli ile ilişkisi
// yok hatta herhangi bir ortak property'si dahi yok 
class EmployeeTaxData extends Employee {
  constructor(
    name: string,
    email: string,
    private readonly ssn: string,
    private readonly salary: number) {
    super(name, email);
  }

  // ...
}
```

**Doğru:**

```ts
class Employee {
  private taxData: EmployeeTaxData;

  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  setTaxData(ssn: string, salary: number): Employee {
    this.taxData = new EmployeeTaxData(ssn, salary);
    return this;
  }

  // ...
}

class EmployeeTaxData {
  constructor(
    public readonly ssn: string,
    public readonly salary: number) {
  }

  // ...
}
```
ÇN: Yukarıdaki örnek daha akla yatkın ve doğru ancak yine de şöyle bir sıkıntı var. Öncelikle yukarıda yer alan kodu çevirdiğim dökümandan aldığım için müdahele etmedim.
Ancak dökümanı yazan şahsın kasıtlı veya kasıtsız olabilir atladığı bir husus var. `setTaxData` metodunda `EmployeeTaxData` örneği oluşturup kullanıyoruz. Ancak
farklı durumlarda, örneğin farklı ülkelerde, vergi bilgileri farklı şekillerde elde ediliyor olabilir. Bu durumda `EmployeeTaxData` benzeri farklı bir nesneye
ihtiyaç duyacağız. 

Ancak `EmployeeTaxData` sınıfını doğrudan `setTaxData` metodu içerisinde örneklediğimiz için dışarıdan müdahele etme şansımız sıfır. Üstelik yukarıda bahsettiğimiz
gibi bir sınıfı başka sınıflara bağımlı ettiğimiz zaman, doğrudan spesifik bir sınıfa değil, bir arayüz `(interface)` kullanarak soyut bir varlığa bağımlı hale
getirmek dah doğru bir yol olur.

**Daha Doğru:**

```ts
class Employee {
  private taxData: TaxData;

  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  setTaxData(taxData: TaxData): Employee {
    this.taxData = taxData;
    return this;
  }
  // ...
}

interface TaxData {
  getTaxData()
}

class EmployeeTaxData implements TaxData {
  getTaxData(){
  }
}

class EmployeeTaxDataAtUSA implements TaxData {
  getTaxData(){
  }
}
```

**[⬆ başa dön](#iindekiler)**

### Zincirleme metodlar `(method chaining)` kullanın.

Bu syntax sıklıkla kütüphanelerde kullanılmaktadır. Genellikle `setter` metodlarda `void` yerine objenin kendisi döndürülmek suretiyle akışkan `fluent` bir yapı
oluşturulur. Bu yapı hem kullanım `(usability)` hem de okunurluk `(readibility)` açısından kolaylık sağlar.


**Yanlış:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): void {
    this.collection = collection;
  }

  page(number: number, itemsPerPage: number = 100): void {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
  }

  orderBy(...fields: string[]): void {
    this.orderByFields = fields;
  }

  build(): Query {
    // ...
  }
}

// ...

const queryBuilder = new QueryBuilder();
queryBuilder.from('users');
queryBuilder.page(1, 100);
queryBuilder.orderBy('firstName', 'lastName');

const query = queryBuilder.build();
```

**Doğru:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): this {
    this.collection = collection;
    return this;
  }

  page(number: number, itemsPerPage: number = 100): this {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
    return this;
  }

  orderBy(...fields: string[]): this {
    this.orderByFields = fields;
    return this;
  }

  build(): Query {
    // ...
  }
}

// ...

const query = new QueryBuilder()
  .from('users')
  .page(1, 100)
  .orderBy('firstName', 'lastName')
  .build();
```

**[⬆ başa dön](#iindekiler)**

## SOLID

### Tekil Sorumluluk `(Single Responsibility)` Prensibi

Tekil Sorumluluk yani `Single Responsibilty` bir nesnenin `(class)` sadece tek bir amaca hizmet etmesini, tek bir amacı gerçekleştirmesini, tek bir işin yönetiminden sorumlu
olmasını ifade eder.

Bir nesnenin birden fazla işin yönetiminden sorumlu olması, örneğin, alış veriş ürünlerinin tutan Card nesnesinin aynı zamanda ödeme bilgileri tutması ve ödeme 
işlemi gerçekleştirebilmesi Tekil Sorumluluk `(Single Responsibility)` ilkesine aykırılık teşkil edecektir.

Bir nesne `(class)` oluştururken bu nesnenin kapsam ve görevleri, daha doğrusu ne tür bir amaca hizmet edeceği, bu nesnenin konsepti daha nesne oluturulmadan önce 
net bir şekilde *(fiziken veya mental olarak)* çizilmelidir. 

**Yanlış:**

```ts
class UserSettings {
  constructor(private readonly user: User) {
  }

  changeSettings(settings: UserSettings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Doğru:**

```ts
class UserAuth {
  constructor(private readonly user: User) {
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  private readonly auth: UserAuth;

  constructor(private readonly user: User) {
    this.auth = new UserAuth(user);
  }

  changeSettings(settings: UserSettings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ başa dön](#iindekiler)**

### Açık/Kapalı `(Open Closed)` Prensibi

Bertrand Meyer tarafından ifade edildiği üzere;

> Yazılım mimarısında kullanılan varlıklar (nesneler, modüller, fonksiyonlar ...) düzenlemeye **kapalı** ancak her daim genişletmeye **açık** olmalıdır

Bertrand Meyer'in burada demek istediği yazdığınız bir nesne veya modül gelişime açık olmalı, bu nesnelere yeni özellikler, yeni işlevler, 
yeni metodlar eklenebilmeli ancak var olan kodlar değiştirilmemeli *(genişletilebilmeli)*, mevcut kodların çalışma düzeni zarar 
görmeden bu genişletme işlemi yapılabilmelidir. 

**Yanlış:**

```ts
class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    if (this.adapter instanceof AjaxAdapter) {
      const response = await makeAjaxCall<T>(url);
    } else if (this.adapter instanceof NodeAdapter) {
      const response = await makeHttpCall<T>(url);
    }
  }
}

function makeAjaxCall<T>(url: string): Promise<T> {
}

function makeHttpCall<T>(url: string): Promise<T> {
}
```

**Doğru:**

```ts
abstract class Adapter {
  abstract async request<T>(url: string): Promise<T>;
}

class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    const response = await this.adapter.request<T>(url);
    // transform response and return
  }
}
```
Eğer koşula dayalı bir yapı kullanırsak her yeni adapter eklediğimizde, `HttpRequester.fetch` metodunu düzenlemek zorundayız. Ancak
Açık/Kapalı prensibinde kabul ettiğimiz üzere biz yeni bir adapter eklediğimizde mevcut kod üzerinde bir değişim olmasını istemiyoruz. 

Dolayısıyla en başta bizim sonradan yaptığımız eklemelerden etkilenmeyecek bir yapı kurmamız gerekiyor. Yukarıda yer alan örnekte sürücü
dizaynı `(Driver Pattern)` ile oluşturulmuş bir yapı söz konusu. Bu yapı sayesinde 40 farklı adapter dahi olsa, `Adapter` abstract
nesnesinde yer alan gereken nitelikleri sağladığı sürece, mevcut kodumuzda hiçbir düzenleme yapmaksızın kullanabiliriz. 

**[⬆ başa dön](#iindekiler)**

### Yerine Geçme `(Liskov Substitution)` Prensibi

Kabaca tabiriyle kalıtım alan ve kalıtım veren sınıfların hiç bir ekstra müdahele olmaksızın birbirlerinin yerine kullanılabilmesidir.
*Yerine Geçme* prensibi olarak da geçen **Liskov Substitution** prensibinde amaç kalıtım veren `(parent)` nesnenin bütün kalıtım alan
`(child)` nesneleri kapsayacak şekilde oluşturulması, bu sayede kalıtım alan sınıflar `(child)` ile çalışırken mevcut kod yapısının
dışına çıkmadan, if else devreleri kurmak zorunda kalmadan kalıtım aldığı işlemleri yerine getirebilmesidir. 

Kalıtım alan çocuk nesne `(child)` kalıtım aldığı `(parent)` nesnenin soyut `(abstract)` ve somut bilgilerini, metodlarını kullanarak
kendisinden beklenen işlevi yerine getirebilmelidir. Kalıtım veren `(parent)` ne gereğinden az (çocuk nesneleri kendi ortamını kurmaya 
zorlamadan), ne de gereğinden fazla (çocuk nesnelerin ihtiyaç duymayacağı) bilgi-metod içermelidir. 

**Yanlış:**

```ts
// Rectangle: Dikdörtgen
class Rectangle {
  constructor(
    protected width: number = 0,
    protected height: number = 0) {

  }

  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  setWidth(width: number): this {
    this.width = width;
    return this;
  }

  setHeight(height: number): this {
    this.height = height;
    return this;
  }

  getArea(): number {
    return this.width * this.height;
  }
}
// Square: Kare
class Square extends Rectangle {
  setWidth(width: number): this {
    this.width = width;
    this.height = width;
    return this;
  }

  setHeight(height: number): this {
    this.width = height;
    this.height = height;
    return this;
  }
}

function renderLargeRectangles(rectangles: Rectangle[]) {
  rectangles.forEach((rectangle) => {
    const area = rectangle
      .setWidth(4)
      .setHeight(5)
      .getArea(); // Hatalı çalışacak ve kareyi dikdörtgen olarak renderleyecektir.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Doğru:**

```ts
// Share: Şekil
abstract class Shape {
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  abstract getArea(): number;
}

class Rectangle extends Shape {
  constructor(
    private readonly width = 0,
    private readonly height = 0) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private readonly length: number) {
    super();
  }

  getArea(): number {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes: Shape[]) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ başa dön](#iindekiler)**

### Arayüz Ayrımı `(Interface Segregation)` Prensibi

`Interface Segregation` prensibi şu şekilde tanımlanır: *Geliştiriciler hiç kullanmayacakları arayüzleri `(Interface)` kullanmak 
zorunda bırakılmamalı.*

Burada anlatılmak istenen aslında yukarıda `Single Responsibility` prensibinde anlatılandan çok da farklı değildir. Bu prensipte ise
nesnelerin yerine arayüzlerin `(interface)` kapsamının daha dar, daha **öz** olması gerektiğinden, bir arayüzün kapsamı dışında kalan metodları
tanımlamamasından bahseder.

Bu sayede bir arayüzü `(interface)` uygulayan `(implement eden)` geliştiricinin hiç ihtiyacı olmadığı halde bu metodları, sadece 
arayüzde tanımlandığı için oluşturduğu sınıfa uygulamak zorunda bırakılmaması amaçlanır.

**Yanlış:**

```ts
interface SmartPrinter {
  print();
  fax();
  scan();
}

class AllInOnePrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    throw new Error('Fax not supported.');
  }

  scan() {
    throw new Error('Scan not supported.');
  }
}
```

**Doğru:**

```ts
interface Printer {
  print();
}

interface Fax {
  fax();
}

interface Scanner {
  scan();
}

class AllInOnePrinter implements Printer, Fax, Scanner {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements Printer {
  print() {
    // ...
  }
}
```

**[⬆ başa dön](#iindekiler)**

### Bağımlılığın Ters Çevrilmesi `(Dependency Inversion)` Prensibi (DIP)

Bu prensip 2 temel kural üzerine kurulmuştır:

1. Yüksek seviye modüller, nesneler alt seviye modüllere dayanmamalıdır. Bu bağımlılık soyutlanmalıdır.

2. Soyutlama işlemi alt seviye bağımlılığın detaylarına dayanmamalıdır. Alt seviye nesnenin sorumluluğu soyutlanmalı, ancak bu
sorumluluğu alt seviye nesnenin nasıl yerine getirileceği onun insifiyatine bırakılmalı, üst seviye nesne veya modül alt nesnenin 
bu sorumluluklarından bağımsız hareket edebilmelidir. 

Okunduğunda çok fazla bir anlam ifade etmiyor gibi görünebilir ancak aslında burada anlattığımız bu prensibi biraz önce yukarıda
hatalı bulduğumuz bir örneği düzenlemek yoluyla kullandık.

Hatırlarsanız `EmployeeTaxData` sınıfını `setTaxData` metodu içerisinde oluşturan *(örnekleyen)* `Employee` sınıfını düzenlemiştik
ve bu sınıfın doğrudan `EmployeeTaxData` sınıfına bağımlı bırakılmaması gerektiğini, bunun yerine `getTaxData` gibi soyut bir 
implementasyonu yerine getiren herhangi bir sınıfla ile çalışabilmesi gerektiğinden bahsetmiştik. 

Burada aslında yaptığımız düzenleme ile `Dependency Inversion` prensibi ile bizden beklenen üst nesnenin alt nesneye bağımlı olmaktan
kurtulmasından başkaca bir şey değil. Bunun yerine biz `Employee` nesnesini `EmployeeTaxData` sınıfından kurtarak `TaxData` arayüzünü
`(interface)` sağlayan herhangi bir sınıfla çalışabilecek hale getirdik. 

**Yanlış:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

class XmlFormatter {
  parse<T>(content: string): T {
    // Xml veriyi generic bir sınıfa dönüştürür.
  }
}

class ReportReader {

  // Burada yaptığımız işlem ile ReportReader nesnesini doğrudan
  // XmlFormatter sınıfına bağımlı hale getirmiş olduk
  private readonly formatter = new XmlFormatter();

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader();
await report = await reader.read('report.xml');
```

**Doğru:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

interface Formatter {
  parse<T>(content: string): T;
}

class XmlFormatter implements Formatter {
  parse<T>(content: string): T {
    // Xml veriyi generic bir sınıfa dönüştürür
  }
}


class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // Xml veriyi generic bir sınıfa dönüştürür
  }
}

class ReportReader {
  constructor(private readonly formatter: Formatter) {
  }

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader(new XmlFormatter());
await report = await reader.read('report.xml');

const reader = new ReportReader(new JsonFormatter());
await report = await reader.read('report.json');
```

Burada yaptığımız soyutlama işlemi sayesinde ReportReader nesnemiz artık spesifik bir Formatter nesnesine bağımlı olmadan çalışabilecek,
`Formatter` arayüzünde istenen implementasyonları yerine getiren herhangi bir nesne ile çalışabilecektir. 

**[⬆ başa dön](#iindekiler)**

## Testing

Testing yazılım geliştirmenin en önemli parçalarından biridir. Eğer hiç test yazılmamış ise veya yetersiz sayıda test yazılmış ise, yazdığınız kodu her yayınladığınızda
uygulamada bir hata oluşup oluşmayacağından asla tam olarak emin olamazsınız. Bu belki geliştirme sürecinde olan uygulama için ciddi bir sorun teşkil etmeyebilir ancak
10 milyon kullanıcı tarafından kullanılmakta olan bir uygulama düşünün ve yayınladığınız yeni bir özelliğin veya yaptığınız bir değişikliğin 10 milyon kişiyi etkilediğini,
kodunuzda yanlış çalışan bir bölüm olduğu veya mevcut kodla bir çakışma meydana getirdiği bir senaryoda milyonlarca müşterinin mağduriyetine yol açacaktır.

[Javascript için yaazılmış test `framework`leri](http://jstherightway.org/#testing-tools) 

Bu `framework`ler ayrıca typescript desteği de sunmaktadır.

### Test Öncelikli Geliştirme `(Test Driven Development (TDD))`

`TDD` olarak bilinen bu yöntem, henüz kod yazmaya başlamadan önce kodunuz soyut çalışma mantığında önce testleri yazmayı, sonrasında ise uygulamanızı çalıştıracak
olan kodların yazılmasını amaçlar. `TDD` için belirlenmiş üç ana kural vardır

1. Test süresince kod yazılmaz, test yazım aşaması bittikten sonra kod yazım aşamasına geçilir.

2. Birim `(Unit)` test kısa ve öz olmalı, beklenenin gerçekleşmemesi halinde başarısızlıkla `(fail)` sonuçlanmlıdır. Gereğinden uzun birim test yazılmamalıdır. 

3. Test ile sınırları çizilenden daha fazla kod yazmayın, yazdığınız her kod parçası kendisi için yazılan birim testi başarı ile sonuçlandıracak kadar olmalıdır.

**[⬆ başa dön](#iindekiler)**

### F.I.R.S.T. kuralları

Temiz *(açık)* `(Clean)` bir test yazmak için aşağıda belirtilen kurallar takip edilmelidir:

- **Fast** Testler sık sık yazılan kodları kontrol etmek için kullandığından olabildiğince hızlı bir şekilde çalışmalıdır.

- **Independent** Bir test başka bir teste dayanmamalı, izole olmalıdır. Testlerin hangi sırayla çalıştırıldığı önem arz etmemelidir.

- **Repeatable** Testler farklı farklı ortamlarda aynı bir şekilde çalışabilmelidir. Çalıştırıldığı ortamdan bağımsız olmalıdır.

- **Self-Validating** Bir test başarılı ise *Passed* şeklinde, başarısız ise *Failed* şeklinde cevap vermelidir. Bir testing başarılı mı yoksa başarısız mı
sonuçlandığını anlamak için log kayıtları incelenmek zorunda kalınmamalı.

- **Timely** Birim `(Birim)` testleri kod yazmaya başlamadan önce yazılmalıdır. Mevcut kod için test yazmak zor gelebilir.

**[⬆ başa dön](#iindekiler)**

### Her testing tek bir konsepti olmalı.

Aynı nesneler gibi testlerde Tekil Sorumluluk `(Single Responsibility)` Prensibine uymalıdır. Bir test için tek bir doğrulama `(assertion)` yapılmalıdır..

**Yanlış:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles date boundaries', () => {
    let date: AwesomeDate;

    date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));

    date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));

    date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**Doğru:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles 30-day months', () => {
    const date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));
  });

  it('handles leap year', () => {
    const date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));
  });

  it('handles non-leap year', () => {
    const date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**[⬆ başa dön](#iindekiler)**

### Testing adı yaptığı işlevi açık bir şekilde ortaya koymalıdır.

Bir test başarısız olduğunda sadece testing ismine bakılarak hangi işlemin başarısız olduğu anlaşılabilmeli, kaynak kodlar incelenmek zorunda bırakılmamalıdır.

**Yanlış:**

```ts
describe('Calendar', () => {
  it('2/29/2020', () => {
    // ...
  });

  it('throws', () => {
    // ...
  });
});
```

**Doğru:**

```ts
describe('Calendar', () => {
  it('should handle leap year', () => {
    // ...
  });

  it('should throw when format is invalid', () => {
    // ...
  });
});
```

**[⬆ başa dön](#iindekiler)**

## Asenkron İşlemler

### `Callback` yerine `Promise` kullanın

`Callback` kullanılan yapılar özellikle basamaklı işlemler birden fazla `Callback` kullanmanızı gerektirecek durumlarda hem kullanım `(usability)` hem de okunabilirlik
 `(readibility)` açısından sıkıntılı bir hal almaktadır. Bu geliştiriciler arasında `callback cehennemi` `(callback hell)` olarak da adlandırılır.
  
Bunun yerine ES6 ile birlikte gelen ve kısa zamanda yaygın bir kullanım kazanan `Promise` yapısını kullanabilirsiniz. 

[es6-promisify](https://www.npmjs.com/package/es6-promisify))

**Yanlış:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';

function downloadPage(url: string, saveTo: string, callback: (error: Error, content?: string) => void) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html', (error, content) => {
  if (error) {
    console.error(error);
  } else {
    console.log(content);
  }
});
```

**Doğru:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url)
    .then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

`Promises` nesnesi `(class)` bir takım yardımcı metodlar içermektedir.  

|---------------------------------------------------------------------------------------|  
| Method                   | Açıklama                                                   |   
| ------------------------ | -----------------------------------------------------------|  
| `Promise.resolve(value)` | Girilen değeri bir `Promise` olarak çözümler.              |  
| ------------------------ | -----------------------------------------------------------|  
| `Promise.reject(error)`  | Girilen değeri `Promise` hatası olarak çözümler.           |  
| ------------------------ | -----------------------------------------------------------|  
| `Promise.all(promises)`  | Girilen bütün `Promise`ler çözümlendikten sonra            |
|                          | hepsini birlikte bir dizi `(array)` olarak döndürür.       |
|                          | Eğer bir `Promise` hata olarak `(rejected)` çözümlenirse   |
|                          | bütün `Promise`ler hatalıymış gibi muamele görür           |
| ------------------------ | -----------------------------------------------------------|  
| `Promise.race(promises)` | Girilen bütün `Promise`ler den herhangi biri tamamlandığı  | 
|                          | anda `Promise` tamamlanır. Geri kalan `Promise`ler         |
|                          | çözümlenmez                                                |
|---------------------------------------------------------------------------------------|  

`Promise.all` genellike eşzamanlı *(birbirine paralel)* işlemlerde kullanılır. 

**[⬆ başa dön](#iindekiler)**

### Async/Await kullanarak daha temiz `(clean)` yapılar oluşturun

`async`/`await` *syntax*'ı ile birlikte daha zincirleme yapılara kıyasla daha temiz daha anlaşılır kod yazabiliriz. `async` anahtar kelimesi ile birlikte çalışma 
zamanında `(runtime)` kodu okuyan javascript motoruna burada asenkron bir işlem gerçekleşeceğini, `await` anahtar kelimesi ile birlikte ise burada gerçekleşek olan
işlemin anlık olarak gerçekleşmeyeceğini, bu nedenle tamamlanmasının beklememesini, bu işlemi sıraya alarak aşağıda yer alan kodların okunmaya devam etmesini kodumuzu
okuyan javascript motora `(enginee, örn. v8)` söyleriz.

**Yanlış:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = util.promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

**Doğru:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

async function downloadPage(url: string, saveTo: string): Promise<string> {
  const response = await get(url);
  await write(saveTo, response);
  return response;
}

// async diğer fonksiyonun içerisinde ise
try {
  const content = await downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html');
  console.log(content);
} catch (error) {
  console.error(error);
}
```

**[⬆ başa dön](#iindekiler)**

## Hata Yönetimi (Error Handling)
Hata almak, hata vermek bunlar güzel şeyler. Bir hata aldığınız yazığınızda bir şeylerin ters gittiğini, bazı şeylerin doğru çalışmadığını hemen anlarız. Ancak 
hata almadığımız bir senaryoda, çalışması gerektiği gibi çalışmayan bu kod parçasından haberimiz olmayacak, belki yayına çıktıktan ve belli bir maddi zarara sebep
olduktan sonra, harcayacağımız saatlerce `debugging` işleminden sonra tespit edebileceğiz. 

Bu yüzden bir uygulama yazarken, eğer bir aksiyonda istenmeyen bir durum oluşma, daha doğru bir ifadeyle, hataya sebebiyet verecek bir halin varlığı söz konusu ise
bu noktaları gerek `try..catch` gerek farklı yapılar kullanarak, bu hataları almak ve bildirmek geliştirme sürecinde büyük bir kolaylık sağlayacaktır.

### Bir hata bildirmek istediğinizde her zaman `Error` nesnesini fırlatın `(throw)` 

Javascript'te olduğu gibi Typescript'te bir objeyi fırlatmanıza `(throw)` imkan verir. Keza Promise nesnesi de herhangi bir objeyi hata olarak 
çözümlemenize `(reject)` imkan tanır. 

Javascript'te hata yönetiminde `Error` sınıfını `throw` anahtar sözcüğü ile birlikte kullanıyoruz. `throw` anahtar kelimesi, adından da anlaşılacağı üzere
bir objeyi fırlatmanıza olanak sağlar, bu sayede, bir üst kod segmesinde yer alan `try catch` bloğunda fırlatılan objeyi yakalayabiliriz. 

İstenmeyen bir durum ile karşılaştığımızda hata sınıfını ve `throw` anahtar kelimesini kullanarak, oluşan hatalar ile ilgilenen `catch` bloğuna bu hatayı iletebilir
ve istenmeyen durumun gerçekleştiğini kayıt altına alabilir, ve eğer mümkünse, hataya sebebiyet veren olayın oluşturduğu olumsuz sonuçları önlemeye çalışabiliriz.

[Hata yönetimi hakkında](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).  
**Yanlış:**

```ts
function calculateTotal(items: Item[]): number {
  throw 'Not implemented.';
}

function get(): Promise<Item[]> {
  return Promise.reject('Not implemented.');
}
```

**Doğru:**

```ts
function calculateTotal(items: Item[]): number {
  throw new Error('Not implemented.');
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error('Not implemented.'));
}

// or equivalent to:

async function get(): Promise<Item[]> {
  throw new Error('Not implemented.');
}
```

Hata yönetiminde `Error` sınıfını kullanmanın en önemli avantajlarından biri de sahip olduğu `stack` adlı *property*'si sayesinde hata meydana gelmeden hangi satır
hangi sütünda hangi işlemlerin gerçekleştiğini, hangi metodların çalıştırıldığını görebilir ve hatanın kaynağını çok daha kolay bir şekilde tespit edebiliriz.

`throw` anahtar kelimesine alternatif olarak, Typescript ile ile kod yazarken daha uyumlu bir şekilde çalışabilecek şekilde türü `(type)` belirli hata sınıfları
oluşturabilir, bu hataları oluşturduğumuz metodlarda, kullanabiliriz. Bu sayede bu fonksiyonu veya metodu kullanacak olan kişilere bu metodun kullanımı sırasında
beklenmeyen bir durumun oluşabileceğini, bu sayede, gelen yanıtı kontrol etmesi gerektiğini bildirmiş oluruz.

```ts
type Result<R> = { isError: false, value: R };
type Failure<E> = { isError: true, error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, 'empty'> {
  if (items.length === 0) {
    return { isError: true, error: 'empty' };
  }

  // ...
  return { isError: false, value: 42 };
}
```

Daha detaylı bilgi için [Making exceptions type safe in Typescript](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).

**[⬆ başa dön](#iindekiler)**

### Yakalanan hataları ihtmal etmeyin.

Hata fırlatmak hata yönetiminin sadece bir yüzüdür, eğer fırlatılan hatayı yakalayıp değerlendirmiyorsak bu hatayı ne tespit etmenin ne fırlatmanın bir anlamı 
kalmayacaktır. Geliştirme sırasında hatayı konsola yazdırmak ihtiyacımızı büyük bir ölçüde karşılayabilir, ancak, yayında kaynaklanan hataların konsola yazdırılması
bize hiç bir fayda sağlamayacaktır. Bu yüzden yakalanan hataların kayıt altına alması için bir `logger` yapısına ihtiyaç duyuyoruz. 

Bu `logger` yapısı ile, eğer tarayıcı tarafından çalışıyorsak api üzerinden backend'e iletebilir, eğer sunucu tarafında çalışıyorsak `(node)` doğrudan hatayı kayıt
altına alabiliriz.

**Yanlış:**

```ts
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}

// or even worse

try {
  functionThatMightThrow();
} catch (error) {
  // ignore error
}
```

**Doğru:**

```ts
import { logger } from './logging'

try {
  functionThatMightThrow();
} catch (error) {
  logger.log(error);
}
```

**[⬆ başa dön](#iindekiler)**

### Hata olarak `(reject)` çözümlenen `Promise`leri ihmal etmeyin

Yukarıda throw ile fırlattığımız hatalarda olduğu gibi, `Promise` ile hata olarak çözümlediğimiz beklenmeyen olayları da kayıt altına almamız gerekmektedir.

**Yanlış:**

```ts
getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    console.log(error);
  });
```

**Doğru:**

```ts
import { logger } from './logging'

getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    logger.log(error);
  });

// eğer async/await kullanıyorsak:

try {
  const user = await getUser();
  await sendEmail(user.email, 'Welcome!');
} catch (error) {
  logger.log(error);
}
```

**[⬆ başa dön](#iindekiler)**

## Yazım Kuralları (Formatting)

Yazım kuralları kişiden kişiye değişkenlik gösterebilir. Typescript yazımında herkesin zorunlu olarak uyması gereken zorunlu bir `yazım kuralları listesi` yoktur.
Bu kurallar daha çok bir ekip halinde çalışırken, gerek açık kaynak kütüphanelerde, gerek ekip arkadaşlarınız ile çalışırken ortak bir standart oluşturmak
bu sayede farklı farklı kişilerin yazdığı kodlar arasında bir uyum oluşturmak amacıyla belirlenen ortak standartlardan başka bir şey değildir. 

Günümüzde bu standartları otomatik olarak uygulamak için çeşitli, konfigurasyon sistemleri geliştirilmiş olup `IDE`ler bu konfigurasyonları okuyup anlayabilmekte
ve yazdığınız kodu bu konfigurasyonlara formatlayabilmektedir. Bu konfigurasyonlarda belirlenen standartlara aykırı bir şekilde yazıyorsanız ise sizi uyarmaktadır.
 
Typescript için bu konfigurasyonları oluşturmak amacıyla [TSLint](https://palantir.github.io/tslint/) geliştirilmiştir. TSLint belirlediğiniz konfigurasyonlar
doğrultusunda kodunuzu tarayarak, standartlara aykırı olarak yazılmış noktaları tespit eder. 

Bu yazım standartlarını manuel olarak oluşturabileceğiniz gibi, daha önceden yazılmış olan ve typscript ile geliştiren bir çok kimse tarafından kullanılan hazır
konfigurasyonları da kullanabilirsiniz.

- [TSLint Config Standard](https://www.npmjs.com/package/tslint-config-standard)
- [ESLint Config Google](https://www.npmjs.com/package/https://www.npmjs.com/package/eslint-config-google)
- [TSLint Config Airbnb](https://www.npmjs.com/package/tslint-config-airbnb)
- [TSLint Clean Code](https://www.npmjs.com/package/tslint-clean-code) 
- [TSLint React](https://www.npmjs.com/package/tslint-react)
- [TSLint + Prettier](https://www.npmjs.com/package/tslint-config-prettier) 
- [ESLint rules for TSLint](https://www.npmjs.com/package/tslint-eslint-rules)
- [Immutable](https://www.npmjs.com/package/tslint-immutable)

ÇN: Şöyle bir durum var. TSLint kendi dökümanına göre kullanımı durdurulmuş `(deprecated)` görünüyor. `Eslint` altında `eslint-typescript` yardımcı kütüphanesi
ile birlikte kullılacağı bildirilmiş. 

### Büyük harf kullanımında sabit bir teknik izleyin.

Değişken, sabit, method, fonksiyon, nesne, obje, dizi isimlendirirken her zaman sabit bir teknik kullanın. Örneğin sınıfları tanımlarken büyük harf ile başlayıp küçük harf ile
devam ediyorsanız, oluşturduğunuz bütün sınıflarda bu tekniği kullanın, metodlarınızı isimlendirirken farklı kelimeleri birleştirmek için `camelCase` kullanıyorsanız
oluşturduğunuz bütün fonksiyonlarda ve metodlarda bu adımı izleyin. 

Bu sayede yazdığınız kodları kullanırken hem siz, hem de ekip arkadaşlarınız sorun yaşamaz. Yazılmış olan bir değişkeni kullanırken her seferinde büyük harfle mi yoksa
küçük hafle mi, yahut `camelCase` mi `snake_case` mi kullanacağı karmaşasına düşmez. Yazılmış olan kodları tekrar tekrar kontrol etme ihtiyacı duymaz.

**Yanlış:**

```ts
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

type animal = { /* ... */ }
type Container = { /* ... */ }
```

**Doğru:**

```ts
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = { /* ... */ }
type Container = { /* ... */ }
```

Genel olarak javascript geliştiren toplulukta kabul edilen ve kullanılan bir takım yazım kuralları vardır.
,
1. Nesne `(class)`, arayüz `(interface)` ve namespace tanımlarken her zaman `PascalCase` kullanın.
2. Değişkenler, fonksiyonlar, metodlar ve `property`leri isimlendirirken `camelCase` kullanın

**[⬆ başa dön](#iindekiler)**

### Fonksiyon tanımlamaları ve bu fonksiyon kullanıldığı yerler olabildiğince birbirine yakın olmalı.

Burada aslında değerlendirilmesi gereken birden fazla durum var. Eğer bu durum bir sınıf `(class)` içerisinde gerçekleşiyorsa, örneğin bir metod içerisinde, 
sınıf içerisinde tanımladığınız diğer metodları çağırıyorsanız bu metodu çağırdığınız metodların yakınına yerleştirin. Hatta daha da ideal olarak bu metodu
çağırdığınız bu metodların en üstüne yerleştirin.


Ancak sınıf dışında tanımladığınız yardımcı metodlarınız varsa ve uygulama genelinde bu yardımcı metodları kullanıyorsanız bu metodları `helpers.ts` veya `utils.ts`
adı altında ayrı bir dosya içerisinde tutmanız daha sağlıklı olacaktır. 

**Yanlış:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**Doğru:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  // sınıf içerisinde herhangi bir noktada değil,
  // çağırdığımız metodların üzerinde
  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**[⬆ başa dön](#iindekiler)**

### `Import` kullanımında dikkat edilmesi gerekenler

Modüler programlamanın gelişmesiyle birlikte import ve export kullanımları yazdığımız kodların bir parçası olmaya başladı. Özellikle modüle veya main javascript
dosyalarımızda onlarca import kullanmak durumunda kalabiliyoruz. İmport kullanımlarını bir düzene oturtmak, hem kullanım `(usability)` hem de okunurluk `(readibility)`
açısından bize büyük bir kazanç sağlar. 

- Import ifadeleri gruplanmalı ve alfabetik olarak bir sıralanmalı.
- Kullanılmayan import ifadeleri temizlenmeli.
- Import ifade içerisinde birden fazla import varsa alfabetik olarak sıralanmalı (örn. `import {A, B, C} from 'foo';`)
- Import grupları, import edilen kaynağa göre alfabetik olarak sıralanmalı, örn.: `import * as foo from 'a'; import * as bar from 'b';`
- Import grupları arasında boş bir satır bırakılmalı.
- Import Grupları aşağıda yer aldığı gibi sıralanmalı:
  - Polifiller `(Polyfills)` (örn. `import 'reflect-metadata';`)
  - Native Node modülleri `(Node builtin modules)` (örn. `import fs from 'fs';`)
  - Harici modüller *(npm, bower)* (örn. `import { query } from 'itiriri';`)
  - Relative modüller (örn. `import { UserService } from 'src/services/userService';`)
  - Bir üst dizinden çağrılan modüller (örn. `import foo from '../foo'; import qux from '../../foo/qux';`)
  - Aynı dizinden çağrılan modüller (örn. `import bar from './bar'; import baz from './bar/baz';`)

**Yanlış:**

```ts
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Doğru:**

```ts
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```

**[⬆ başa dön](#iindekiler)**

### Uzak relative dosyalar için takma isimler `(alias)` kullanın
`tsconfig.json` içerisinde yer alan `compilerOptions` bölümünde `paths` konfigurasyonunu kullanarak bir dizin `(directory)` için takma ad oluşturabilirsiniz.

Typescript `paths` içerisinde tanımladığınız dizinleri `baseUrl` ile belirttiğiniz dizinin altında tespit ederek, belirlediğiniz takma ad `(alias)` ile özleştirir.
Yazdığınız typescript kodunu compile ederken bu `alias` ile karşılaştığında ise bu değeri relative yol ile değiştirir.

**Yanlış:**

```ts
import { UserService } from '../../../../core/services/UserService';
```

**Doğru:**

```ts
/*
tsconfig.json

{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "@core/*": ["app/core/*"]
    }
  }
}
*/

import { UserService } from '@core/services/UserService';
```

**[⬆ başa dön](#iindekiler)**

## Yorumlar `(Comments)`

Bir kod bloğu içerisinde gerçekleşen işlemi açıklamak yerine, önceliğiniz kodun kendisini, kodu okuyan kimse için anlaşılacak şekilde yazmakdır. 

> Kötü yazılmış koda açıklık getirmek için yorum yazmayın, kötü bir şekilde yazdığınız kodu, yeniden, daha iyi bir şekilde yazın. 
> — *Brian W. Kernighan and P. J. Plaugher*

### Yorum yazmak yerine, okunduğunda anlaşılacak kodlar yazmaya özen gösterin.

Yorumlar bir zorunluluk değil, kötü yazılmış bir kod için mazeretten ibarettir. İyi bir şekilde yazılmış kodu anlamak için yorum gerekmez.

**Yanlış:**

```ts
// Subscription aktif mi değil mi kontrol et
if (subscription.endDate > Date.now) {  }
```

**Doğru:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) { /* ... */ }
```

**[⬆ başa dön](#iindekiler)**

### Kullanımdan kaldırdığınız kodu yoruma almayın. Doğrudan silin.

Versiyon kontrol sistemleri ile çalıştığımız için *(github, bitbucket)*, bir uygulama içerisinde yazılan, değiştirilen, silinen bütün kodlara ve tarihçesine
ayrıntılı bir şekilde sahibiz. Bu nedenle, eğer kodunuzda kullanımdan kaldırılan bir bölümün olması durumunda, bu alanı yorum almak yerine doğrudan kodunuzdan kaldırın.
Gerekli olması halinde kaldırılan kodlara `VSC` sistemi üzerinde bulunan `repository`'nizden ulaşabilirsiniz.

**Yanlış:**

```ts
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
}
```

**Doğru:**

```ts
type User = {
  name: string;
  email: string;
}
```

**[⬆ başa dön](#iindekiler)**

### Yazdığınız koda işlem kaydı eklemeyin.
Versiyon kontrol sistemleri ile çalışırken yaptığımız değişiklikleri, açıklayıcı bir cümle ile `commit` ettiğimiz için, yapılan işlemlerin kaydı
`VCS` üzerinde kayıt altında tutulmaktadır ve `git log` diyerek bu kayıtlara kolayca ulaşılabilir.

Bu nedenle yazdığınız kod üzerinde yaptığınız her değişiklikte yapılan işleme ait bilgiler içeren bir yorum eklemenizin hiç bir faydası yoktur. Yazdığınız kod
içerisinde var olan işlem kayıtlarını, kullanımdan kaldırılan kodları temizleyin ve çalışma ortamınızı olabildiğince gereksiz bilgilerden ve detaylardan arındırarak
temiz bir çalışma ortamı `(codebase)` oluşturun.

**Yanlış:**

```ts
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**Doğru:**

```ts
function combine(a: number, b: number): number {
  return a + b;
}
```

**[⬆ başa dön](#iindekiler)**

### Bir segmenti veya bloğu belli etmek için yorum blokları oluşturmayın.
Bu tür yorum blokları görüntü kirliliğinden başka bir şey değildir. Günümüzde bir çok ide kod bloklarını gizleyip `(collapse)`, genişletebilir `(expand)`.
Dolayısı artık bu tür yorum bloklarının işlevselliği kalmamıştır.

**Yanlış:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Client class
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public methods
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private methods
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**Doğru:**

```ts
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**[⬆ başa dön](#iindekiler)**

### TODO yorumlarını kullanın
Bazen yazdığınız kodu sonradan değiştirmek, güncellemek veya düzenlemek isteyebilirsiniz. Bu gibi durumlarda bir kenara not almak yerine `// TODO` *syntax*'ını kullanarak
sonradan yapılacak olan işlemi yazabilirsiniz.

Günümüzde bir çok IDE bu syntaxı anlamakta ve çalışma ortamı `(codebase)` içerisinde yer alan bütün `TODO` yorumlarını bir araya getirerek listelemektedir.

Eğer yazdığınız kodun yeniden geliştirilmeye ve düzenlemeye ihtiyaç duyduğunu düşürüyorsanız. Ayrıca bir sistem üzerinde not tutmak yerine kod bloğunun hemen üst satırına
`// TODO ...` yorumu yazabilirsiniz. 

**Yanlış:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // 'dueDate''nin indexlendiğini kontrol et
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Doğru:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO 'dueDate''nin indexlendiğini kontrol et
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ başa dön](#iindekiler)**
