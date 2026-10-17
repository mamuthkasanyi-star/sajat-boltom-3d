# FELADAT – a fejlesztőnek (Grok)

**Ez nem prototípus-megrendelés.** Kiadható, teljes értékű mobiljátékot kérünk. Ne álljon meg a munka egy „alap demó”-nál.
Minden mérföldkő végén a játék legyen futtatható, és a következő mérföldkő erre épüljön tovább. A cél a 8. pont szerinti kész játék.

## 1. A játék
3D bolt-menedzser játék a 2015-ös „Én kicsi Tescóm” mintájára, de annál jobb.
A játékos egy üres üzlethelyiségből indul, és nagy, sikeres áruházat épít belőle.
- Motor: **Godot 4.3+**, GDScript.
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

## 3. Grafika és hangulat
- Stilizált, low-poly 3D. Csak **CC0** vagy saját modell (például Kenney csomagok). A forrást és a licencet a `CREDITS.md` sorolja fel.
- Mobilon a cél **30 FPS, középkategóriás telefonon 60 FPS** (például Samsung Galaxy A51, Mali-G72). Ehhez LOD, MultiMesh és a mobil renderelő kell.
- Kamera: forgatás, nagyítás és pásztázás érintéssel, valamint egy „biztonsági kamera” nézet.
- Hangok és zene (CC0), külön hangerő-szabályzóval.

## 4. Kód és minőség
- Tiszta mappaszerkezet: `scenes/`, `scripts/`, `assets/`, `data/` (termékek és berendezések JSON/Resource fájlokban, nem a kódba égetve), `tests/`.
- Az autoloadok külön felelősséggel: `GameState`, `Economy`, `SaveSystem`, `TimeSystem`.
- **Automata tesztek GUT-tal:** gazdasági számítások, mentés és betöltés, útkeresés, elhelyezési szabályok.
- **GitHub Actions:** minden pushra fusson le a tesztek, és készüljön Linux build, valamint egy debug Android APK artefakt.
- Commitonként egy logikai változás, magyar commitüzenettel. Kis lépések, minden lépés után működő állapot.

## 5. Biztonság és jog
- A repó **nyilvános**. Aláírókulcs, jelszó, token és API-kulcs **soha** nem kerülhet bele.
  A kiadási aláírás csak GitHub Secretből történhet.
- **Tilos** a Tesco vagy bármely más valós cég neve, logója, színvilága és szlogenje. Saját, kitalált márkát használj.
- Nincs reklám, nincs fizetős tartalom, nincs adatgyűjtés.

## 6. Mérföldkövek
Mindegyik végén működő APK készüljön, és a `CHANGELOG.md` frissüljön.
1. **M1 – Alap:** projekt, CI, bolt, kamera, rácsos építés, mentés.
2. **M2 – Élet:** vevők útkereséssel, polcok és készlet, kassza, sorban állás, bevétel.
3. **M3 – Gazdaság:** nagyker, árazás, kiadások, napi zárás, személyzet.
4. **M4 – Fejlődés:** szintek, feloldások, bővítés, feladatok, offline haladás.
5. **M5 – Csiszolás:** grafika, hangok, effektek, oktató rész az elején, beállítások, teljesítmény-optimalizálás.
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
