# Clean Code alapelvek Java példákkal

## Tartalomjegyzék

0. [Bevezetés](#bevezetés)
1. [Általános szabályok](#általános-szabályok)
2. [Változók](#változók)
    - Használj önmagukban is jelentéssel bíró, kimondható változóneveket
    - Használd ugyanazt az elnevezést az összetartozó változók esetében
    - Használj kereshető elnevezéseket
    - Használj magyarázó változókat
    - "Úgyis emlékezni fogok rá!" Nem fogsz.
    - Kerüld a felesleges duplikációkat
3. [Függvények](#függvények)
    - Függvények paraméterezése (ideális esetben 2 vagy kevesebb)
    - Egy függvény egy dolgot csináljon
    - Adj beszélő nevet a függvényeknek
    - Egy függvényen belül csak egy absztrakciós szintű kód szerepeljen
    - Kerüld a kódduplikációt
    - Ne használj flag-eket metódus paraméterként
    - Kerüld a mellékhatásokat
    - Preferáld a funkcionális programozást az imperatív programozással szemben
    - Zárd egységbe a feltételeket
    - Kerüld a tagadó feltételeket
    - Kerüld a feltételeket
    - Csak akkor használj type-checkinget, ha mindenképp szükséges
    - Töröld a már nem használt kódot
4. [Objektumok és adatstruktúrák](#objektumok-és-adatstruktúrák)
    - Használj gettereket és settereket
5. [Osztályok](#osztályok)
    - Fontold meg, hogy új osztályt hozol létre új metódus helyett
    - Használj metódus láncolást
    - Használj objektum-összetételt öröklés helyett, ahol csak lehet
6. [SOLID](#solid)
    - Single Responsibility Principle (SRP)
    - Open/Closed Principle (OCP)
    - Liskov Substitution Principle (LSP)
    - Interface Segregation Principle (ISP)
    - Dependency Inversion Principle (DIP)
7. [Tesztelés](#tesztelés)
    - Egy teszt mindig egyetlen dologra fókuszáljon
8. [Hibakezelés](#hibakezelés)
    - Kezdj valamit az elkapott hibákkal
    - Aszinkron műveletek esetén is kezeld a hibákat
9. [Formázás](#formázás)
    - Alkalmazd következetesen a kis- és nagybetűket
    - A hívó és meghívott függvényeket helyezd közel egymáshoz
10. [Kommentek](#kommentek)
    - Kommentben csak üzleti logikát magyarázz
    - Ne hagyj kikommentezett kódot a kódbázisban
    - Ne naplózz kommentben
    - Kerüld a "formázó" kommentek használatát

## Bevezetés

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

Szoftverfejlesztési alapelvek ismertetése Robert C. Martin könyve, a [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) alapján Java programnyelven írt gyakorlati példákkal. A leírás bemutatja, hogyan érdemes olvasható, egyszerűen karbantartható és könnyen refaktorálható kódot írni.

Az itt leírtak pusztán irányelvek, azonban a Clean Code népszerűsége nyomán széles körben ismert és elismert sztenderddé váltak.

A szoftverfejlesztő szakma alig több mint 50 éves, és kollektíve még mindig sok tanulnivalónk van. Ha a szoftverarchitektúra olyan idős lesz, mint mondjuk az építészet tudománya, talán már keményebb szabályokat kell követnünk. Ennek megfelelően nem kell minden itt szereplő elvet szigorúan követned, de érdemes tartanod magad a könyvben megfogalmazott szellemiséghez.

Még egy megjegyzés: a Clean Code irányelvek ismerete nem fog azonnal jobb szoftverfejlesztővé tenni, és ha évekig ezekkel dolgozol, az sem jelenti azt, hogy nem fogsz hibázni. Minden kód piszkozatként indul, aminek tökéletlenségeit akkor javíthatod ki, amikor a társaiddal közösen átnézitek azt. Ne ostorozd magad a javításra szoruló első vázlatok miatt.

Vágjunk bele!


## **Általános szabályok**

- Kövesd az iparági sztenderdeket és konvenciókat.
- Keep it simple, stupid (KISS), avagy az egyszerűbb mindig jobb. Csökkentsd a komplexitást, ahol csak tudod.
- Tartsd a cserkésztörvényt (Boy scout rule), avagy hagyd tisztábban a táborhelyet, mint ahogyan találtad.
- Mindig keresd a probléma kiváltó okát.


## **Változók**

### Használj önmagukban is jelentéssel bíró, kimondható változóneveket

**Rossz:**

```java
String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```

**Jó:**

```java
String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Használd ugyanazt az elnevezést az összetartozó változók esetében

**Rossz:**

```java
String userName;
Map clientDocuments;
int customerRecordNr;
```

**Jó:**

```java
String userName;
Map userDocuments;
int userRecordNr;
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Használj kereshető elnevezéseket

A karriered során sokkal több kódot fogsz olvasni, mint amennyit valaha is megírhatnál. Éppen ezért fontos, hogy a kód, amit megírsz, könnyen olvasható és kereshető legyen. Ha nem érthető, magyarázó erővel bíró neveket adsz a változóidnak, annak te és a csapattársaid is megisszátok a levét. Az olyan kódminőség-ellenőrző eszközök, mint pl. a [SonarQube](https://rules.sonarsource.com/java/RSPEC-109/) segítenek észrevenni az ilyen hibákat.

**Rossz:**

```java
// Mi a fene az, hogy 86400000?
setTimeout(blastOff, 86400000);
```

**Jó:**

```java
// Deklaráld egy nagybetűkből álló, alsóvonásokkal tagolt elnevezésű változóban az értéket
public static final int MILLISECONDS_IN_A_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Használj magyarázó változókat

**Rossz:**

```java
String address = "One Infinite Loop, Cupertino 95014";
String cityZipCodeRegex = "([^,]+),\\s*(\\d{5})";

Pattern pattern = Pattern.compile(cityZipCodeRegex);
Matcher matcher = pattern.matcher(address);

if (matcher.find()) {
    saveCityZipCode(matcher.group(1), matcher.group(2));
}
```

**Jó:**

```java
String address = "One Infinite Loop, Cupertino 95014";
String cityZipCodeRegex = "([^,]+),\\s*(\\d{5})";

Pattern pattern = Pattern.compile(cityZipCodeRegex);
Matcher matcher = pattern.matcher(address);

String city;
String zipCode;

if (matcher.find()) {
    city = matcher.group(1);
    zipCode = matcher.group(2);
}

saveCityZipCode(city, zipCode);
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### "Úgyis emlékezni fogok rá!" Nem fogsz.

**Rossz:**

```java
String[] locations = {"Austin", "New York", "San Francisco"};

for (String l : locations) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Várjunk, mi is az az l?
    dispatch(l);
}
```

**Jó:**

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

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Kerüld a felesleges duplikációkat

Ha az osztályod vagy objektumod neve elmond magáról valamit (ahogy az ajánlott is), ne ismételd meg ugyanazt a változónévben.

**Rossz:**

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

**Jó:**

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

**[⬆ ugrás a tetejére](#tartalomjegyzék)**


## **Függvények**

### Függvények paraméterezése (ideális esetben 2 vagy kevesebb)

A függvénydeklarációban nem kell megadnunk a függvény törzsét, csak a hívásához szükséges információkat: a nevet, a paramétereket és a visszatérési érték típusát (ha van). A függvények deklarációjában használt paraméter neveket szokás formális paraméternek (parameter) nevezni, míg a függvény meghívásakor ténylegesen átadott értékek az aktuális paraméter (argument). A függvény nevét a teljes paraméterlistával a függvény szignatúrájának (signature) nevezzük, a visszatérő érték típusát és a szignatúrát együtt pedig a függvény prototípusának (function prototype) nevezzük.

A függvényparaméterek mennyiségének korlátozása hihetetlenül fontos, mert ez megkönnyíti a függvény tesztelését. Ha háromnál több van, az egy kombinatorikai robbanáshoz vezet, ahol rengeteg különböző esetet kell tesztelned minden egyes paraméterrel.

Egy vagy két paraméter használata az ideális eset, a három pedig már lehetőleg kerülendő. Általában ha kettőnél több paramétered van, akkor a függvényed túl sok dolgot próbál csinálni. Ha négy vagy több paramétered van, jó eséllyel érdemes szétbontanod a függvényed. Azokban az esetekben, ahol ez nem így van, a legtöbbször egy magasabb szintű objektum paraméterként való átadása is elégséges lehet.


**Rossz:**

```java
public void processOrder(String customerName, String productName, int quantity, double price) {
    // ... implementáció ...
}
```

**Jó:**

```java
public void createOrder(String customerName, String productName, int quantity) {
    // ... rendelés létrehozása ...
}

public void calculateTotal(String productName, int quantity, double price) {
    // ... rendelés értékének számítása ...
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Egy függvény egy dolgot csináljon

Ez messze a legfontosabb szabály a szoftverfejlesztésben. Amikor a függvények egynél több dolgot csinálnak, nehezebb őket megírni, tesztelni és általában beszélni róluk. Ha egy függvényt kifejezetten csak egy művelethez tudsz társítani, akkor az nagyban segíti az olvashatóságot és a refaktorálhatóságot. Ha semmi mást, csak ezt viszed magaddal ebből az útmutatóból, már jobb vagy, mint sok más fejlesztő.

**Rossz:**

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

**Jó:**

```java
public void emailActiveClients(List<Client> clients) {
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

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Adj beszélő nevet a függvényeknek

**Rossz:**

```java
private void addToDate(Date date, int month){
    //..
}

Date date = new Date();

// A függvény nevéből nehéz megmondani, mit adunk hozzá a dátumhoz
addToDate(date, 1);
```

**Jó:**

```java
private void addMonthToDate(Date date, int month){
    //..
}

Date date = new Date();
addMonthToDate(1, date);
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Egy függvényen belül csak egy absztrakciós szintű kód szerepeljen

Ha egy függvényen belül több absztrakció is fellelhető, a függvény valószínűleg a kelleténél több dolgot csinál. Bontsd alkotólemeikre a könnyebb használhatóság és tesztelhetőség érdekében.

**Rossz:**

```java
public class ShoppingCart {
    public static void calculateAndPrintReceipt(List<ShoppingCartItem> cartItems) {
        double total = 0;
        double discount = 0;
        double taxes = 0;
        double finalPrice = 0;

        // 1: részösszeg számítása
        for (ShoppingCartItem item : cartItems) {
            total += item.getPrice() * item.getQuantity();
        }

        // 2: kedvezmény számítása
        if (total > 100) {
            discount = total * 0.1;
        }

        // 3: adó számítása
        taxes = total * 0.08;

        // 4: végösszeg számítása kedvezménnyel és adóval
        finalPrice = total - discount + taxes;

        // 5: bizonylat nyomtatása
        System.out.println("Receipt:");
        System.out.println("Total Price: $" + String.format("%.2f", total));
        System.out.println("Discount: $" + String.format("%.2f", discount));
        System.out.println("Taxes: $" + String.format("%.2f", taxes));
        System.out.println("Final Price: $" + String.format("%.2f", finalPrice));
    }

    public static void main(String[] args) {
        List<ShoppingCartItem> cartItems = new ArrayList<>();
        cartItems.add(new ShoppingCartItem("Item 1", 20.0, 2));
        cartItems.add(new ShoppingCartItem("Item 2", 30.0, 1));
        cartItems.add(new ShoppingCartItem("Item 3", 10.0, 3));

        calculateAndPrintReceipt(cartItems);
    }
}
```

**Jó:**

```java
class ShoppingCartItem {
    String name;
    double price;
    int quantity;

    public ShoppingCartItem(String name, double price, int quantity) {
        this.name = name;
        this.price = price;
        this.quantity = quantity;
    }

    public double getTotalPrice() {
        return price * quantity;
    }
}

public class ShoppingCart {
    public static double calculateTotalPrice(List<ShoppingCartItem> items) {
        double total = 0;
        for (ShoppingCartItem item : items) {
            total += item.getTotalPrice();
        }
        return total;
    }

    public static double applyDiscount(double total) {
        double discount = 0;
        if (total > 100) {
            discount = total * 0.1;
        }
        return discount;
    }

    public static double calculateTaxes(double total) {
        double taxRate = 0.08;
        return total * taxRate;
    }

    public static double calculateFinalPrice(double total, double discount, double taxes) {
        return total - discount + taxes;
    }

    public static void printReceipt(double total, double discount, double taxes, double finalPrice) {
        System.out.println("Receipt:");
        System.out.println("Total Price: $" + String.format("%.2f", total));
        System.out.println("Discount: $" + String.format("%.2f", discount));
        System.out.println("Taxes: $" + String.format("%.2f", taxes));
        System.out.println("Final Price: $" + String.format("%.2f", finalPrice));
    }

    public static void main(String[] args) {
        List<ShoppingCartItem> cartItems = new ArrayList<>();
        cartItems.add(new ShoppingCartItem("Item 1", 20.0, 2));
        cartItems.add(new ShoppingCartItem("Item 2", 30.0, 1));
        cartItems.add(new ShoppingCartItem("Item 3", 10.0, 3));

        double total = calculateTotalPrice(cartItems);
        double discount = applyDiscount(total);
        double taxes = calculateTaxes(total);
        double finalPrice = calculateFinalPrice(total, discount, taxes);

        printReceipt(total, discount, taxes, finalPrice);
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Kerüld a kódduplikációt

Tegyél meg minden tőled telhetőt, hogy elkerüld a kódduplikációt. A duplikált kód azért rossz, mert több helyen kell módosítanod rajta, ha változtatni kell a kód alaplogikáján.

A kódduplikáció gyakori oka, hogy van két vagy több egy kicsit eltérő dolog, amiben sok a közös, de a különbségeik miatt kénytelen vagy két vagy több különálló függvényt használni, amik maguk is nagyrészt ugyanazt a dolgot csinálják. A duplikált kód eltávolítása azt jelenti, hogy létrehozunk egy olyan absztrakciót, amely képes kezelni ezeket a különböző dolgokat egyetlen függvény/modul/osztály segítségével.

A megfelelő absztrakció használata létfontosságú, ezért érdemes használni a _SOLID_ alapelveket. A rossz absztrakció jobban hátráltat, mint a duplikált kód, úgyhogy légy óvatos. Mindenesetre a kockázat megéri, ha egy-egy változtatás esetén nem akarod mindig több helyen átírni a kódod.

**Rossz:**

```java
class Developer{
    // ...
}

class Manager{
    // ...
}

public class EmployeeListRenderer {
    public void showDeveloperList(List<Developer> developers) {
        for (Developer developer : developers) {
            int expectedSalary = developer.calculateExpectedSalary();
            int experience = developer.getExperience();
            String githubLink = developer.getGithubLink();
            render(expectedSalary, experience, githubLink);
        }
    }

    public void showManagerList(List<Manager> managers) {
        for (Manager manager : managers) {
            int expectedSalary = manager.calculateExpectedSalary();
            int experience = manager.getExperience();
            String[] portfolio = manager.getMBAProjects();
            render(expectedSalary, experience, portfolio);
        }
    }

    private void render(int expectedSalary, int experience, String githubLink) {
        // Fejlesztők adatai
        System.out.println("Expected Salary: $" + expectedSalary);
        System.out.println("Experience: " + experience + " years");
        System.out.println("GitHub Link: " + githubLink);
        // ...
    }

    private void render(int expectedSalary, int experience, String[] portfolio) {
        // Menedzserek adatai
        System.out.println("Expected Salary: $" + expectedSalary);
        System.out.println("Experience: " + experience + " years");
        System.out.println("MBA Projects: " + String.join(", ", portfolio));
        // ...
    }
}
```

**Jó:**

```java
class Employee {
    // ...
}

class Developer extends Employee {
    // ...
}

class Manager extends Employee {
    // ...
}

public class EmployeeListRenderer {
    public void showEmployeeList(List<Employee> employees) {
        for (Employee employee : employees) {
            int expectedSalary = employee.calculateExpectedSalary();
            int experience = employee.getExperience();

            EmployeeData data = new EmployeeData(expectedSalary, experience);

            if (employee instanceof Manager) {
                Manager manager = (Manager) employee;
                data.setPortfolio(manager.getMBAProjects());
            } else if (employee instanceof Developer) {
                Developer developer = (Developer) employee;
                data.setGithubLink(developer.getGithubLink());
            }

            render(data);
        }
    }

    private void render(EmployeeData data) {
        // Alkalmazotti adatok
        System.out.println("Expected Salary: $" + data.getExpectedSalary());
        System.out.println("Experience: " + data.getExperience() + " years");

        if (data.getPortfolio() != null) {
            System.out.println("Portfolio: " + String.join(", ", data.getPortfolio()));
        }

        if (data.getGithubLink() != null) {
            System.out.println("GitHub Link: " + data.getGithubLink());
        }

        // ...
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Ne használj flag-eket metódus paraméterként

A flagek használata biztostja, hogy egy függvény több dolgot is tudjon csinálni. Ezzel szemben ideális esetben egy függvény egyszerre egy dolgot csinál. Szedd szét a boolean flag-gel paraméterezett függvényeid több egyfunkciós függvényre.

**Rossz:**

```java
public void processUserData(User user, boolean isAdmin) {
    if (isAdmin) {
        // Admin-specifikus lépések
    } else {
        // Általános user-specifikus lépések
    }
}
```

**Jó:**

```java
public void processUser(User user) {
    // Általános user-specifikus lépések
}

public void processAdminUser(User user) {
    // Admin-specifikus lépések
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Kerüld a mellékhatásokat

Egy függvény akkor produkál mellékhatást, ha bármi mást tesz, mint egy értéket vesz át és egy másik értéket vagy értékeket ad vissza. Mellékhatás lehet például egy fájlba való írás, egy globális változó módosítása, vagy véletlenül az összes pénzünk átutalása egy idegennek.

A programokban időnként szükség van mellékhatásokra. Ahogy az előző példában is, szükség lehet arra, hogy egy fájlba írjunk. Akkor jársz el helyesen, ha központosítod, hol kerül sor erre a műveletre. Ne legyen több függvény és osztály amelyek egy adott fájlba írnak. Legyen egy szolgáltatás, ami ezt elvégzi. Egy és csakis egy.

A lényeg az, hogy elkerüljük az olyan gyakori buktatókat, mint a strukturálatlan objektumok közötti állapotmegosztás, változtatható adattípusok használata, amelyekbe bármi beleírhat, és a mellékhatások központosításának mellőzése. Ha ezt meg tudod tenni, akkor boldogabb leszel, mint a programozók nagy többsége.

**Rossz:**

```java
public class SideEffectsExample {
    // Globális változó (static mező). amit felhasznál egy metódus
    // Ha lenne még egy metódusunk, ami felhasználja a name változót, ami a splitIntoFirstAndLastName() outputjaként tömbbé változott, az hibát eredményezhet
    private static String name = "Ryan McDermott";

    public static void splitIntoFirstAndLastName() {
        name = name.replaceAll("\\s+", " ").trim();
        String[] nameParts = name.split(" ");

        if (nameParts.length >= 2) {
            name = "[" + nameParts[0] + ", " + nameParts[1] + "]";
        } else {
            // ...
        }
    }

    public static void main(String[] args) {
        splitIntoFirstAndLastName();
        System.out.println(name); // Output: ['Ryan', 'McDermott']
    }
}
```

**Jó:**

```java
public class Person {
    private String firstName;
    private String lastName;

    public Person(String fullName) {
        String[] parts = fullName.split(" ");
        if (parts.length >= 2) {
            this.firstName = parts[0];
            this.lastName = parts[1];
        } else {
            // ...
        }
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public static void main(String[] args) {
        Person person = new Person("Ryan McDermott");
        System.out.println("First Name: " + person.getFirstName());
        System.out.println("Last Name: " + person.getLastName());
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Preferáld a funkcionális programozást az imperatív programozással szemben

A hagyományos objektumorientált programozásban (OOP) a legtöbb fejlesztő hozzászokott az imperatív/eljárási stílusban történő programozáshoz. Ahhoz, hogy tiszta funkcionális stílusban válthassanak a fejlesztésre, át kell térniük a gondolkodásukban és a fejlesztéshez való hozzáállásukban.

A problémák megoldásához az OOP-fejlesztők osztályhierarchiákat terveznek, a megfelelő beágyazásra összpontosítanak, és osztályszerződésekben gondolkodnak. Az objektumtípusok viselkedése és állapota alapvető fontosságú, és nyelvi funkciók, például osztályok, interfészek, öröklés és polimorfizmus állnak rendelkezésre ezen problémák megoldásához.

Ezzel szemben a funkcionális programozás a számítási problémákat az adatgyűjtések tiszta funkcionális átalakításainak kiértékelése során közelíti meg. A funkcionális programozás elkerüli az állapotokat és a változtatható adatokat, és inkább a függvények alkalmazását hangsúlyozza.

**Rossz:**

```java
import java.util.List;
import java.util.ArrayList;

public class ProgrammerOutputExample {
    public static void main(String[] args) {
        List<Programmer> programmerOutput = new ArrayList<>();
        programmerOutput.add(new Programmer("Uncle Bobby", 500));
        programmerOutput.add(new Programmer("Suzie Q", 1500));
        programmerOutput.add(new Programmer("Jimmy Gosling", 150));
        programmerOutput.add(new Programmer("Gracie Hopper", 1000));

        int totalOutput = 0;

        for (int i = 0; i < programmerOutput.size(); i++) {
            totalOutput += programmerOutput.get(i).getLinesOfCode();
        }

        System.out.println("Total Lines of Code: " + totalOutput);
    }
}

class Programmer {
    private String name;
    private int linesOfCode;

    public Programmer(String name, int linesOfCode) {
        this.name = name;
        this.linesOfCode = linesOfCode;
    }

    public int getLinesOfCode() {
        return linesOfCode;
    }
}
```

**Jó:**

```java
import java.util.Arrays;
import java.util.List;

public class ProgrammerOutputExample {
    public static void main(String[] args) {
        List<Programmer> programmerOutput = Arrays.asList(
            new Programmer("Uncle Bobby", 500),
            new Programmer("Suzie Q", 1500),
            new Programmer("Jimmy Gosling", 150),
            new Programmer("Gracie Hopper", 1000)
        );

        int totalOutput = programmerOutput.stream()
            .mapToInt(Programmer::getLinesOfCode)
            .sum();

        System.out.println("Total Lines of Code: " + totalOutput);
    }
}

class Programmer {
    private String name;
    private int linesOfCode;

    public Programmer(String name, int linesOfCode) {
        this.name = name;
        this.linesOfCode = linesOfCode;
    }

    public int getLinesOfCode() {
        return linesOfCode;
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Zárd egységbe a feltételeket

**Rossz:**

```java
if ("fetching".equals(fsm.getState()) && isEmpty(listNode)) {
    // ...
}
```

**Jó:**

```java
if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
    // ...
}

private boolean shouldShowSpinner(FSM fsm, ListNode listNode) {
    return "fetching".equals(fsm.getState()) && isEmpty(listNode);
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Kerüld a tagadó feltételeket

**Rossz:**

```java
public boolean isDOMNodeNotPresent(Node node) {
    // Ellenőrzi, hogy a node NINCS jelen
}

if (!isDOMNodeNotPresent(node)) {
    // Dupla tagadás - a blokk akkor fut le, ha a node jelen van
}

```

**Jó:**

```java
public boolean isDOMNodePresent(Node node) {
    // Ellenőrzi, hogy a node jelen van-e
}

if (isDOMNodePresent(node)) {
    // A blokk akkor fut le, ha a node jelen van
}

```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Kerüld a feltételeket

Tudom, ez lehetetlen feladatnak tűnik. Hogyan is működhet bármi egy jó kis `if` nélkül? A válasz sok esetben a polimorfizmus. Rendben van, de ilyenkor felmerül a második kérdés: miért tennék ilyet magammal? Erre a válasz egy korábbi Clean Code alapelvben rejlik, miszerint egy függvény szigorúan csak egy dolgot csináljon. Ha `if`-ekkel terhelt osztályokat és függvényeket írsz, ott a függvényed bizony több dologra lesz képes. Emlékezz a főszabályra: egy függvény, egy funkció.

**Rossz:**

```java
public class Airplane {
    private String type;

    public Airplane(String type) {
        this.type = type;
    }

    public int getCruisingAltitude() {
        switch (type) {
            case "777":
                return getMaxAltitude() - getPassengerCount();
            case "Air Force One":
                return getMaxAltitude();
            case "Cessna":
                return getMaxAltitude() - getFuelExpenditure();
            default:
                throw new IllegalArgumentException("Unsupported airplane type: " + type);
        }
    }
}
```

**Jó:**

```java
public class Airplane {
    // minden repülőfajta közös tulajdonsága és metódusa
    private int maxAltitude;

    public Airplane(int maxAltitude) {
        this.maxAltitude = maxAltitude;
    }

    public int getMaxAltitude() {
        return maxAltitude;
    }
}

public class Boeing777 extends Airplane {
    private int passengerCount;

    public Boeing777(int maxAltitude, int passengerCount) {
        super(maxAltitude);
        this.passengerCount = passengerCount;
    }

    public int getCruisingAltitude() {
        return getMaxAltitude() - passengerCount;
    }
}

public class AirForceOne extends Airplane {
    public AirForceOne(int maxAltitude) {
        super(maxAltitude);
    }

    public int getCruisingAltitude() {
        return getMaxAltitude();
    }
}

public class Cessna extends Airplane {
    private int fuelExpenditure;

    public Cessna(int maxAltitude, int fuelExpenditure) {
        super(maxAltitude);
        this.fuelExpenditure = fuelExpenditure;
    }

    public int getCruisingAltitude() {
        return getMaxAltitude() - fuelExpenditure;
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Csak akkor használj type-checkinget, ha mindenképp szükséges

A Java nyelv alapvető jellemzője a típusosság, ami azt jelenti, hogy a változók szigorúan definiált típusokkal rendelkeznek, így nincs szükség külön típusellenőrzésre.

**Rossz:**

```java
public void travelToTexas(Vehicle vehicle) {
    if (vehicle instanceof Bicycle) {
        ((Bicycle) vehicle).pedal(this.currentLocation, new Location("texas"));
    } else if (vehicle instanceof Car) {
        ((Car) vehicle).drive(this.currentLocation, new Location("texas"));
    }
}
```

**Jó:**

```java
public void travelToTexas(Vehicle vehicle) {
    vehicle.move(currentLocation, new Location("Texas"));
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Töröld a már nem használt kódot

A halott kód ugyanolyan rossz, mint a duplikált kód. Semmi okod rá, hogy a kódbázisodban tartsd - ha nem használod a blokkot, szabadulj meg tőle. A verziókezelőd historyjában meg fogod találni a törölt részeket, ha később mégis szükséged lenne rájuk.

**Rossz:**

```java
public class InventoryTracker {
    public static void main(String[] args) {
        String product = "apples";
        String url = "www.inventory-awesome.io";

        Function<String, String> req = InventoryTracker::newRequestModule;
        inventoryTracker(product, req, url);
    }

    public static String oldRequestModule(String url) {
        return "Old Request: " + url;
    }

    public static String newRequestModule(String url) {
        return "New Request: " + url;
    }

    public static void inventoryTracker(String product, Function<String, String> requestModule, String url) {
        String requestResult = requestModule.apply(url);
        System.out.println("Tracking product: " + product);
        System.out.println(requestResult);
    }
}
```

**Jó:**

```java
public class InventoryTracker {
    public static void main(String[] args) {
        String product = "apples";
        String url = "www.inventory-awesome.io";

        Function<String, String> req = InventoryTracker::newRequestModule;
        inventoryTracker(product, req, url);
    }

    public static String newRequestModule(String url) {
        return "New Request: " + url;
    }

    public static void inventoryTracker(String product, Function<String, String> requestModule, String url) {
        String requestResult = requestModule.apply(url);
        System.out.println("Tracking product: " + product);
        System.out.println(requestResult);
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

## **Objektumok és adatstruktúrák**

### Használj gettereket és settereket

A getterek és setterek használata az objektumok adatainak eléréséhez jobb, mint egyszerűen csak közvetlenül elérni egy objektum tulajdonságait. Hogy miért?

- Ha nem csak átvenni akarod egy objektum adott tulajdonságát, nem kell módosítanod a többi kódot, ami hozzáfér ugyanahhoz az adathoz.
- Könnyíti a validáció hozzáadását;
- Könnyen hozzáadható logolás és hibakezelés a get és set során;
- Érvényesíti az adatok egységbe zárásának elvét (encapsulation).

**Rossz:**

```java
public class Person {
    public String name; // Public mező, közvetlenül hozzáférhető

    public Person(String name) {
        this.name = name;
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person("John");
        System.out.println("Name: " + person.name); // közvetlen hozzáférés a változóhoz
        person.name = "Alice"; // Az érték közvetlen módosítása
        System.out.println("Updated Name: " + person.name);
    }
}
```

**Jó:**

```java
public class Person {
    private String name; // Private mező

    public Person(String name) {
        this.name = name;
    }

    // Getter metódus
    public String getName() {
        return name;
    }

    // Setter metódus validációval
    public void setName(String name) {
        if (name != null && !name.isEmpty()) {
            this.name = name;
        } else {
            System.out.println("Invalid name. Name cannot be null or empty.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person("John");
        System.out.println("Name: " + person.getName());
        person.setName("Alice"); 
        System.out.println("Updated Name: " + person.getName());
        
        // Attempting to set an invalid name
        person.setName("");
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

## **Osztályok**

### Fontold meg, hogy új osztályt hozol létre új metódus helyett

Gyakran egyszerűbb olvasható, örökölhető elemekből álló, strukturált metódusdefiníciókkal bíró kódot írni osztályok használatával, mintha kizárólag függvényekre támaszkodnál. Ha előre látod, hogy összetett objektumstruktúrákra van szükség, akkor inkább definiálj új osztályokat. Azonban fontos, hogy csak akkor érdemes osztályokkal kiváltani metódusokat, amikor az alkalmazás összetettsége ezt indokolja.

**Rossz:**

```java
public class MathOperations {
    public static int add(int a, int b) {
        return a + b;
    }

    public static int subtract(int a, int b) {
        return a - b;
    }
}

public class Calculator {
    public int calculate(String operation, int a, int b) {
        if (operation.equals("add")) {
            return MathOperations.add(a, b);
        } else if (operation.equals("subtract")) {
            return MathOperations.subtract(a, b);
        }
        return 0;
    }
}
```

**Jó:**

```java
public abstract class Operation {
    public abstract int execute(int a, int b);
}

public class Addition extends Operation {
    @Override
    public int execute(int a, int b) {
        return a + b;
    }
}

public class Subtraction extends Operation {
    @Override
    public int execute(int a, int b) {
        return a - b;
    }
}

public class Calculator {
    private Operation operation;

    public void setOperation(Operation operation) {
        this.operation = operation;
    }

    public int calculate(int a, int b) {
        if (operation != null) {
            return operation.execute(a, b);
        }
        return 0;
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Használj metódus láncolást

A metódus láncolás egy olyan programozási technika, amely lehetővé teszi több metódus hívását egy objektumon egyetlen, tömör kódsorban. A kód olvashatóságának javítása és az átmeneti változók számának csökkentése érdekében használatos. A metódus láncolás akkor lehetséges, ha minden metódus ugyanazon osztállyal vagy annak egy leszármazottjának az objektumával tér vissza, ami lehetővé teszi egy további metódus hívását a visszatért objektumon.

**Rossz:**

```java
public class Car {
    private String color;
    private String model;
    private String make;

    public void setColor(String color) {
        this.color = color;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public void setMake(String make) {
        this.make = make;
    }

    public void displayInfo() {
        System.out.println("Car Information:");
        System.out.println("Color: " + color);
        System.out.println("Model: " + model);
        System.out.println("Make: " + make);
    }

    public static void main(String[] args) {
        Car myCar = new Car();

        myCar.setColor("Red");
        myCar.setModel("Sedan");
        myCar.setMake("Toyota");
        
        myCar.displayInfo();
    }
}
```

**Jó:**

```java
public class Car {
    private String color;
    private String model;
    private String make;

    public Car setColor(String color) {
        this.color = color;
        return this; // Visszatérés a jelenlegi objektummal
    }

    public Car setModel(String model) {
        this.model = model;
        return this; // Visszatérés a jelenlegi objektummal
    }

    public Car setMake(String make) {
        this.make = make;
        return this; // Visszatérés a jelenlegi objektummal
    }

    public void displayInfo() {
        System.out.println("Car Information:");
        System.out.println("Color: " + color);
        System.out.println("Model: " + model);
        System.out.println("Make: " + make);
    }

    public static void main(String[] args) {
        Car myCar = new Car();

        // Metódus láncolás
        myCar.setColor("Red").setModel("Sedan").setMake("Toyota").displayInfo();
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Használj objektum-összetételt öröklés helyett, ahol csak lehet

Az objektum-összetétel az öröklődés (inheritance) alternatívája. Az öröklődést szokás IS-A kapcsolatnak (the dog is a vertebrate / a kutya egy gerinces), míg az objektum-összetételt HAS-A kapcsolatnak (the dog has a spine / a kutyának van egy gerince) nevezni. Itt az új szolgáltatások úgy jönnek létre, hogy kisebb részekből építünk fel objektumokat, hogy több szolgáltatással rendelkezzenek. Az objektum-összetételnél az összeépített objektumoknak jól meghatározott interfésszel kell rendelkezniük. Az ilyen újrafelhasználást feketedobozos újrafelhasználásnak nevezzük, mert az objektumok belső részei láthatatlanok. Az objektumok „fekete dobozokként” jelennek meg. Az alosztályokon keresztül történő újrafelhasználást fehérdobozos újrafelhasználásnak nevezzük. A „fehér doboz” itt a láthatóságra utal: az öröklődéssel az alosztályok gyakran látják a szülőosztály belső részeit.

Azok az esetek, amikor érdemesebb öröklést használnod:
1. IS-A vs. HAS-A (Ember -> Állat vs. User -> UserDetails).
2. Fel tudsz használni kódot az ősosztályból (az Emberek ugyanúgy mozognak, mint minden Állat).
3. Meg akarod változtatni az összes leszármazott osztályt az ősosztály megváltoztatásával (pl. az Állatok mozgásásának kalóriaigényét változtatod).

**Rossz:**

```java
class Employee {
    private String name;
    private String email;

    public Employee(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // ...
}

// Rossz, mert a munkavállalók adózási adatai nem egy bizonyos fajta munkavállalót jelölnek, hanem olyan dolgok, amije van egy munkavállalónak
class EmployeeTaxData extends Employee {
    private String ssn;
    private double salary;

    public EmployeeTaxData(String name, String email, String ssn, double salary) {
        super(name, email);
        this.ssn = ssn;
        this.salary = salary;
    }

    // ...
}
```

**Jó:**

```java
class Employee {
    private String name;
    private String email;
    private EmployeeTaxData taxData;

    public Employee(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // ...

    public void setTaxData(String ssn, double salary) {
        this.taxData = new EmployeeTaxData(ssn, salary);
    }

    // ...
}

class EmployeeTaxData {
    private String ssn;
    private double salary;

    public EmployeeTaxData(String ssn, double salary) {
        this.ssn = ssn;
        this.salary = salary;
    }

    // ...
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

## **SOLID**

Az objektumorientált programozásban a SOLID egy mozaikszó, amely az öt tervezési alapelv (Single responsibility principle, Open/closed principle, Liskov substitution principle, Interface segregation principle, Dependency inversion principle) kezdőbetűjéből áll, és célja, hogy a szoftvertervezést még érthetőbbé, rugalmasabbá és karbantarthatóbbá tegye.

### Single Responsibility Principle (SRP)

Avagy az "egyetlen felelősség elve". A Clean Code szerint "Soha nem lehet egynél több oka annak, hogy egy osztály megváltozzon". A lényege, hogy minden osztálynak egyetlen felelősséget kell lefednie, de azt teljes mértékig. Amennyiben egy osztály nem fedi le teljesen a saját felelősségi körét, akkor muszáj lesz implementációra programozni, hogy egy másik osztály megvalósítsa azokat a szolgáltatásokat, amik kimaradtak az osztályból. Amikor egy osztály több felelősségi kört is ellát, akkor sokkal jobban ki van téve a változásoknak, mintha csak egy felelősséget látna el, ráadásul módosítások esetén nehéz lehet megérteni, hogy ez hogyan hat a kódbázis fennmaradó részére.

**Rossz:**

```java
public class UserSettings {
    private User user;

    public UserSettings(User user) {
        this.user = user;
    }

    public void changeSettings(Settings settings) {
        if (verifyCredentials()) {
            // Implementáció...
        }
    }

    private boolean verifyCredentials() {
        // Implementáció...
        return true;
    }
}
```

**Jó:**

```java
public class UserAuth {
    private User user;

    public UserAuth(User user) {
        this.user = user;
    }

    public boolean verifyCredentials() {
        // Implementáció...
        return true;
    }
}

public class UserSettings {
    private User user;
    private UserAuth auth;

    public UserSettings(User user) {
        this.user = user;
        this.auth = new UserAuth(user);
    }

    public void changeSettings(Settings settings) {
        if (auth.verifyCredentials()) {
            // Implementáció...
        }
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Open/Closed Principle (OCP)

Avagy a nyílt/zárt elv". Lényege, hogy az osztályhierarchiánk legyen nyitott a bővítésre, de zárt a módosításra. Ez az jelenti, hogy új alosztályt vagy egy új metódust nyugodtan felvehetünk, de meglévőt nem írhatunk felül. Ennek azért van értelme, mert ha már van egy működő, letesztelt, kiforrott metódusunk, és azt megváltoztatjuk, akkor több hátrányos dolog is történhet: a változás miatt az eddig működő ágak hibássá válhatnak, illetve a változás miatt a tőle implementációs függőségben lévő kódrészek megváltoztatására is szükség lehet.

**Rossz:**

```java
class ShapeCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.getRadius() * circle.getRadius();
        } else if (shape instanceof Square) {
            Square square = (Square) shape;
            return square.getSideLength() * square.getSideLength();
        }
        // Ha újabb sokszöget határozok meg újabb osztályban, ezt az osztályt módostanom kell
        return 0;
    }
}

class Circle {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }
}

class Square {
    private double sideLength;

    public Square(double sideLength) {
        this.sideLength = sideLength;
    }

    public double getSideLength() {
        return sideLength;
    }
}

```

**Jó:**

```java
abstract class Shape {
    public abstract double calculateArea();
}

class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Square extends Shape {
    private double sideLength;

    public Square(double sideLength) {
        this.sideLength = sideLength;
    }

    @Override
    public double calculateArea() {
        return sideLength * sideLength;
    }
}

class Triangle extends Shape {
    private double base;
    private double height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Liskov Substitution Principle (LSP)

Avagy a "Liskov-féle behelyettesítési alapelv". Ez az elv egy egyszerű gondolat bonyolult megfogalmazása. Formálisan az objektumok helyettesíthetőségének tétele így hangzik: ha S a T altípusa, akkor egy programban a T típusú objektumok helyettesíthetők S típusú objektumokkal anélkül, hogy a program kívánatos tulajdonságai (pl. a helyesség) megváltoznának.

**Rossz:**

```java
class Rectangle {
    private double width;
    private double height;

    public Rectangle() {
        this.width = 0;
        this.height = 0;
    }

    public void setColor(String color) {
        // ...
    }

    public void render(double area) {
        // ...
    }

    public void setWidth(double width) {
        this.width = width;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getArea() {
        return this.width * this.height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(double width) {
        this.width = width;
        this.height = width;
    }

    @Override
    public void setHeight(double height) {
        this.width = height;
        this.height = height;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Rectangle> rectangles = new ArrayList<>();
        rectangles.add(new Rectangle());
        rectangles.add(new Rectangle());
        rectangles.add(new Square());

        renderLargeRectangles(rectangles);
    }

    public static void renderLargeRectangles(List<Rectangle> rectangles) {
        rectangles.forEach(rectangle -> {
            rectangle.setWidth(4);
            rectangle.setHeight(5);
            double area = rectangle.getArea(); // Négyzet esetén ez a helyes 25 helyett 20-at ad vissza
            rectangle.render(area);
        });
    }
}
```

**Jó:**

```java
import java.util.ArrayList;
import java.util.List;

class Shape {
    public void setColor(String color) {
        // Implementáció...
    }

    public void render(double area) {
        // Implementáció...
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public double getArea() {
        return this.width * this.height;
    }
}

class Square extends Shape {
    private double length;

    public Square(double length) {
        this.length = length;
    }

    public double getArea() {
        return this.length * this.length;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Shape> shapes = new ArrayList<>();
        shapes.add(new Rectangle(4, 5));
        shapes.add(new Rectangle(4, 5));
        shapes.add(new Square(5));

        renderLargeShapes(shapes);
    }

    public static void renderLargeShapes(List<Shape> shapes) {
        shapes.forEach(shape -> {
            double area = shape.getArea();
            shape.render(area);
        });
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Interface Segregation Principle (ISP)

Avagy "interfész elválasztási elv". A tétel úgy tartja, egyetlen kliens se legyen rákényszerítve arra, hogy olyan eljárásoktól függjön, amelyeket nem is használ. Az ISP a nagyon nagy interfészeket kisebbekre és sokkal specifikusabbakra osztja fel, így a klienseknek csak azokról a metódusokról kell tudniuk, amelyeket használnak. Az ilyen leegyszerűsített interfészeket szerepinterfészeknek is nevezik (role interface).

Ha egy rendszer nagyon sok belső összekapcsolódást tartalmaz, akkor egy kisebb módosítás is számos további változtatást tesz szükségessé a rendszer más helyein. Interfészek intenzív használatával ez elkerülhető.

**Rossz:**

```java
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Manager implements Worker {
    public void work() {
        // Manager-specifikus munka
    }

    public void eat() {
        // Manager-specifikus evés
    }

    public void sleep() {
        // // Manager-specifikus alvás
    }
}

class Programmer implements Worker {
    public void work() {
        // Programozó-specifikus munka
    }

    public void eat() {
        // Programozó-specifikus evés
    }

    public void sleep() {
        // Programozó-specifikus alvás
    }
}
```

**Jó:**

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class Manager implements Eatable, Sleepable {
    public void eat() {
        // Manager-specifikus evés
    }

    public void sleep() {
        // Manager-specifikus alvás
    }
}

class Programmer implements Workable, Eatable {
    public void work() {
        // Programozó-specifikus munka
    }

    public void eat() {
        // Programozó-specifikus evés
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Dependency Inversion Principle (DIP)

Avagy "függőség megfordítási elv". Ez a tétel két alapvetést fektet le:

1. A magas szintű kód nem függ az alacsonyabb szintűtől. Mindkettő absztrakciótól függ;
2. Az absztrakciók nem függenek a részletektől. A részletek absztrakciótól függenek.

Ez azt jelenti, hogy a részleteknek és a magas szintű moduloknak is absztrakciókra vagy interfészekre kell támaszkodniuk, ahelyett, hogy közvetlenül egymásra támaszkodnának. A DIP alkalmazásával a függőségi irány megfordul. Ahelyett, hogy a magas szintű modulok vezérelnék a részleteket, a részletek vezérlik a magas szintű modulokat. Ezáltal a részletek kicserélhetők, bővíthetők, vagy módosíthatók anélkül, hogy a magas szintű modulokat érintenék. Az absztrakciók és interfészek használata elősegíti a kódújrafelhasználást, mivel ugyanazt az interfészt vagy absztrakciót használhatjuk fel több osztály vagy modul számára.

**Rossz:**

```java
public class LightBulb {
    public void turnOn() {
        System.out.println("LightBulb: Bulb turned on...");
    }
    public void turnOff() {
        System.out.println("LightBulb: Bulb turned off...");
    }
}

public class ElectricPowerSwitch {
    public LightBulb lightBulb;
    public boolean on;
    public ElectricPowerSwitch(LightBulb lightBulb) {
        this.lightBulb = lightBulb;
        this.on = false;
    }
    public boolean isOn() {
        return this.on;
    }
    public void press(){
        boolean checkOn = isOn();
        if (checkOn) {
            lightBulb.turnOff();
            this.on = false;
        } else {
            lightBulb.turnOn();
            this.on = true;
        }
    }
}
```

**Jó:**

```java
public interface Switch {
    boolean isOn();
    void press();
}

public interface Switchable {
    void turnOn();
    void turnOff();
}

public class ElectricPowerSwitch implements Switch {
    public Switchable client;
    public boolean on;
    public ElectricPowerSwitch(Switchable client) {
        this.client = client;
        this.on = false;
    }
    public boolean isOn() {
        return this.on;
    }
   public void press(){
       boolean checkOn = isOn();
       if (checkOn) {
           client.turnOff();
           this.on = false;
       } else {
             client.turnOn();
             this.on = true;
       }
   }
}

public class LightBulb implements Switchable {
    @Override
    public void turnOn() {
        System.out.println("LightBulb: Bulb turned on...");
    }
    @Override
    public void turnOff() {
        System.out.println("LightBulb: Bulb turned off...");
    }
}

public class Fan implements Switchable {
    @Override
    public void turnOn() {
        System.out.println("Fan: Fan turned on...");
    }
    @Override
    public void turnOff() {
        System.out.println("Fan: Fan turned off...");
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

## **Tesztelés**

Az alapos tesztelés fontosabb, mint az azonnali szállítás. Ha nincsenek tesztek, vagy vannak, de nem megfelelő mennyiségben, akkor sosem lehetsz biztos abban, hogy vajon elrontottál-e valamit. Annak eldöntése, hogy mi számít megfelelő mennyiségű tesztnek, a te dolgod, de a 100%-os lefedettség nem árt ahhoz, hogy nyugodtan alhass.

Nincs mentség a tesztek mellőzésére. Kivétel nélkül minden új üzleti funkció vagy modul esetében írj teszteket. Jó, ha a TDD (Test Driven Development) elv mentén haladsz, de ez nem követelmény - a lényeg, hogy legyen meg a kitűzött lefedettségi cél, mielőtt élesítesz egy új funkciót vagy véglegesítesz egy refaktorálást.

### Egy teszt mindig egyetlen dologra fókuszáljon

**Rossz:**

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class MomentJSTest {

    @Test
    public void testHandlesDateBoundaries() {
        MomentJS date;

        date = new MomentJS("1/1/2015");
        date.addDays(30);
        assertEquals("1/31/2015", date.toString());

        date = new MomentJS("2/1/2016");
        date.addDays(28);
        assertEquals("02/29/2016", date.toString());

        date = new MomentJS("2/1/2015");
        date.addDays(28);
        assertEquals("03/01/2015", date.toString());
    }
}
```

**Jó:**

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class MomentJSTest {

    @Test
    public void testHandles30DayMonths() {
        MomentJS date = new MomentJS("1/1/2015");
        date.addDays(30);
        assertEquals("1/31/2015", date.toString());
    }

    @Test
    public void testHandlesLeapYear() {
        MomentJS date = new MomentJS("2/1/2016");
        date.addDays(28);
        assertEquals("02/29/2016", date.toString());
    }

    @Test
    public void testHandlesNonLeapYear() {
        MomentJS date = new MomentJS("2/1/2015");
        date.addDays(28);
        assertEquals("03/01/2015", date.toString());
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

## **Hibakezelés**

A hibaüzenet egy jó dolog. Ha ilyet látsz, az azt jelenti, hogy a runtime sikeresen észlelte a hibás működést, megtette a szükséges lépéseket (pl. futtatás leállítása), és erről értesített is.

### Kezdj valamit az elkapott hibákkal

Ha nem csinálsz semmit egy elkapott hibával, akkor nincs lehetőséged sem kijavítani azt, sem reagálni rá. Sokszor a konzolba kiírt hibaüzenet sem elégséges, mivel az elveszhet a többi, nem közvetlen a hibához tartozó konzol log között. Ha vetted a fáradságot és írtál egy `try/catch` blokkot, az azt jelenti, hogy ott potenciálisan hibára számítasz, tehát érdemes megtervezned, mi történjen hiba esetén.

**Rossz:**

```java
try {
    functionThatMightThrow();
} catch (Exception e) {
    System.out.println(e);
}
```

**Jó:**

```java
try {
    functionThatMightThrow();
} catch (Exception e) {
    // 1. opció: hibaüzenet rögzítése a konzolban
    System.err.println(e);

    // 2. opció: felhasználó értesítése a hibáról
    notifyUserOfError(e);

    // 3. opció: valamilyen szolgáltatás, külső folyamat értesítése a hibáról
    reportErrorToService(e);

    // A fenti lehetőségek magukban és együttesen is használhatóak
}
```

### Aszinkron műveletek esetén is kezeld a hibákat

Az aszinkron műveletek olyan folyamatokat jelentenek, amelyek nem blokkolják a programot, és a végükre való várakozás helyett azonnal továbbhalad a kód. Ilyen műveletek például hálózati kérések vagy hosszabb számítások.

A Promise-ok lehetővé teszik a fejlesztők számára, hogy kezeljék és ellenőrizzék ezeket az aszinkron folyamatokat. Egy Promise azt ígéri, hogy később valamilyen értéket vagy eredményt szolgáltat, vagy jelzi a folyamat sikertelenségét egy hiba esetén. A Promise-ok kiválóan alkalmasak arra, hogy strukturált módon dolgozzunk az aszinkronitással, és hatékonyan kezeljük az esetlegesen felmerülő hibákat.

A Promise-ok hibakezelése Java-ban például a try-catch szerkezet segítségével történhet. A Promise sikertelen kimenetelét a catch blokkal kezelhetjük, ahol leírhatjuk azokat a műveleteket, amelyeket a hiba esetén szeretnénk végrehajtani. Ez lehet például a hibajelzés, naplózás vagy alternatív érték visszaadása. A Promise-ok és a megfelelő hibakezelés nagyban hozzájárulhatnak az alkalmazások stabilitásához és megbízhatóságához, különösen az aszinkron műveletekkel dolgozva.

**Rossz:**

```java
import java.util.concurrent.CompletableFuture;

public class PromiseExample {
    public static void main(String[] args) {
        CompletableFuture<Integer> result = getData()
            .thenApply(data -> {
                try {
                    return functionThatMightThrow(data);
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            });

        result.handle((value, error) -> {
            if (error != null) {
                System.out.println(error.getMessage());
            }
            return null;
        });
        
        // Terminálművelet a CompletableFuture lánc indításához
        result.join();
    }

    public static CompletableFuture<Integer> getData() {
        return CompletableFuture.supplyAsync(() -> 42);
    }

    public static int functionThatMightThrow(int data) {
        try {
            if (data < 0) {
                throw new Exception("Data is negative!");
            }
            return data * 2;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

```

**Jó:**

```java
import java.util.concurrent.CompletableFuture;

public class PromiseExample {
    public static void main(String[] args) {
        CompletableFuture<Integer> result = getData()
            .thenApply(data -> {
                try {
                    return functionThatMightThrow(data);
                } catch (Exception e) {
                    handleErrors(e);
                    return null;
                }
            });
        
        // Terminálművelet a CompletableFuture lánc indításához
        result.join();
    }

    public static CompletableFuture<Integer> getData() {
        // Egy aszinkron művelet szimulálása
        return CompletableFuture.supplyAsync(() -> 42);
    }

    public static int functionThatMightThrow(int data) throws Exception {
        if (data < 0) {
            throw new Exception("Data is negative!");
        }
        return data * 2;
    }

    public static void handleErrors(Throwable error) {
        // 1. opció: hibaüzenet rögzítése a konzolban
        System.err.println(error.getMessage());
        
        // 2. opció: felhasználó értesítése a hibáról
        notifyUserOfError(error);
        
        //  3. opció: valamilyen szolgáltatás, külső folyamat értesítése a hibáról
        reportErrorToService(error);
    }

    public static void notifyUserOfError(Throwable error) {
        // Felhasználó értesítése a hibáról (pl. felugróablak)
        System.out.println("Notifying user of error: " + error.getMessage());
    }

    public static void reportErrorToService(Throwable error) {
        // Hiba jelzése egy külső szolgáltatás felé (pl. egy API kérés küldése)
        System.out.println("Reporting error to service: " + error.getMessage());
    }
}

```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

## **Formázás**

A formázás szubjektív. Mint a legtöbb itt leírt szabály esetében, itt sincs kőbe vésett igazság, amit mindenkinek követnie kell. Sok különböző formázást [automatizáló eszköz](https://github.com/google/google-java-format) létezik. Használj egyet, mert a fejlesztőknek idő- és pénzpocsékolás a formázáson vitatkozni.

### Alkalmazd következetesen a kis- és nagybetűket

A Java-ban jól bevált névkonvenciók vannak, amelyeket a közösség széles körben követ.

**Jó:**

```java
// Osztályok: PascalCase
public class PersonDetails{}

// Interfészek: PascalCase
interface ActionLogger{}

// Függvény (metódus) nevek: camelCase
getUserDetails();

// Változónevek: camelCase
String employeeId;

// Konstans értékek: végig nagybetűvel, _-sal elválasztva
public static final int MILLISECONDS_IN_A_DAY;

// Csomag nevek: végig kisbetűvel, .-tal elválasztva
com.example.myapp

// Enumok: PascalCase, majd az értékek végig nagybetűvel
enum DayOfWeek {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

// Annotáció: @, majd PascalCase
@Override
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### A hívó és meghívott függvényeket helyezd közel egymáshoz

Ha egy függvény meghív egy másikat, tartsd ezeket a függvényeket függőlegesen közel egymáshoz a forráskódban Ideális esetben a hívó függvény közvetlenül a hívott függvény fölött található meg. Hajlamosak vagyunk a kódot felülről lefelé olvasni, akár egy újságot, így érdemes a kódot is eszerint tagolni.

**Rossz:**

```java
public class PerformanceReview {
    private Employee employee;

    public PerformanceReview(Employee employee) {
        this.employee = employee;
    }

    public void lookupPeers() {
        // ...
    }

    public void lookupManager() {
        // ...
    }

    public void getPeerReviews() {
        lookupPeers();
        // ...
    }

    public void perfReview() {
        getPeerReviews();
        getManagerReview();
        getSelfReview();
    }

    public void getManagerReview() {
        lookupManager();
    }

    public void getSelfReview() {
        // ...
    }

    public static void main(String[] args) {
        Employee employee = new Employee();
        PerformanceReview review = new PerformanceReview(employee);
        review.perfReview();
    }
}

```

**Jó:**

```java
public class PerformanceReview {
    private Employee employee;

    public PerformanceReview(Employee employee) {
        this.employee = employee;
    }

    public void perfReview() {
        getPeerReviews();
        getManagerReview();
        getSelfReview();
    }

    public void getPeerReviews() {
        lookupPeers();
        // ...
    }

    public void lookupPeers() {
        // ...
    }

    public void getManagerReview() {
        lookupManager();
    }

    public void lookupManager() {
        // ...
    }

    public void getSelfReview() {
        // ...
    }

    public static void main(String[] args) {
        Employee employee = new Employee();
        PerformanceReview review = new PerformanceReview(employee);
        review.perfReview();
    }
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

## **Kommentek**

### Kommentben csak üzleti logikát magyarázz

A komment nem elvárás, hanem bocsánatkérés. A szép kód _jórészt_ önmagát dokumentálja. Ha még csak tanulod a programozást, használj nyugodtan annyi kommentet, amennyire csak szükséged van a megértéshez.

**Rossz:**

```java
public static int hashIt(String data) {
    // Hash inicializálása
    int hash = 0
    // Az input string hosszának megadása
    int length = data.length()

    // Végigiterálás a string minden egyes karakterén
    for (int i = 0; i < length; i++) {
        // Az adott karakterhez tartozó kód változóba emelése
        char currentChar = data.charAt(i);
        int charCode = (int) currentChar
        // A hash frissítése a megadott formula alapján
        hash = (hash << 5) - hash + charCode
        // A hash 32 bites integerré konvertálása
        hash &= hash;
    }
    // Visszatérés a végső hash értékkel
    return hash;
}
```

**Jó:**

```java
public static int hashIt(String data) {
    int hash = 0;
    int length = data.length;

    for (int i = 0; i < length; i++) {
        char currentChar = data.charAt(i);
        int charCode = (int) currentChar;
        hash = (hash << 5) - hash + charCod 

        // A hash 32 bites integerré konvertálása
        hash &= hash;
    }

    return hash;
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Ne hagyj kikommentezett kódot a kódbázisban

Okkal használunk verziókezelőt. A régi kódot megőrizzük a historyban.

**Rossz:**

```java
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Jó:**

```java
doStuff();
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Ne naplózz kommentben

Újfent, használj verziókezelőt! Nincs szükség hosszadalmas naplózásra, a `git log` paranccsal előhívható a history.

**Rossz:**

```java
/**
 * 2022-12-20: Visszaírtam a változóneveket (RM)
 * 2022-10-01: Átírtam a változóneveket (JP)
 * 2022-02-03: Töröltem a kapcsolatot az adatbázissal (LI)
 * 2011-03-14: Felállítottam a kapcsolatot az adatbázissal(JR)
 */
public static int combine(int a, int b) {
    return a + b;
}
```

**Jó:**

```java
public static int combine(int a, int b) {
    return a + b;
}
```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**

### Kerüld a "formázó" kommentek használatát

A legtöbbször csak zavaróak. A függvény- és változónevek a megfelelő behúzásokkal és formázással elegendő struktúrát biztosítanak a kód számára.

**Rossz:**

```java
public class Main {
    public static void main(String[] args) {
        //////////////////////////////////////////////////////////////
        // Scope Model Instantiation
        //////////////////////////////////////////////////////////////
        Map<String, String> model = new HashMap<>();
        model.put("menu", "foo");
        model.put("nav", "bar");

        //////////////////////////////////////////////////////////////
        // Action setup
        //////////////////////////////////////////////////////////////
        Actions actions = new Actions();
        actions.setupActions();
    }
}

class Actions {
    public void setupActions() {
        // ...
    }
}

```

**Jó:**

```java
public class Main {
    public static void main(String[] args) {
        Map<String, String> model = new HashMap<>();
        model.put("menu", "foo");
        model.put("nav", "bar");

        Actions actions = new Actions();
        actions.setupActions();
    }
}

class Actions {
    public void setupActions() {
        // ...
    }
}

```

**[⬆ ugrás a tetejére](#tartalomjegyzék)**
