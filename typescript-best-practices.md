## Table of Contents

  1. [Değişkenler](#deikenler)
  2. [Fonksiyonlar](#functions)
  3. [Objeler ve Veri Tipleri](#objects-and-data-structures)
  4. [Sınıflar](#classes)
  5. [SOLID](#solid)
  6. [Testing](#testing)
  7. [Concurrency](#concurrency)
  8. [Hata Yönetimi](#error-handling)
  9. [Düzenleme](#formatting)

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

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
  private taxData: EmployeeTaxData;

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

**[⬆ başa dön](#table-of-contents)**

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

**[⬆ başa dön](#table-of-contents)**

## SOLID

### Tekil Sorumluluk `(Single Responsibility)` Prensibi (SRP)

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

**[⬆ başa dön](#table-of-contents)**

### Açık/Kapalı Prensibi `(Open Closed Princible)`

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
dizaynı `(Driver Pattern)` ile oluşturulmuş bir yapı söz konusu. Bu yapı sayesinde biz 40 farklı adapter dahi 

**[⬆ başa dön](#table-of-contents)**

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S is a subtype of T, then objects of type T may be replaced with objects of type S (i.e., objects of type S may substitute objects of type T) without altering any of the desirable properties of that program (correctness, task performed, etc.)." That's an even scarier definition.  
  
The best explanation for this is if you have a parent class and a child class, then the parent class and child class can be used interchangeably without getting incorrect results. This might still be confusing, so let's take a look at the classic Square-Rectangle example. Mathematically, a square is a rectangle, but if you model it using the "is-a" relationship via inheritance, you quickly get into trouble.

**Yanlış:**

```ts
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
      .getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Doğru:**

```ts
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

**[⬆ başa dön](#table-of-contents)**

### Interface Segregation Principle (ISP)

ISP states that "Clients should not be forced to depend upon interfaces that they do not use.". This principle is very much related to the Single Responsibility Principle.
What it really means is that you should always design your abstractions in a way that the clients that are using the exposed methods do not get the whole pie instead. That also include imposing the clients with the burden of implementing methods that they don’t actually need.

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

**[⬆ başa dön](#table-of-contents)**

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.

2. Abstractions should not depend upon details. Details should depend on abstractions.

This can be hard to understand at first, but if you've worked with Angular, you've seen an implementation of this principle in the form of Dependency Injection (DI). While they are not identical concepts, DIP keeps high-level modules from knowing the details of its low-level modules and setting them up. It can accomplish this through DI. A huge benefit of this is that it reduces the coupling between modules. Coupling is a very bad development pattern because it makes your code hard to refactor.  
  
DIP is usually achieved by a using an inversion of control (IoC) container. An example of a powerful IoC container for TypeScript is [InversifyJs](https://www.npmjs.com/package/inversify)

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
    // Converts an XML string to an object T
  }
}

class ReportReader {

  // BAD: We have created a dependency on a specific request implementation.
  // We should just have ReportReader depend on a parse method: `parse`
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
    // Converts an XML string to an object T
  }
}


class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts a JSON string to an object T
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

// or if we had to read a json report
const reader = new ReportReader(new JsonFormatter());
await report = await reader.read('report.json');
```

**[⬆ başa dön](#table-of-contents)**

## Testing

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything.
Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches)
is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a good [coverage tool](https://github.com/gotwarlost/istanbul).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools) with typings support for TypeScript, so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.  

### The three laws of TDD

1. You are not allowed to write any production code unless it is to make a failing unit test pass.

2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.

3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

**[⬆ başa dön](#table-of-contents)**

### F.I.R.S.T. rules

Clean tests should follow the rules:

- **Fast** tests should be fast because we want to run them frequently.

- **Independent** tests should not depend on each other. They should provide same output whether run independently or all together in any order.

- **Repeatable** tests should be repeatable in any environment and there should be no excuse for why they fail.

- **Self-Validating** a test should answer with either *Passed* or *Failed*. You don't need to compare log files to answer if a test passed.

- **Timely** unit tests should be written before the production code. If you write tests after the production code, you might find writing tests too hard.

**[⬆ başa dön](#table-of-contents)**

### Single concept per test

Tests should also follow the *Single Responsibility Principle*. Make only one assert per unit test.

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

**[⬆ başa dön](#table-of-contents)**

### The name of the test should reveal its intention

When a test fail, its name is the first indication of what may have gone wrong.

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

**[⬆ başa dön](#table-of-contents)**

## Concurrency

### Prefer promises vs callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting *(the callback hell)*.  
There are utilities that transform existing functions using the callback style to a version that returns promises
(for Node.js see [`util.promisify`](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original), for general purpose see [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify))

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

Promises supports a few helper methods that help make code more concise:  

| Pattern                  | Description                                |  
| ------------------------ | -----------------------------------------  |  
| `Promise.resolve(value)` | Convert a value into a resolved promise.   |  
| `Promise.reject(error)`  | Convert an error into a rejected promise.  |  
| `Promise.all(promises)`  | Returns a new promise which is fulfilled with an array of fulfillment values for the passed promises or rejects with the reason of the first promise that rejects. |
| `Promise.race(promises)`| Returns a new promise which is fulfilled/rejected with the result/error of the first settled promise from the array of passed promises. |

`Promise.all` is especially useful when there is a need to run tasks in parallel. `Promise.race` makes it easier to implement things like timeouts for promises.

**[⬆ başa dön](#table-of-contents)**

### Async/Await are even cleaner than Promises

With `async`/`await` syntax you can write code that is far cleaner and more understandable than chained promises. Within a function prefixed with `async` keyword you have a way to tell the JavaScript runtime to pause the execution of code on the `await` keyword (when used on a promise).

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

// somewhere in an async function
try {
  const content = await downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html');
  console.log(content);
} catch (error) {
  console.error(error);
}
```

**[⬆ başa dön](#table-of-contents)**

## Error Handling

Thrown errors are a good thing! They mean the runtime has successfully identified when something in your program has gone wrong and it's letting you know by stopping function
execution on the current stack, killing the process (in Node), and notifying you in the console with a stack trace.

### Always use Error for throwing or rejecting

JavaScript as well as TypeScript allow you to `throw` any object. A Promise can also be rejected with any reason object.  
It is advisable to use the `throw` syntax with an `Error` type. This is because your error might be caught in higher level code with a `catch` syntax.
It would be very confusing to catch a string message there and would make
[debugging more painful](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).  
For the same reason you should reject promises with `Error` types.

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

The benefit of using `Error` types is that it is supported by the syntax `try/catch/finally` and implicitly all errors have the `stack` property which
is very powerful for debugging.  
There are also another alternatives, not to use the `throw` syntax and instead always return custom error objects. TypeScript makes this even easier.
Consider following example:

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

For the detailed explanation of this idea refer to the [original post](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).

**[⬆ başa dön](#table-of-contents)**

### Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix or react to said error. Logging the error to the console (`console.log`) isn't much better as often times it can get lost in a sea of things printed to the console. If you wrap any bit of code in a `try/catch` it means you think an error may occur there and therefore you should have a plan, or create a code path, for when it occurs.

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

**[⬆ başa dön](#table-of-contents)**

### Don't ignore rejected promises

For the same reason you shouldn't ignore caught errors from `try/catch`.

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

// or using the async/await syntax:

try {
  const user = await getUser();
  await sendEmail(user.email, 'Welcome!');
} catch (error) {
  logger.log(error);
}
```

**[⬆ başa dön](#table-of-contents)**

## Formatting

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is *DO NOT ARGUE* over formatting. There are tons of tools to automate this. Use one! It's a waste of time and money for engineers to argue over formatting. The general rule to follow is *keep consistent formatting rules*.  

For TypeScript there is a powerful tool called [TSLint](https://palantir.github.io/tslint/). It's a static analysis tool that can help you improve dramatically the readability and maintainability of your code. There are ready to use TSLint configurations that you can reference in your projects:

- [TSLint Config Standard](https://www.npmjs.com/package/tslint-config-standard) - standard style rules

- [TSLint Config Airbnb](https://www.npmjs.com/package/tslint-config-airbnb) - Airbnb style guide

- [TSLint Clean Code](https://www.npmjs.com/package/tslint-clean-code) - TSLint rules inspired by the [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.ca/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)

- [TSLint react](https://www.npmjs.com/package/tslint-react) - lint rules related to React & JSX

- [TSLint + Prettier](https://www.npmjs.com/package/tslint-config-prettier) - lint rules for [Prettier](https://github.com/prettier/prettier) code formatter

- [ESLint rules for TSLint](https://www.npmjs.com/package/tslint-eslint-rules) - ESLint rules for TypeScript

- [Immutable](https://www.npmjs.com/package/tslint-immutable) - rules to disable mutation in TypeScript

Refer also to this great [TypeScript StyleGuide and Coding Conventions](https://basarat.gitbook.io/typescript/styleguide) source.

### Use consistent capitalization

Capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just *be consistent*.

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

Prefer using `PascalCase` for class, interface, type and namespace names.  
Prefer using `camelCase` for variables, functions and class members.

**[⬆ başa dön](#table-of-contents)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee.
We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

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

**[⬆ başa dön](#table-of-contents)**

### Organize imports

With clean and easy to read import statements you can quickly see the dependencies of current code. Make sure you apply following good practices for `import` statements:

- Import statements should be alphabetized and grouped.
- Unused imports should be removed.
- Named imports must be alphabetized (i.e. `import {A, B, C} from 'foo';`)
- Import sources must be alphabetized within groups, i.e.: `import * as foo from 'a'; import * as bar from 'b';`
- Groups of imports are delineated by blank lines.
- Groups must respect following order:
  - Polyfills (i.e. `import 'reflect-metadata';`)
  - Node builtin modules (i.e. `import fs from 'fs';`)
  - external modules (i.e. `import { query } from 'itiriri';`)
  - internal modules (i.e `import { UserService } from 'src/services/userService';`)
  - modules from a parent directory (i.e. `import foo from '../foo'; import qux from '../../foo/qux';`)
  - modules from the same or a sibling's directory (i.e. `import bar from './bar'; import baz from './bar/baz';`)

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

**[⬆ başa dön](#table-of-contents)**

### Use typescript aliases

Create prettier imports by defining the paths and baseUrl properties in the compilerOptions section in the `tsconfig.json`

This will avoid long relative paths when doing imports.

**Yanlış:**

```ts
import { UserService } from '../../../services/UserService';
```

**Doğru:**

```ts
import { UserService } from '@services/UserService';
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@services": ["services/*"]
    }
    ...
  }
...
```

**[⬆ başa dön](#table-of-contents)**

## Comments

The use of a comments is an indication of failure to express without them. Code should be the only source of truth.
  
> Don’t comment bad code—rewrite it.  
> — *Brian W. Kernighan and P. J. Plaugher*

### Prefer self explanatory code instead of comments

Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Yanlış:**

```ts
// Check if subscription is active.
if (subscription.endDate > Date.now) {  }
```

**Doğru:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) { /* ... */ }
```

**[⬆ başa dön](#table-of-contents)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

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

**[⬆ başa dön](#table-of-contents)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use `git log` to get history!

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

**[⬆ başa dön](#table-of-contents)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the proper indentation and formatting give the visual structure to your code.  
Most IDE support code folding feature that allows you to collapse/expand blocks of code (see Visual Studio Code [folding regions](https://code.visualstudio.com/updates/v1_17#_folding-regions)).

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

**[⬆ başa dön](#table-of-contents)**

### TODO comments

When you find yourself that you need to leave notes in the code for some later improvements,
do that using `// TODO` comments. Most IDE have special support for those kind of comments so that
you can quickly go over the entire list of todos.  

Keep in mind however that a *TODO* comment is not an excuse for bad code. 

**Yanlış:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Doğru:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ başa dön](#table-of-contents)**

## Translations

This is also available in other languages:
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [vitorfreitas/clean-code-typescript](https://github.com/vitorfreitas/clean-code-typescript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese**: 
  - [beginor/clean-code-typescript](https://github.com/beginor/clean-code-typescript)
  - [pipiliang/clean-code-typescript](https://github.com/pipiliang/clean-code-typescript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [ralflorent/clean-code-typescript](https://github.com/ralflorent/clean-code-typescript)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [MSakamaki/clean-code-typescript](https://github.com/MSakamaki/clean-code-typescript)
- ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [738/clean-code-typescript](https://github.com/738/clean-code-typescript)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Real001/clean-code-typescript](https://github.com/Real001/clean-code-typescript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [JoseDeFreitas/clean-code-typescript](https://github.com/JoseDeFreitas/clean-code-typescript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [ozanhonamlioglu/clean-code-typescript](https://github.com/ozanhonamlioglu/clean-code-typescript)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hoangsetup/clean-code-typescript](https://github.com/hoangsetup/clean-code-typescript)

References will be added once translations are completed.  
Check this [discussion](https://github.com/labs42io/clean-code-typescript/issues/15) for more details and progress.
You can make an indispensable contribution to *Clean Code* community by translating this to your language.
