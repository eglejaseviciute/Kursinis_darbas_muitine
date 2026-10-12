# Natūralios kalbos apdorojimo ir aibių siaurinimo metodai siūlant pilnus ar dalinius nomenklatūros prekių kodus pagal muitinės deklaracijų prekių aprašymus:)

## Projekto aprašymas, tikslai ir uždaviniai

Šiame kursiniame darbe nagrinėjama muitinės deklaracijose pateikiamų prekių aprašymų ir deklaruotų Kombinuotosios nomenklatūros (KN) prekių kodų neatitikimo problema.

## Darbo tikslas

Iritaikyti ir įvertinti natūralios kalbos apdorojimo ir aibių siaurinimo metodus, leidžiančius muitinės deklaracijose aptikti prekės aprašymo ir prekės kodo neatitikimus, pasiūlyti tikėtinus atitinkamai pilnus arba dalinius kodus, ir įvertinti tokių neatitikimų paplitimą žaliojo srauto duomenyse.

Tyrime nagrinėjamas **73-iasis Kombinuotosios nomenklatūros skirsnis – gaminiai iš geležies arba plieno**.

## Pagrindiniai uždaviniai

1. Paruošti muitinės hierarchinės struktūros ir tekstinius duomenis analizei.
2. Įvertinti NLP metodų tikslumą siūlant kelis tinkamiausius pilnus KN kodus.
3. Įvertinti aibių siaurinimo metodų tikslumą nustatant dalinius KN kodus.
4. Atlikti žaliojo srauto duomenų problemų diagnostiką ir nustatyti galimai neinformatyvių deklaracijų dalį.

## Duomenys ir konfidencialumas

Tyrimui naudojami **Lietuvos Respublikos muitinės tiesiogiai pateikti duomenys**. Duomenys yra konfidencialūs ir negali būti viešinami.

Dėl šios priežasties pirminiai muitinės duomenys į šią repozitoriją neįtraukti, todėl repozitorijoje esantis kodas negali būti savarankiškai paleistas.

Tyrime naudoti:

* 2023 m. Kombinuotosios nomenklatūros duomenys;
* 2024 ir 2025 m. KN pakeitimai;
* 2023 m. žaliojo srauto deklaracijų duomenys;
* 2024 m. kovo mėn. – 2026 m. kovo mėn. laikotarpio žaliojo srauto deklaracijų duomenys.

2026 m. deklaracijų duomenys iš analizės pašalinti, nes tyrimui nebuvo pateikti atitinkami 2026 m. KN pakeitimai.

Po pradinių duomenų paruošimo liko **195482 deklaracijos**, o pašalinus pasikartojančias eilutes, kuriose sutapo deklaruotas kodas ir prekės aprašymas, liko **40920 įrašų**.

## Duomenų paruošimas

Prekių aprašymai paruošti natūralios kalbos apdorojimo metodams:

* tekstas paverstas mažosiomis raidėmis;
* pašalinti nereikalingi skyrybos ženklai;
* pašalinti dažni, tačiau analizei nereikšmingi žodžiai;
* lietuviškos raidės pakeistos jų nelietuviškais atitikmenimis;
* pritaikytas lietuviškų žodžių kamienų nustatymas;
* išlaikyti skaičiai ir matavimo vienetai, kurie gali būti svarbūs prekių klasifikavimui;
* suvienodinti dešimtainių skaičių atskyrimo simboliai;
* pašalintos pasikartojančios deklaracijos, kai sutapo prekės kodas ir aprašymas.

Pavyzdžiui, skirtingos žodžio formos:

`varžtas`, `varžtai`, `varžtų`

po apdorojimo priskiriamos tam pačiam žodžio kamienui.

## Taikyti metodai

### Natūralios kalbos apdorojimas

Pilnų KN kodų nustatymui išbandyti trys tekstų panašumo metodai:

1. **Deklaracijos žodžių bendrumas** – vertinama, kokia deklaracijos žodžių dalis sutampa su KN aprašymo žodžiais.
2. **Kosinusinis panašumas**
3. **Euklido atstumas**

Kiekvienai deklaracijai atrenkami penki panašiausi KN aprašymai ir tikrinama, ar deklaruotas kodas patenka į:

* Top-1;
* Top-3;
* Top-5

rezultatus.

### Aibių siaurinimas

Dalinių KN kodų nustatymui sukurti du modeliai:

#### Rare

Kandidatų aibė siaurinama naudojant **rečiausiai nomenklatūroje pasitaikančius deklaracijos žodžių kamienus**.

Prielaida – retesni ir specifiškesni terminai gali suteikti daugiau informacijos apie konkrečią prekę.

#### Common

Kandidatų aibė siaurinama naudojant **dažniausiai nomenklatūroje pasitaikančius deklaracijos žodžių kamienus**.

Prielaida – bendresni terminai gali leisti palaipsniui susiaurinti galimų kodų aibę ir išvengti pernelyg specifinių deklaracijos terminų problemos.

Abiem atvejais galutinėje kandidatų aibėje ieškoma ilgiausios bendros KN kodo pradžios.

Vertinami 4, 6, 8 ir 10 skaitmenų kodų lygiai. Pirmieji du skaitmenys nevertinami, nes visos nagrinėjamos prekės priklauso 73-ajam KN skirsniui.

## Pagrindiniai rezultatai

### NLP metodai

Pilno KN kodo nustatymo rezultatai:

| Metodas                       |   Top-1 |   Top-3 |   Top-5 |
| ----------------------------- | ------: | ------: | ------: |
| Deklaracijos žodžių bendrumas |  6,08 % | 14,48 % | 18,17 % |
| Kosinusinis panašumas         | 13,49 % | 27,74 % | 33,37 % |
| Euklido atstumas              | 13,52 % | 27,26 % | 32,88 % |

NLP metodų Top-1 tikslumas neviršijo **13,52 %**, o Top-5 tikslumas siekė iki **33,37 %**. Tai rodo, kad vien tekstiniu žodžių panašumu pagrįsti metodai nėra pakankamai tikslūs pilnam KN kodui nustatyti.

### Aibių siaurinimo metodai

Aibių siaurinimo metodų svertinis tikslumas, neįtraukiant 2 skaitmenų lygio:

| Modelis | Svertinis tikslumas |
| ------- | ------------------: |
| Rare    |             27,65 % |
| Common  |             58,87 % |

Common modelis pasiekė didesnį svertinį tikslumą ir dažniau teisingai nustatė ilgesnes dalines KN kodų pradžias.

### Duomenų problemų diagnostika

Analizėje taip pat nustatyta:

* deklaracijos prekės aprašyme vidutiniškai yra apie **7 žodžiai**, o KN aprašyme – apie **27 žodžiai**;
* didėjant deklaracijos aprašymo žodžių skaičiui, aibių siaurinimo modelių svertinis tikslumas mažėja;
* didėjant nespecifinių žodžių kamieno **„kit“** pasikartojimų skaičiui KN aprašyme, modelių tikslumas taip pat mažėja;
* daugiau nei trečdalis deklaracijų yra susijusios su KN kodais, kurių aprašyme „kit“ kamienas pasikartoja bent 5 kartus;
* iš **195 482** įvertintų deklaracijų **25,34 %** neturėjo nė vieno bendro žodžio tarp deklaracijos aprašymo ir atitinkamo KN kodo aprašymo.

## Darbo apribojimai

Pagrindiniai tyrimo apribojimai:

* vertinant deklaracijas remiamasi prielaida, kad žaliojo srauto duomenyse deklaruotas KN kodas yra teisingas;
* NLP ir aibių siaurinimo metodų rezultatų tiesioginis palyginimas yra ribotas, nes metodai pateikia skirtingo pobūdžio rezultatus.

## Galimos tolesnio tyrimo kryptys

Toliau būtų galima:

* įtraukti semantinius kalbos modelius;
* tobulinti aibių siaurinimo algoritmus;
* atsižvelgti į istorinius KN kodų naudojimo dažnius;
* atskirai modeliuoti nespecifinius „kit“ tipo KN kodus;
* sukurti automatinį deklaracijos prekės aprašymo ir deklaruojamo KN kodo tekstinio atitikimo tikrinimą.

## Projekto aplinka

Darbo programinis kodas parašytas **Python** programavimo kalba **Google Colab** aplinkoje **Jupyter
Notebook** formatu.

Projekto kodas pateiktas faile **Duomenų_mokslo_projektas.ipynb**.

## Autoriai

**Eglė Jasevičiūtė**,
**Karina Stūronaitė**,
**Elzė Voveriūnaitė**
