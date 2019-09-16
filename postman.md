# Postman

# Webes alkalmazások

Azon alkalmazásokat nevezzük webes alkalmazásoknak, melyek felhasználói felülete
böngészőből vehető igénybe. Ezek azért terjedtek el annyira, mert
böngésző feltehetően minden számítógépre van telepítve, és
az alkalmazás igénybevételéhez további telepítésre nincs szükség.

Jelenleg a legelterjedtebb böngészők a Google Chrome, a Mozilla Firefox és a 
Microsoft Edge nevű böngészője. Különböző statisztikák vannak a 
böngészők elterjedésével kapcsolatban (pl. https://gs.statcounter.com/),
de annyi tisztán látszik, hogy a Chrome szélesebb körben használt, 
mint a többi. A Microsoft böngészője a Windows 10-ben jelent meg 2015-ben,
de 2019-ben újraírták Chromium alapokra (mely egy nyílt forráskódú
böngésző alap), melyre a Google Chrome is épül. Persze speciális környezetekben,
pl. bankoknál vagy közigazgazásban más lehet az elterjedtségük (pl. belső policy
  miatt preferált Internet Explorer).

Régebben sok probléma volt, hogy a böngészők bár szabványokra építenek, mégis
másképp működtek (kompatibilitási problémák). Ma már erre vannak megoldások,
azonban tesztelési szempontból még mindig oda kell figyelni.

A webes alkalmazásokat úgy is szokták jellemezni, hogy vékony klienses alkalmazások.
Azért, mert a felhasználó számítógépén csak a felhasználói felület jelenik meg
(a böngészőn belül), és az kommunikál egy (vagy több) központi számítógéppel,
melyet szervernek neveznek. Az üzleti entitások, üzleti logika (üzleti folyamatok), valamint
az adatbázis mind a szerver oldalon jelenik meg. Ezért ezek kliens-szerver
alkalmazások. A szerver valamilyen szolgáltatást biztosít, míg a kliens ezt igénybe 
veszi.

A böngészős technológiának tehát a legfőbb haszna az üzemeltetési könnyebbség,
hogy kliens oldalon nem kell telepíteni a rendszert, valamint amennyiben új
verzió kerül kiadásra, kliens oldalon nem kell frissíteni, elegendő központilag,
szerver oldalon.

A böngészőben futó felhasználói felületet, klienst szokták frontendnek, a
szerver oldalt backendnek nevezni.

![Frontend - backend](https://pics.me.me/frontend-backend-nice-looking-ui-40722684.png)

A böngészős alkalmazások a frontend oldalon következő technológiákra építenek: HTTP, HTML, CSS, JavaScript.
A HTTP protokollt a böngésző és a szerver közötti kommunikációra használják. Az internetes címek is 
ezzel a betűszóval kezdődnek, pl. `http://www.example.com`. 

Amikor a számítógépek közötti kommunikációról beszélünk, gyakran futunk bele az [OSI-modellbe](https://hu.wikipedia.org/wiki/OSI-modell), mely egy leírás, hogyan is kell elképzelnünk a hálózati kommunikációt.
Egyelőre elég annyi, hogy ez egy hétrétegű modell. A különböző rétegeknek különböző implementációi vannak,
alsó rétegekhez tartozik az Ethernet, felsőbb rétegekben a TCP/IP, és erre épül rá a legmagasabb
rétegben elhelyezkedő HTTP, magas szintű protokoll. A protokoll nem más, mint egy szabálygyűjtemény,
ami egy szabványban van rögzítve (RFC), mely leírja, hogy a kommunikáló felek hogyan viselkedhetnek.
Webes alkalmazások esetén elképzelhető úgy, hogy a böngésző és a szerver hogyan kommunikál egymással.
HTTPS protokoll esetén a végpontok között titkosítás történik. Ezt úgy képzeljük, hogy van egy
TLS szabvány szerint működő titkosított csatorna, amiben a bájtok titkosítva utaznak, és a http
mondja meg, hogyan kell ezeket a bájtokat értelmezni.

A http esetén a különböző erőforrásokat, mint pl. weboldalakat, szöveges tartalmakat, képeket,
videókat URL-lel azonosítjuk. Ezt írjuk be fenn a böngészőbe. Ez a következő elemekből áll pl. a
`http://example.com/article/12`:

* Protokoll (`http` vagy `https`): hogyan kommunikál a böngésző a szerverrel, milyen protokoll szerint
* Tartománynév(domain)/ip-cím (`example.com` vagy `192.168.0.64`): ez azonosítja a számítógépet az interneten (azért jelentek meg a domain-nevek, hogy ne a számokat kelljen megjegyezni)
* Port: egy számítógépen belül a különböző szolgáltatatások, szoftverek megkülönböztetésére szolgál, ezzel mondjuk meg, hogy a számítógépen belül mivel szeretnénk kommunikálni
* Elérési út: ezzel különböztetjük meg a különböző oldalakat, erőforrásokat

A HTTP protokoll egy elég régóta használt szabvány még az 1990-es évekből. Arra találták ki, hogy
a tudósok cikkeket kezeljenek egyetemek közötti hálózatokon, ezeket meg tudják osztani, le tudják kérni, és egymásra tudjanak hivatkozni.

A HTTP kommunikáció legfontosabb ismérvei:

* Kliens-szerver kommunikáció.
* A kliens tipikusan a böngésző.
* Kérés-válasz alapú protokoll: mindig a böngésző kérdez, és a szerver válaszol.
* Szöveges: ember és számítógép által is olvasható, egyszerű szövegszerkesztőben megjeleníthető, azaz karakteres.
* A kliens egy szöveges kérés üzenetet küld, a szerver erre egy szöveges válasz üzenettel válaszol. Minden üzenet tartalmazhat fejlécet és törzset, de nem szükségszerűen.
* Állapotmentes, azaz két egymás utáni kérés független egymástól.

A HTTP kommunikációt meg lehet jeleníteni a böngészőkben is, létezik bennük egy fejlesztői eszköztár (`F12` billentyű megnyomásával hívható elő - DevTools - és itt a Network fülre kattintva nézhetjük meg
  a kéréseket és válaszokat.)
  
Ami érdekes, hogy egy oldal lekérésekor sok HTTP kérést és választ látunk. Ez azért van,
mert egy oldal tipikusan egy szöveges HTML fájl, mely hivatkozik sok-sok másik fájlra.
Mint pl. stílust leíró állományok (CSS), hogy nézzen ki egy oldal. Vagy kis kliens oldalon futó,
programocskákra, JavaScript forrásfájlokra. Sőt képekre, videókra, stb. Ezeket a böngésző mind
külön-külön HTTP kérésekben kéri le és kapja vissza. Egy oldal akár 100 feletti ilyen
további állományra hivatkozhat, így ilyen sok HTTP kérés megy a szerver felé.

A HTTP esetén még meg kell említeni a státuszkódokat, amivel a szerver azt mondja meg, hogy
ki tudja-e szolgálni a kéréseket. Mindegyikhez tartozik egy üzenet is, pl. `200` esetén `OK`,
ekkor azt mondja a szerver, hogy sikerült kiszolgálni a kérést, azaz elküldeni a válaszüzenetet.
A másik legismertebb a `404 - Not Found`. A `3xx` kérések esetén a böngészőnek még valamit csinálnia kell,
`4xx` esetén hibás a kérés, `5xx` esetén valami baj van szerver oldalon.

A könnyebb megjegyezhetőséget segíti a https://httpstatusdogs.com/ oldal.

A HTTP metódussal mondjuk meg, hogy milyen irányú adatforgalmat szeretnénk lebonyolítani. Tipikusan, ha 
lekérünk valamit, az HTTP `GET`, ha viszont kitöltünk egy űrlapot, akkor `POST` metódussal mondjuk meg,
hogy adatot küldünk.

Amennyiben adatot kérünk le, akkor a választ a HTTP válasz üzenet törzsében kapjuk, amennyiben 
adatot küldünk, a kérés törzsében küldjük el az űrlapon szereplő mezők neveit, és egyesével
a beírt értékeket.

Plusz információkat úgy is tudunk felküldeni, hogy azokat URL paraméterként adjuk meg. 
Ez tipikusan kereséseknél használjuk, ekkor a keresési feltétel megjelenik az URL-ben,
pl. `http://www.example.com/search?name=John`.

A régebbi webes alkalmazások esetén, amennyiben adatot akartunk küldeni a szervernek,
az egy oldalváltással járt, új oldalt kellett megjeleníteni, és ez lehet, hogy
másképp nézett ki, vagy a legideálisabb esetben is villant egyet. Ennek kiküszöbölésére
megjelent az AJAX technológia, mely segítségével oldalváltás nélkül tudunk a szerverrel
kommunikálni. Ennek egy példája a Google kereső főoldala, amikor elkezdünk beírni egy
keresőfeltételt, és már ajánlásokat jelenít meg. A weboldal minden billentyű lenyomásra
kommunikál a szerver oldallal.

Ezek is pont olyan HTTP kérések és válaszok, mintha külön oldalakat kérnénk le, ezeket is
ugyanúgy lehet monitorozni. A formátum, melyet használni szoktak átvitelre, mely a 
HTTP kérések és válaszok törzsében utazik, a JSON. A JSON egy olyan adatformátum,
mely a böngészőben futó programozási nyelv, a JavaScript segítségével nagyon könnyen
előállítható és feldolgozható.

Összefoglalva: a böngésző HTTP üzenetekkel, AJAX-szal kommunikál a szerver oldallal,
JSON üzeneteket küldve és fogadva. Tehát ez egy megoldás a felhasználói felület és a 
szerver oldal (frontend és backend) közötti kommunikációra. A frontend mindig
JavaScript, azonban a backend bármilyen programozási nyelven írható, erről a kliens oldal
semmit nem tud, csak annyit, hogy HTTP-n kell megszólítani és JSON formátumban kell küldeni az
adatokat.

A példa alkalmazás, mely elérhető a http://www.learnwebservices.com/locations/ címen is
pont ilyen módon működik.

Azonban rájöttek arra, hogy ez a megoldás nem csak kliens és szerver közötti kommunikációra
jó (ezt képzelhetjük el úgy, mint egy rendszer két mondulját), hanem rendszerek közötti 
kommunikációra is.

Ehhez azonban egy kicsit komplexebb modell szükségeltetik, és ezért találta ki Roy Fielding a RESTful
webszolgáltatásokat.

Ő azt mondja, hogy az alkalmazásokra gondoljunk úgy, mint egyedi entitások halmazára, melyeken
a négy alapműveletet (CRUD - create, read, update, delete) lehet elvégezni. Mindegyik
entitás egyedileg címezhető a jól ismert URL-lel. A műveleteket a HTTP metódusokkal lehet 
igénybe vennünk.

Azaz pl. egy kedvenc hely elérhető a `http://www.learnwebservices.com/locations/api/locations/91` címen.
Ezt böngészőbe is beírhatjuk, és ugyanúgy visszakapjuk a JSON választ, mintha a JavaScript kérte volna le.
Ekkor a kérés `GET` metódussal megy el.

Ha ezt módosítani szeretnénk, akkor ugyanerre a címre egy `POST` metódussal kell kérést küldenünk,
és a HTTP kérés törzsében meg kell adnunk a JSON-t, mely a módosítani kívánt adatokat tartalmazza.

Ezen kívül van `DELETE` metódus is, mellyel törölhetünk. Ez így valójában leír egy programozói
interfészt (API), amin keresztül hozzáférhetünk az alkalmazáshoz egy másik programmal. A
példa program teljes API dokumentációja elérhető a `http://www.learnwebservices.com/locations/api.html`
címen.

A `POST` kéréseket nem tudjuk böngészőből ilyen egyszerűen elküldeni, és erre, valamint 
egy REST API fejlesztésére és tesztelésére elterjedt és kiváló eszköz a [Postman](https://www.getpostman.com/).

Ezzel HTTP kéréseket tudunk indítani a megfelelő URL-re, különböző metódusokkal. A HTTP válasz tartalmát meg tudja jeleníteni (pl. a JSON-t formázza is). A kérés törzsét is meg tudjuk adni. Megadhatjuk a kérés fejlécét, de
a válaszét is láthatjuk. Látjuk a válasz státuszkódját is. A kéréseket kollekciókba rendezhetjük. Megadhatóak teszt assertek is JavaScript nyelven.
Egy kollekcióban szereplő kéréseket egyben futtathatjuk.








