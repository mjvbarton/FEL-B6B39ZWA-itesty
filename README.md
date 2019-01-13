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

## 4. Instalace

V současné fázi aplikace nepočítá s instalací na vícero serverech. Postup pro případnou instalaci je však následující. Instalaci aplikace provedete rozbalením adresáře z gitu do kořenového adresáře webového prostoru. V adresáři *install* pak naleznete skripty v DDL, které importujete do databáze:
    * db-structure.sql
    * db-sample-data.sql

## 5. Uživatelská příručka

Uživatelskou příručku naleznete [zde](https://itesty.mjvbarton.cz)

## 6. Souhrn použití aplikace

Při analýze problému jsem definoval 5 skupin uživatelů: *návštěvníka stránky*, *studenta*, *učitele*, *správce školy* a *superuživatele*. Pro potřeby této verze budou implementovány pouze první dvě skupiny.

### Guest (návštěvník stránky)

Host stránek, který se může přihlásit do systému (UC001). Může číst nápovědu (UC002).


### User (student)

Přihlášený uživatel, který si může vygenerovat nový test tím, že vybere tématický okruh testu (UC101). Poté nastaví jeho další parametry (UC102) Poté co uživatel odešle test k vyplnění je test vyhodnocen, uložen do databáze (UC103) a následně zobrazen uživateli (UC104). Uživatel si také může zobrazit historii již vygenerovaných testů (UC105) a zobrazit si je (UC104) nebo v případě, že je test nevyplní i vyplnit (UC102). Uživatel také může přidávat do databáze vlastní testové otázky (UC106). Může si také zobrazit svůj profil (UC107) a změnit heslo (UC108). Uživatel se může odhlásit (UC109)

## 7. Popis případů užití

### UC001 Přihlášení do systému

*Guest* si zobrazí hlavní stránku pro návštěvníky (V01). Do přihlašovacího formuláře v horní části stránky zadá email, pod kterým je registrovaný v systému a své heslo. Kliknutím na tlačítko přihlášení dojde nejprve k validaci formuláře - je ověřeno, zda uživatel vyplnil email a heslo a zda data splňují maximální dělku datového typu v databázi. Validace probíhá nejdřív v klinetském prostředí v Javascriptu a poté jsou data odeslána na server, kde jsou opět zvalidována.
Pokud jsou data validní je *Guest* přihlášen do systému a stává se *Userem*. Po přihlášení je přesměrován na stránku pro generování testu. (V11)

### UC002 Zobrazení nápovědy

Po kliknutí na tlačítko nápovědy se první stránka nápovědy na (V02). Pomocí postranního menu nebo na základě url je uživateli vykreslen soubor s daným obsahem nápovědy. Soubory jsou uloženy ve formátu Markdown a jsou převáděny pomocí externí knihovny *Parsedown* do HTML.

### UC101 Generování testu

Na začátku je uživateli zobrazena obrazovka pro výbět tématu (V11). Uživatel si nejprve vybere předmět z menu v levém postranním panelu. Následně je uživateli v tabulce zobrazen seznam dostupných tematických okruhů. Hledaná témata může uživatel vyhledávat pomocí filtru nad tabulkou. Ten je ovládán pomocí tlačítek *Vyhledat* a *Filtr*. Kliknutí na tyto tlačítka vyvolá odeslání vyhledávacího formuláře, který překreslí stránku. V poli pro fulltextové vyhledávání je použit našeptávač, který je obsluhován pomocí AJAXem.

Výběr tématu provede uživatel zašktrnutím příslušného tématu a kliknutím na tlačítko pokračovat. Pokud téma obsahuje menší počet otázek než 5, nelze vybrat. V případě, že uživatel, žádná pole nezašktrl zobrazí se mu chybová hláška v podobě statusboxu pod titulkem stránky.

### UC102 Nastavení parametrů testu

Toto užití se aktivuje po přijetí formuláře z UC101. Uživateli zobrazena obrazovka (V12) s dalšími parametry testu. V této verzi je to pouze počet otázek, který je tvořen *dropdownem*, který zobrazí hodnotu 5 a pak všechna čísla dělitelná 10, která jsou menší než je počet ptázek a která nejsou větší než 50. Po kliknutí na tlačítko odeslat se vygeneruje nový test.

### UC103 Vyplnění testu

Na základě url je uživateli zobrazen nevyplněný test, který je tvořen otázkami (V13). Pokud uživatel klikne na tlačítko *Opustit test* odejde z testu na zpět na UC101. Test zůstává stále uložen v databázi a jeví se jako nevyplněný. Pokud uživatel klikne na tlačítko *Vyhodnotit* je uživateli dialog, který se uživatele ptá, zda si zkontroloval odpovědi. V případě, že uživatel klikne na tlačítko *Ne* může pokračovat vyplňování, v opačném případě jsou data z testu odeslána k validaci na server. Na serveru je ověřeno, jsou odpovědi zformátovány (jsou ořezány mezery v řetězcích) a jsou přiřazeny k otázkám. Poté jsou otázky předány v vyhodnocení testu. Po vyhodnocení je uživatel přesměrován na UC104.

### UC104 Zobrazení vyplněného testu

Na základě url je uživateli zobrazen již vyplněný test. Pokud test neexistuje, je uživatel přesměrován na UC101, pokud je test nevyplněn, je uživatel přesměrován na UC103. Jsou načteny data z databáze a test je znovu vyhodnocen, a jsou zobrazeny otázky s výsledkem validace.

### UC105 Zobrazení napsaných testů

### UC106 Přidání testové otázky

### UC107 Uživatelský profil

### UC108 Změna hesla

### UC109 Odhlášení

## 8. Uživatelské rozhraní

Uživatelské rozhraní je detailně popsáno v nápovědě.

## 9. Popis workflow

### W01 Přihlášení

### W02 Generování testu

### W03 Vyhodnocení testu

### W04 Uložení testové otázky do databáze

### W05 Změna hesla

### W06 Validace formulářových polí na serveru

### W07 Front controller

## 10. Úložiště

### Adresářová struktura

```
.
|
+-- /assets                     ** Serverová strana aplikace
|       +-- /control                    ** Action controllers
|       |       +-- /api                    
|       |       +-- /question               ** Akce pro práci s otázkami (volané z question.php)
|       |       +-- /test                   ** Akce pro práci s testy (volané z test.php)
|       |       +
|       +-- /extlib                     ** Externí knihovny
|       |       +-- /Parsedown
|       |       +
|       +-- /lib                        ** Vlastní knihovny
|       +-- /models                     ** Modely (MVC)
|       +-- /views                      ** View (MVC)
|       |       +-- /itest                  ** Rendering testů
|       |       +-- /menu                   ** Rendering uživatelských nabídek
|       |       +-- /misc                   ** Pomocné renderovací knihovny
|       |       +-- /static                 ** Statický obsah stránek
|       |       +-- /user                   ** Rendering akcí s uživatelem
|       +       +
+-- /install                    ** Instalační skripty pro databázi
|       +
+-- /resources                  ** Zdrojové materiály pro klientskou stranu
|       +-- /css
|       +-- /img
|       |       +-- /help                   ** Obrázky v nápovědě
|       |       +-- /profiles               ** Profilové fotky (v beta verzi pouze jedna defaultní)
|       |       +
|       +-- /js
|       |       +-- /ajax                   ** Knihovny pro ajax
|       |       +-- /control                ** Ovládací skript pro každý view (MVC)
|       |       +-- /misc                   ** Různé javascriptové knihovny
|       |       +
|       +-- /md                         ** Statický textový obsah stránek
|       |       +-- /help                   
|       +
+
        
```

## 11. Zabezpečení

## 12. Zpracování dat

## 13. Databáze

## 14. Požadavky mimo rozsah projektu

## 15. Nefunkční požadavky

        