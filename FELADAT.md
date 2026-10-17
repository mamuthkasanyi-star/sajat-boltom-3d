# FELADAT – a fejlesztőnek (Grok)

**Ez nem prototípus-megrendelés.** Kiadható, teljes értékű mobiljátékot kérünk. Ne álljon meg a munka egy „alap demó”-nál.
Minden mérföldkő végén a játék legyen futtatható, és a következő mérföldkő erre épüljön tovább. A cél a 8. pont szerinti kész játék.

## 1. A játék
3D bolt-menedzser játék a 2015-ös „Én kicsi Tescóm” mintájára, de annál jobb.
A játékos egy üres üzlethelyiségből indul, és nagy, sikeres áruházat épít belőle.
- Motor: **Godot 4.4+**, GDScript.
- Platform: **Android** (arm64 APK, Gradle export, minimum Android 8), másodlagos platform a **Linux x86_64**.
- Nyelv: **magyar** (minden szöveg a `.po`/CSV fordítási fájlban, hogy később angol is jöhessen).
- Tájolás: álló és fekvő is, forgatáskor nem töltődhet újra.

## 2. Kötelező játékmenet
1. **Építés és berendezés.** Rácsalapú elhelyezés: polcok, hűtők, fagyasztók, pékáru-pult, kasszák, önkiszolgáló kassza, raktár, dekoráció.
   Áthúzás, forgatás, eladás, visszavonás. Érvénytelen helyre nem tehető le semmi, mert a járható útnak meg kell maradnia.
2. **Áru.** Legalább 40 termék 8 kategóriában (pékáru, tejtermék, zöldség-gyümölcs, hús, ital, édesség, háztartás, fagyasztott).
   Beszerzési és eladási ár, szavatosság (a friss áru romlik), nagyker-rendelés szállítási idővel.
3. **Vevők.** NavigationRegion3D-alapú útkeresés. Bevásárlólista, kosár, keresés a polcokon.
   Elégedettség: az ár, a választék, a sorban állás, a tisztaság és a dekoráció számít bele.
   Ha valami nem tetszik nekik, vásárlás nélkül távoznak. Egyszerre legalább 30 vevő legyen a boltban, akadás nélkül.
4. **Személyzet.** Pénztáros, árufeltöltő, takarító. Felvétel, bér, fáradtság, képzés.
5. **Gazdaság.** Napi bevétel és kiadás (bér, rezsi, bérleti díj), napi zárás összesítő képernyővel, grafikonok.
   Ár-rugalmasság: drágán kevesebb fogy. Akciók és szórólapok.
6. **Fejlődés.** Szintek, feloldható termékek és berendezések, boltbővítés (új szárny, emelet, parkoló).
   Napi és heti feladatok, eredmények.
7. **Idő.** Napszakok (nyitás, csúcsidő, zárás), hét napjai, ünnepek (karácsony, húsvét) eltérő kereslettel.
8. **Offline haladás.** Zárt appnál is telik az idő, legfeljebb 8 óráig, visszatéréskor összesítővel.
9. **Mentés.** Automatikus és kézi mentés több mentéshellyel, verziózott formátummal (a régi mentés egy újabb verzióval is betölthető legyen).
10. **Saját bolt.** Boltnév és logószerkesztő. A logó megjelenik a cégéren, a szatyrokon és az egyenruhán.

## 3. Grafika – VALÓDI, SZÉP 3D (kötelező, nem alku tárgya)
**Nem elfogadható:** low-poly „kockafigura”, textúra nélküli szürke vagy egyszínű modell, primitívekből (doboz, henger, gömb) összerakott polc vagy ember, emoji vagy 2D sprite a 3D jelenetben, árnyék nélküli lapos fény.

**Szint:** a látvány a „Supermarket Simulator” (Nokta Games, 2024) vagy a „Two Point” játékok minőségét közelítse. Valósághű arányok, igényes, enyhén stilizált PBR-anyagok, meleg, hívogató áruházi fények.

### 3.1 Renderelés (Godot 4.4+)
- Renderelő: **Forward+** asztali gépre, **Mobile** Androidra. A projekt mindkettővel jól nézzen ki.
- **Tónusleképezés:** AgX vagy ACES, enyhe glow (bloom) a lámpákon, színkorrekció (LUT) a meleg áruházi hangulathoz.
- **Fények:**
  - a belső teret LightmapGI-vel előre sütött fények adják (indirekt fény és ambient occlusion);
  - egy dinamikus DirectionalLight szolgáltatja a napot az ablakokon át, árnyékkal;
  - a mennyezeti lámpák emissive anyagok, OmniLight vagy SpotLight korlátozott számban.
- **Tükröződés:** ReflectionProbe a padlón és a hűtők üvegén. A fényes csempepadló tükrözze a polcokat és a lámpákat.
- **Égbolt és környezet:** HDRI a Poly Havenről (CC0), és az ablakon át látszó külső tér (utca, parkoló, fák).
- **Asztali gépen:** SSAO, SSR és SSIL is legyen bekapcsolva.
- **Mobilon:** a sütött AO és a ReflectionProbe pótolja ezeket.

### 3.2 Anyagok és modellek
- Minden felület **PBR anyag**: albedo, normal, roughness/metallic és AO térkép. Textúraforrás: Poly Haven vagy ambientCG (CC0).
- Az alapanyagok legyenek kidolgozva:
  - csempe és linóleum padló;
  - festett és csempézett fal;
  - kazettás álmennyezet;
  - fém polc;
  - hűtő üveggel és párával;
  - fa pékáru-pult;
  - gumis futószalag.
- **Termékek:** mind a 40 termék saját 3D modell, saját, kitalált márkás címke-textúrával. Kenyér, tejesdoboz, palack, konzerv, zacskó, gyümölcs és zöldség valósághű formával.
  A polcon a készlet valóban látszódjon: tele polc → sok termék, fogyáskor hézagok. Ezt MultiMeshInstance3D-vel kell megoldani.
- **Berendezés:** polcok árcímkékkel, hűtők és fagyasztók világító belsővel, pénztárpult kijelzővel, kosaras és kocsis oszlop, bevásárlókocsi, akciós rácskosár, plakátok, padlómatrica, ajtó, ablak, lámpatest.
- **Emberek:**
  - riggelt, animált, valósághű arányú 3D karakterek (például Quaternius „Ultimate Modular Men/Women” CC0 vagy saját);
  - legalább 8 különböző kinézet: kor, nem, ruha, hajszín;
  - animációk: séta, megállás, nézelődés, levétel a polcról, kosár vagy kocsi tolása, sorban állás, fizetés, távozás;
  - a dolgozóknak is legyenek animációi: feltöltés, pénztár, felmosás;
  - animációs keverés AnimationTree-vel, a lábak ne csússzanak.
- **Részletességi szintek (LOD):** a modelleken legyen LOD, és a távoli objektumok cullingja is működjön.

### 3.3 Kamera és effektek
- Simított, inerciás kamera: forgatás, nagyítás és pásztázás érintéssel és egérrel.
  Közelről a polcok és termékek részletei is látszanak, távolról az egész bolt.
- Fókuszmélység (DOF) közelítéskor, enyhe vignetta a „biztonsági kamera” nézetben.
- Részecske-effektek: pénz a kasszánál, csillogás új berendezésnél, por építéskor, pára a fagyasztónál.
- Napszakváltás: a beeső fény színe és iránya változik, este kigyulladnak az utcai lámpák.

### 3.4 Teljesítmény
- Samsung Galaxy A51 (Mali-G72) mobilon **legalább 30 FPS** közepes, és **60 FPS** alacsony grafikai beállítással, 30 vevővel.
- Grafikai beállítás-menü (Alacsony, Közepes, Magas): árnyékminőség, renderfelbontás-skála (FSR), részecskék, LOD-távolság.
- Az APK mérete legfeljebb 300 MB. Ehhez ETC2/ASTC textúratömörítés kell.

### 3.5 Elfogadás
Minden mérföldkőnél kerüljenek a `docs/kepek/` mappába **képernyőképek** (legalább 6): az egész bolt nappal és este, közelről egy polc, a kassza sorral, egy vevő közelről, valamint egy kép mobilon fekvő tájolásban.
A grafikai munka akkor kész, ha ezeken nincs szürke, textúra nélküli felület, minden tárgynak van árnyéka, és a képek egy kiadott, kereskedelmi játék minőségét mutatják.

### 3.6 Hang
- Bolti hangkulisszák: zsongás, csipogó vonalkódolvasó, kocsikerék, hűtőzúgás, ajtócsengő.
- Háttérzene több számmal, mind CC0.
- Külön hangerő a zenére és az effektekre.

### 3.7 Licenc
Csak **CC0** vagy saját készítésű asset használható. Minden asset forrása és licence szerepeljen a `CREDITS.md`-ben.

## 4. Kód és minőség
- Tiszta mappaszerkezet: `scenes/`, `scripts/`, `assets/`, `data/` (termékek és berendezések JSON/Resource fájlokban, nem a kódba égetve), `tests/`.
- Az autoloadok külön felelősséggel: `GameState`, `Economy`, `SaveSystem`, `TimeSystem`.
- **Automata tesztek GUT-tal:** gazdasági számítások, mentés és betöltés, útkeresés, elhelyezési szabályok.
- **GitHub Actions:** minden pushra fusson le a tesztek, és készüljön Linux build, valamint egy debug Android APK artefakt.
- Commitonként egy logikai változás, magyar commitüzenettel. Kis lépések, minden lépés után működő állapot.

## 4/A. Szerverkapcsolat (online funkciók)
A szervert a megrendelő készíti el **https://bigbossfm.hu/VALAMI** alatt. A végleges útvonal később derül ki.
A kliensoldalt viszont már most úgy kell megírni, hogy a szerver elkészültekor csak a címet kelljen beírni.

- **A szerver címe** egyetlen helyen legyen, a `data/szerver.json`-ban (`"base_url": "https://bigbossfm.hu/VALAMI/api/v1"`), és ne a kódba égetve.
  Debug buildben a játék beállításaiban át is lehessen írni.
- **Offline-first működés:**
  - a játék szerver nélkül is teljes értékű, a mentés helyben van;
  - az online rész csak kiegészítés;
  - ha nincs kapcsolat, nem lehet hibaüzenet-áradat és várakozás, csak egy kis „offline” jelzés.
- **Kapcsolat:**
  - csak HTTPS, JSON REST, Godot `HTTPRequest`;
  - időkorlát 10 s, újrapróbálás exponenciális várakozással;
  - a küldendő adatok sorba kerülnek, és az újra-online állapotban mennek el.
- **Azonosítás:**
  - első indításkor a kliens véletlen játékos-azonosítót (UUID) és helyi titkot generál (`POST /jatekos`);
  - a szerver tokent ad vissza, ami a felhasználói adatkönyvtárban tárolódik;
  - a kliensben **nem lehet beépített API-kulcs vagy jelszó**, mert az APK visszafejthető.
- **Funkciók:** a kliensoldal az M4 mérföldkőben készüljön el, kikapcsolható módon.
  1. **Felhőmentés:** mentés fel- és letöltése (`PUT/GET /mentes`), ütközéskor választás a helyi és a felhős mentés között.
  2. **Ranglista:** napi bevétel, boltméret és szint szerint (`GET /ranglista?tipus=...`, `POST /eredmeny`).
  3. **Barátok:** barátkód, a barát boltjának megtekintése csak olvashatóan, a mentés-pillanatképe alapján.
     Napi ajándék a barátoknak, segítségkérés (például gyorsabb szállítás).
  4. **Közös események:** szerverről letöltött hetiesemény-leírás (`GET /esemenyek`): kiemelt termék, szezonális akció, közös cél.
  5. **Hírek és üzenetek:** a játékon belül megjelenő hírek (`GET /hirek`).
- **API-leírás:** a fejlesztő írja meg a `docs/api.md`-ben (OpenAPI-szerű) a kliens által elvárt összes végpontot, kérés- és válaszformátummal, hibakódokkal.
  Készítsen hozzá egy **helyi teszt-szervert** is (`tools/teszt_szerver.py`, csak Python stdlib), hogy a kliens szerver nélkül is tesztelhető legyen.
- **Csalás elleni minimum:**
  - a beküldött eredmény mellé kerüljön a mentés-statisztika;
  - a kliens ne küldjön lehetetlen értéket, mert a végső ellenőrzés a szerveré.
- **Adatvédelem:** csak a játékhoz szükséges adat (azonosító, boltnév, mentés, eredmény) mehet fel. Nincs reklám-azonosító, nincs helyadat.
- **Kapcsolat a „Biztonság” ponttal:** a szerver jelszava, tokenje vagy adatbázis-elérése **soha** nem kerülhet ebbe a nyilvános repóba.

## 5. Biztonság és jog
- A repó **nyilvános**. Aláírókulcs, jelszó, token és API-kulcs **soha** nem kerülhet bele.
  A kiadási aláírás csak GitHub Secretből történhet.
- **Tilos** a Tesco vagy bármely más valós cég neve, logója, színvilága és szlogenje. Saját, kitalált márkát használj.
- Nincs reklám, nincs fizetős tartalom, nincs adatgyűjtés.

## 6. Mérföldkövek
Mindegyik végén működő APK készüljön, és a `CHANGELOG.md` frissüljön.
1. **M1 – Alap:** projekt, CI, a PBR-anyagokkal és sütött fényekkel kész bolttér (3.1–3.2), kamera, rácsos építés, mentés.
2. **M2 – Élet:** vevők útkereséssel, polcok és készlet, kassza, sorban állás, bevétel.
3. **M3 – Gazdaság:** nagyker, árazás, kiadások, napi zárás, személyzet.
4. **M4 – Fejlődés és online:** a 4/A pont kliensoldala a teszt-szerverrel, szintek, feloldások, bővítés, feladatok, offline haladás.
5. **M5 – Csiszolás:** a 3.5 pont szerinti grafikai elfogadás, hangok, effektek, oktató rész az elején, beállítások, teljesítmény-optimalizálás.
6. **M6 – Kiadás:** ünnepek, eredmények, egyensúlyozás, hibajavítás, aláírt kiadási APK.

## 7. Hogyan dolgozz
- A munkát a `main` ágon, vagy mérföldkövenként PR-ban add le.
- Ha egy döntés nem egyértelmű, a jobb játékélményt választd, és írd le a `DONTESEK.md`-be.
- Ne kérdezz vissza apróságokon. Haladj, és a végén foglald össze, mi készült el.

## 8. Mikor kész
Akkor kész a játék, ha mind a hat mérföldkő teljesült, és teljesülnek ezek is:
- a CI zöld;
- az APK telepíthető Android 8 és újabb rendszeren;
- egy új játék oktatással indul, és legalább 10 órányi fejlődés van benne;
- a mentés két verzió között is működik;
- nincs ismert összeomlás.
