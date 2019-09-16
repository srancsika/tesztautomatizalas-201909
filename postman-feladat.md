# RESTful webszolgáltatások és Postman gyakorlati feladatok

Adott egy alkalmazás a `http://www.learnwebservices.com/crystalball` címen.

Ebben az alkalmazásba a felhasználók jóslatokat fogalmazhatnak meg, amit a rendszer titkosít.
Megadható, hogy mikor legyen olvasható a jóslat, akkor kinyílik, és meg lehet nézni annak tartalmát.
 
Felhasználói felülettel nem rendelkezik, csupán RESTful webszolgáltatásokat biztosít, melyeken keresztül más 
alkalmazások férhetnek hozzá. A `http://www.learnwebservices.com/crystalball/api/messages/` címen érhető el
(vigyázz a perjelre a végén!).

Az alkalmazás tartalmaz (legalább) egy hibát. Keresd meg!

Jóslatot lehet felvenni, listázni, módosítani, valamint törölni. Listázni lehet az összes jóslatot, de lehet egyet is.
A rendszer a jóslatoknak egyedi azonosítót ad.

## Feladatok

Hozz létre egy Postman collectionben négy kérést, minden egyes műveletre!
Írj JavaScript assertet is!

Hozz létre egy tesztesetet, új collectiont, mely két kérésből áll, az első felvisz egy új jóslatot, a második lekéri, és
ellenőrzi, hogy a szöveg le van-e titkosítva. (Nem egyenlőség vizsgálat, hanem nem-egyenlőség vizsgálat! Használd
a `!=` operátort!)

Milyen teszteseteket írnál még? Mi okoz nehézséget az alkalmazás tesztelésében? 

## API dokumentáció

### Jóslat létrehozása

A `/api/messages/` címre kell elküldeni `POST` metódussal a következő JSON dokumentumot:

```javascript
{
	"content": "Veszedelmes borztámadás lesz...",
	"openAt": "2019-09-17T10:00:00",
	"creator": "Viczi"
}
```

### Jóslatok listázása

A `/api/messages/` címen `GET` metódussal.

### Jóslat lekérése

A `/api/messages/{id}` címen `GET` metódussal. Példa: `http://www.learnwebservices.com/crystalball/api/messages/e306c8ca-4ae5-41ab-9f5e-5a193c4a5596`.

### Jóslat módosítása

A `/api/messages/{id}` címen `POST` metódussal. Csak a tartalmat lehet módosítani:

```javascript
{
	"content": "Rozsomáktámadás lesz"
}
```

### Jóslat törlése

A `/api/messages/{id}` címen `DELETE` metódussal.
