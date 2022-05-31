# clean-code-java-tr

## içindekiler
1. [Giriş](#giriş)
2. [Değişkenler](#değişkenler)
3. [Fonksiyonlar](#fonksiyonlar)
4. [Nesneler Ve Veri Yapıları](#nesneler-ve-veri-yapıları)
5. [Sınıflar](#sınıflar)
6. [SOLID](#solid)
7. [Test etme](#test-etme)
8. [Eşzamanlılık](#eşzamanlılık)
9. [Hata Yakalama](#hata-yakalama)
10. [Yazım Şekli](#yazım-şekli)
11. [Yorumlar](#yorumlar)
12. [Çeviri](#translation)

## Giriş
![Yazılım kalitesi tahmininin kaç höykürme sayısı olarak komik görüntüsü okurken siz de höyküreceksiniz](https://www.osnews.com/images/comics/wtfm.jpg)

Yazlım mühendisliği prensipleri, Robert C. Martin'in kitabından  [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), Java için uyarlandı. Bu belge bir kod yazma semantik rehberi değildir. Bu belge Java ile [okunabilir, yeniden kullanılabilir ve elden geçirilebilir](https://github.com/ryanmcdermott/3rs-of-software-architecture) yazılım üretebilmek için kullanılabilecek bir rehber oluşturmak için yazıldı.

Buradaki her ilkeye kesinlikle uyulması gerekmiyor ve hatta bir çoğu evrensel olarak kabul edilmeyebilir. Bunlar yönergelerdir, daha fazlası değil, ama uzun yıllar boyunca edindikleri toplu tecrübe ile _Clean Code_ kitabı yazarlarınca derlenmiş olanlardır.

Yazılım mühendisliği zanaatı 50 yaşın biraz üzerinde ve hala çok şey öğreniyoruz. Yazılım mimarisi mimarlığın kendisi kadar eskidiğinde, belki o zaman uyması gereken daha sağlam kurallar olacaktır. Şimdilik, bu kuralların sizin ve ekibinizin ürettiği Java kodunun kalitesini değerlendirmek için bir mihenk taşı olarak hizmet etmesine izin verin.

Bir şey daha var: bunları bilmek sizi hemen daha iyi bir yazılım geliştiricisi yapmaz ve bunlarla yıllarca çalışmış olmak hiç hata yapmayacağınız anlamına da gelmez. Her kod parçası, şekillendirilip son haline çevirilen ıslak kilin gibi ilk taslak olarak başlar. Son olarak, akranlarımızla birlikte gözden geçirdiğimiz zaman kusurları gideririz. İyileştirilmesi gereken ilk taslaklar için kendinize eziyet etmeyin. Bunun yerine kodu yorun!

## **Değişkenler**

### Anlamlı ve telafuz edilebilir değişken isimleri kullanın

**Yanlış:**
```java
String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```

**Doğru:**
```java
String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```
**[⬆ başa dön](#içindekiler)**

### Aynı tür değişken için aynı kelimeleri kullanın

**Yanlış:**
```java
getUserInfo();
getClientData();
getCustomerRecord();
```

**Doğru:**
```java
getUser();
```


**[⬆ başa dön](#içindekiler)**

### Aranabilecek isimler kullanın
Yazdığımızdan daha çok kod satırı okuruz. Dolayısıyla yazdığımız kodun okunabilir ve aranabilir olması önemlidir. Değişkenlerimize programımızın ne yapmaya çalıştığını anlatacak anlamlı isimler vermezsek kodumuzu okumaya çalışanlar çok üzülecektir. Verdiğiniz isimlerin kolayca aranabilir olmasını sağlayın.

**Yanlış:**
```java
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);

```

**Doğru:**
```java
// Declare them as capitalized `const` globals.
public static final int MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ başa dön](#içindekiler)**

### Açıklayıcı değişkenler kullanın
**Yanlış:**
```java
String address = "One Infinite Loop, Cupertino 95014";
String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

saveCityZipCode(address.split(cityZipCodeRegex)[0],
                address.split(cityZipCodeRegex)[1]);
```

**Doğru:**
```java
  String address = "One Infinite Loop, Cupertino 95014";
  String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

  String city = address.split(cityZipCodeRegex)[0];
  String zipCode = address.split(cityZipCodeRegex)[1];

  saveCityZipCode(city, zipCode);

```
**[⬆ başa dön](#içindekiler)**

### Zihinsel Haritalamadan Kaçının
Değişken isimlerini anlaması için kodunuzu okuyan kişiyi zorlamayın. Açık olmak kapalı olmaktan daha iyidir.
**Yanlış:**
```java
String [] l = {"Austin", "New York", "San Francisco"};

for (int i = 0; i < l.length; i++) {
    String li = l[i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Wait, what is `$li` for again?
    dispatch(li);
 }
```

**Doğru:**

```java
String[] locations = {"Austin", "New York", "San Francisco"};

for (String location : locations) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch(location);
 }
```
**[⬆ başa dön](#içindekiler)**

### Gereksiz bağlam ekleme

Eğer sınıf ya da nesne ne yaptığını söylüyprsa, değişken isminde tekrar belirtmenize gerek yok.
variable name.

**Yanlış:**
```java
class Car {
  public String carMake = "Honda";
  public String carModel = "Accord";
  public String carColor = "Blue";
}

void paintCar(Car car) {
  car.carColor = "Red";
}
```

**Doğru:**
```java
class Car {
  public String make = "Honda";
  public String model = "Accord";
  public String color = "Blue";
}

void paintCar(Car car) {
  car.color = "Red";
}
```
**[⬆ başa dön](#içindekiler)**

## **Fonksiyonlar**

### Fonksiyon parametreleri (ideal olan 2 veya daha az olması)

Fonksiyon parametrelerinin sayısının sınırlandırılması, fonksiyonunuzun test edilmesini kolaylaştırdığı için inanılmaz derecede önemlidir. Üçten fazlaya sahip olmak, her bir ayrı argümanla tonlarca farklı durumu test etmeniz gereken bir kombinasyon patlamasına yol açar.

Bir veya iki argüman ideal durumdur ve mümkünse üçten kaçınılmalıdır. Bundan fazlası tekrar düşünülmelidir. Çoğunlukla, 2 den fazla parametreye
sahip bir fonksiyonunuz varsa, yapması gerektiğinden fazla iş yapıyordur. Gerçekten gerekli olduğu durumda, çoğu zaman bir üst seviye nesne argüman olarak yeterli olacaktır.


**[⬆ başa dön](#içindekiler)**


### Fonksiyonlar sadece bir iş yapmalı

Bu, yazılım mühendisliğinde belki de en önemli kuraldır. Eğer bir fonksiyon birden fazla iş yapıyorsa, bu fonksiyonu oluşturmaki test etmek ve anlamlandırmak zordur. Eğer bir fonksiyonu sadece bir işe yapacak şekilde sınırlandırırsanız, kolayca elden geçirilebilir ve kodunuz daha okunaklı olur. Bu rehberden birtek bunu alsanız bile, birçok geliştiriciden önde olacaksınız.

**Yanlış:**
```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        Client clientRecord = repository.findOne(client.getId());
        if (clientRecord.isActive()){
            email(client);
        }
    }
}
```

**Doğru:**
```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        if (isActiveClient(client)) {
            email(client);
        }
    }
}

private boolean isActiveClient(Client client) {
    Client clientRecord = repository.findOne(client.getId());
    return clientRecord.isActive();
}
```

**[⬆ başa dön](#içindekiler)**

### Fonksiyon isimleri fonksiyonun yaptığı işi anlatmalı

**Yanlış:**
```java
private void addToDate(Date date, int month){
    //..
}

Date date = new Date();

// Fonksiyon isminden neyin eklendiği anlaşılmıyor
addToDate(date, 1);
```
**Doğru:**
```java
private void addMonthToDate(Date date, int month){
    //..
}

Date date = new Date();
addMonthToDate(1, date);
```

**[⬆ başa dön](#içindekiler)**

### Fonksiyonlarda yalnızca bir seviye soyutlama olmalıdır

Birden fazla soyutlama seviyesine sahipseniz, fonksiyon genellikle çok fazla şey yapar. Fonksiyonları bölmek yeniden kullanılabilirliğe ve daha 
kolay testlere neden olur.


**[⬆ başa dön](#içindekiler)**

### Yinelenen kodları kaldırın
Kod yinelenmesini engellemek için elinizden geleni yapın. Yinelenen kod kötüdür çünkü bir sorun olduğunda ya da değiştirilmesi gerektiğinde ilgilenilmesi gereken birden fazla yer var demektir.

Bir restoran işlettiğinizi ve envanterinizi takip ettiğinizi düşünün: tüm domatesleriniz, soğanlarınız, sarımsaklarınız, baharatlarınız vb. Eğer birden fazla listeniz varsa, bir yemek yaptığınızda hepsini güncellemeniz gerekecektir. Yalnızca bir listeniz varsa, güncellenecek tek bir yer vardır!

Çoğunlukla, yinelenen kod yapılarınız vardır. Çünkü bir ortak işlemi paylaşan iki veya daha fazla farklı fonksiyonunuz olabilir. Ancak bazı ufak farklılıklar sizi aynı şeyleri yapan iki veya daha fazla ayrı fonksiyona sahip olmaya zorlar. Çift kodun kaldırılması, bu farklı şeyleri tek bir işlev/modül/sınıfla işleyebilecek bir soyutlama oluşturmak anlamına gelir.

Soyutlamayı doğru yapmak çok önemlidir, bu yüzden _Sınıflar_ bölümünde belirtilen SOLID ilkelerine uymalısınız. Kötü soyutlamalar yinelenen 
kodlardan daha da kötü olabilir, bu yüzden dikkatli olun! Bunu söyledikten sonra, iyi bir soyutlama yapabilirseniz yapın! Kendinizi tekrar etmeyin, aksi halde, bir şeyi değiştirmek istediğinizde kendinizi birden çok yeri güncellerken bulacaksınız.


**[⬆ başa dön](#içindekiler)**


### Karar verici fonksiyon parametreleri kullanmayın

Karar verici parametreler bir fonksiyonun birden fazla iş yaptığını gösterir. Fonksiyonlar tek iş yapmalılar. Boolean bir değer üzerinden yapacağı işe karar veren fonksiyonları birden fazla fonksiyona bölün.


**[⬆ başa dön](#içindekiler)**

### Yan etkilerden kaçınma

Bir fonksiyon, bir değeri almak ve başka bir değer veya değerler döndürmekten başka bir şey yaparsa, bir yan etki oluşturur. Bu yan etki bir dosyaya yazmak, bazı global değişkenleri değiştirmek veya yanlışlıkla tüm paranızı bir yabancıya bağlamak olabilir.

Zaman zaman bir programda yan etkilere ihtiyacınız olur. Önceki örnekte olduğu gibi, bir dosyaya yazmanız gerekebilir. Yapmak istediğiniz şey, bunu yaptığınız yeri merkezileştirmektir. Belirli bir dosyaya yazan çok sayıda fonksiyon ve sınıfa sahip olmayın. Bunu yapan bir servis oluşturun. Sadece bir tane.

Ana nokta, herhangi bir yapıya sahip olmadan nesneler arasında durum paylaşımı, herhangi bir şey tarafından yazılabilen değişken yanları kullanmak ve yan etkilerin ortaya çıktığı yerleri merkezileştirmemek gibi çok yapılan hatalarda kaçınmaktır. Bunu yapabilirseniz, diğer 
programcıların büyük çoğunluğundan daha mutlu olursunuz.


**[⬆ başa dön](#içindekiler)**



### Encapsulate conditionals

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Avoid negative conditionals

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Avoid conditionals
This seems like an impossible task. Upon first hearing this, most people say,
"how am I supposed to do anything without an `if` statement?" The answer is that
you can use polymorphism to achieve the same task in many cases. The second
question is usually, "well that's great but why would I want to do that?" The
answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you
are telling your user that your function does more than one thing. Remember,
just do one thing.

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Don't over-optimize
Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

**Yanlış:**

**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Remove dead code
Dead code is just as bad as duplicate code. There's no reason to keep it in
your codebase. If it's not being called, get rid of it! It will still be safe
in your version history if you still need it.

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

## **Objects and Data Structures**
### Use getters and setters
Using getters and setters to access data on objects could be better than simply
looking for a property on an object. "Why?" you might ask. Well, here's an
unorganized list of reasons why:

* When you want to do more beyond getting an object property, you don't have
to look up and change every accessor in your codebase.
* Makes adding validation simple when doing a `set`.
* Encapsulates the internal representation.
* Easy to add logging and error handling when getting and setting.
* You can lazy load your object's properties, let's say getting it from a
server.


**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**


### Make objects have private members
This can be accomplished through closures (for ES5 and below).

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Prefer composition over inheritance
As stated famously in [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of
good reasons to use inheritance and lots of good reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for
inheritance, try to think if composition could model your problem better. In some
cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
(Change the caloric expenditure of all animals when they move).

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

## **SOLID**
### Single Responsibility Principle (SRP)
As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify
a piece of it, it can be difficult to understand how that will affect other
dependent modules in your codebase.

**Yanlış:**

**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Open/Closed Principle (OCP)
As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Liskov Substitution Principle (LSP)
This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

**Yanlış:**

**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Interface Segregation Principle (ISP)
JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

### Dependency Inversion Principle (DIP)
This principle states two essential things:
1. High-level modules should not depend on low-level modules. Both should
depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
abstractions.

This can be hard to understand at first, but if you've worked with AngularJS,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very bad development pattern because
it makes your code hard to refactor.

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

**Yanlış:**


**Doğru:**

**[⬆ başa dön](#içindekiler)**

## **Testing**
Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](http://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There's plenty of good test frameworks, so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**Yanlış:**

**Doğru:**

**[⬆ başa dön](#içindekiler)**

## **Error Handling**
Thrown errors are a good thing! They mean the runtime has successfully
identified when something in your program has gone wrong and it's letting
you know by stopping function execution on the current stack, killing the
process (in Node), and notifying you in the console with a stack trace.

### Don't ignore caught errors
Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

**Yanlış:**

**Doğru:**

**[⬆ başa dön](#içindekiler)**

## **Formatting**
Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are tons of tools to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization
JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Yanlış:**

**Doğru:**

**[⬆ başa dön](#içindekiler)**


### Function callers and callees should be close
If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Yanlış:**

**Doğru:**

**[⬆ başa dön](#içindekiler)**

## **Comments**
### Only comment things that have business logic complexity.
Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Yanlış:**
```java
// Creating a List of customer names 
List<String> customerNames = Arrays.asList('Bob', 'Linda', 'Steve', 'Mary'); 

// Using Stream findFirst() 
Optional<String> firstCustomer = customerNames.stream().findFirst(); 

// if the stream is empty, an empty 
// Optional is returned. 
if (firstCustomer.isPresent()) { 
    System.out.println(firstCustomer.get()); 
} 
else { 
    System.out.println("no value"); 
} 
```


**Doğru:**
```java
List<String> customerNames = Arrays.asList('Bob', 'Linda', 'Steve', 'Mary'); 

Optional<String> firstCustomer = customerNames.stream().findFirst(); 

if (firstCustomer.isPresent()) { 
    System.out.println(firstCustomer.get()); 
} 
else { 
    System.out.println("no value"); 
} 
```

**[⬆ başa dön](#içindekiler)**

### Don't Use a Comment When You Can Use a Function or a Variable
The best comment is no comment

**Yanlış:**
```java
//Check to see if order is eligible to ship
if((order.isPaid & order.isLabeled) && CUSTOMER_FLAG) {
  // ...
}
```


**Doğru:**
```java
if(order.isEligibleToShip()) {
  // ...
}
```

**[⬆ başa dön](#içindekiler)**

### Don't leave commented out code in your codebase
Version control exists for a reason. Leave old code in your history.

**Yanlış:**
```java
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```


**Doğru:**
```java
doStuff();
```

**[⬆ başa dön](#içindekiler)**

### Don't have journal comments
Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**Yanlış:**
```java
/**
 * 2021-03-06: Renamed clean to cleanCode (DL)
 * 2020-01-03: Changed return value (LB)
 * 2019-05-12: Added clean method (DL)
 */
 cleanCode(String code) {
   return null;
 }
```

**Doğru:**
```java
 cleanCode(String code) {
   return null;
 }
```

**[⬆ başa dön](#içindekiler)**

### Avoid positional markers
They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**Yanlış:**
```java
////////////////////////////////////////////////////////////////////////////////
// Instantiate Order List
////////////////////////////////////////////////////////////////////////////////
List<Order> orders = new ArrayList();

////////////////////////////////////////////////////////////////////////////////
// Ship Orders that are eligible
////////////////////////////////////////////////////////////////////////////////
 
orders.filter(Order::isEligibleToShip).forEach(x -> ship(x));
```

**Doğru:**
```java
List<Order> orders = new ArrayList();

orders.filter(Order::isEligibleToShip).forEach(x -> ship(x));
```

**[⬆ başa dön](#içindekiler)**

## Translation

Open for translations.

**[⬆ başa dön](#içindekiler)**
