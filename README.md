# iTesty (verze 1.0.0 beta) - softwarová dokumentace

**Autor:** Matěj Bartoň

## 1. Shrnutí

Tento dokument popisuje softwarovou část vývoje aplikace iTesty. Tato webová aplikace byla vytvořena primárně jako semestrální práce z předmětu Základy webových aplikací. Sekundární motivací bylo mé současné studium na Pražské konzervatoři. Aplikace slouží k podpoře výuky tím, že umožňuje sdílení probíraného učiva mezi studenty/uživateli aplikace formou testových otázek. Na základě uživatelských požadavků je pak schopna vygenerovat testy a tím umožnit uživatelům, aby si procvičili probíranou látku.

## 2. Zadání úlohy

Vytvořte beta verzi webové aplikace pro podporu výuky studentů. Aplikace bude založenaa na náhodném generování testových otázek, které budou vytvářet uživatelé.

### Akceptační podmínky
* aplikace musí být přístupná bez Javascriptu
* aplikace musí fungovat na nejnovějších verzích prohlížečů Chrome a Firefox
* aplikace musí mít validní html
* aplikace obsahuje přihlášení
* aplikace musí být schopna vygenerovat test z náhodných otázek
* aplikace musí umožnit uživateli, aby pohodlně nastavil parametry testu
* aplikace musí testy správně vyhodnocovat
* aplikace musí uživateli zobrazit historii jeho testů, včetně výsledků, které vyplnil
* aplikace musí uživateli umožnit, aby přidával vlastní testové otázky do databáze
* aplikace obsahuje uživatelskou příručku, která je implementována jako část webových stránek

## 3. Umístění aplikace

V součas

## 4. Uživatelská příručka

Uživatelskou příručku naleznete [zde](https://itesty.mjvbarton.cz)

## 5. Souhrn použití aplikace

Při analýze problému jsem definoval 5 skupin uživatelů: *návštěvníka stránky*, *studenta*, *učitele*, *správce školy* a *superuživatele*. Pro potřeby této verze budou implementovány pouze první dvě skupiny.

### Guest (návštěvník stránky)

Host stránek, který se může přihlásit do systému (UC001). Může číst nápovědu (UC002).


### User (student)

Přihlášený uživatel, který si může vygenerovat nový test (UC101). Po vygenerování test vyplní (UC102). Poté co uživatel odešle test k vyplnění je test vyhodnocen, uložen do databáze (UC103) a následně zobrazen uživateli (UC104). Uživatel si také může zobrazit historii již vygenerovaných testů (UC105) a zobrazit si je (UC104) nebo v případě, že je test nevyplní i vyplnit (UC102). Uživatel také může přidávat do databáze vlastní testové otázky (UC106). Může si také zobrazit svůj profil (UC107) a změnit heslo (UC108).

## 6. Popis případů užití

### UC001 Přihlášení do systému

Guest si zobrazí hlavní stránku pro návštěvníky (V01). Do přihlašovacího formuláře v horní části stránky zadá email, pod kterým je registrovaný v systému a své heslo. Kliknutím na tlačítko přihlášení dojde nejprve k validaci formuláře - je ověřeno, zda uživatel vyplnil email a heslo a zda data splňují maximální dělku datového typu v databázi. Validace probíhá nejdřív v klinetském prostředí v Javascriptu a poté jsou data odeslána na server. Validace formulářových polí je realizována instancí třídy *FormFieldContainer*
        