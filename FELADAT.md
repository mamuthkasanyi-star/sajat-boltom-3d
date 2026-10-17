# FELADAT – a fejlesztőnek (Grok)

**Ez nem prototípus-megrendelés.** Egyetlen átadást fogadunk el: a teljes, hibátlan, azonnal kiadható játékot (`v1.0.0`).
A kötelező minőségkapukat és az átadási ellenőrzést a [README.md](README.md) „A FEJLESZTŐNEK” része írja le, ezek ennek a megrendelésnek a részei.
A mérföldkövek (6. pont) csak a belső munka sorrendjét adják, nem külön átadások. A `main` ág viszont soha nem lehet törött.

## 1. A játék
**Többjátékos (multiplayer)** 3D bolt-menedzser és gazdasági játék a 2015-ös „Én kicsi Tescóm” mintájára, de annál jobb.
A játékos egy üres üzlethelyiségből indul, és nagy, sikeres áruházat vagy áruházláncot épít belőle, miközben a többi játékossal kereskedik a közös piacon (lásd 4/B).
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
A játék szervere **a megrendelő saját Samsung Galaxy A51 telefonján fut**, és a **https://bigbossfm.hu/VALAMI** címen érhető el egy Cloudflare-alagúton (cloudflared tunnel) keresztül. A végleges útvonal később derül ki.
A szerver üzemeltetését (telepítés, alagút, domain) a megrendelő végzi. A fejlesztő dolga a szerver kódja, a telepítési leírás és a systemd unit-sablon. Titok vagy alagút-azonosító nem kerülhet a repóba.
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

## 4/B. MULTIPLAYER – játékosok közötti kereskedelem és piac (kötelező, a játék magja)
A játék **többjátékos gazdasági játék**. A játékosok boltjai egy közös világban, közös városokban működnek, és egymással kereskednek, versenyeznek, együttműködnek.
Az egyjátékos rész (4/A, offline-first) megmarad: a saját bolt offline is fut. A piac, a kereskedés és a közös világ viszont csak online működik.

### 4/B.1 Közös világ
- **Városok:** a világ városokból (szerverszobákból) áll, városonként legfeljebb 50 játékossal.
  - A térképen látszanak a többiek boltjai, és bármelyikbe be lehet lépni, hogy élőben lásd, mi zajlik bent: vevők, polcok, árak (csak megtekintés).
  - A városnak közös vásárlóköre van, a vevők a boltok között **választanak** az ár, a választék, a hírnév és a távolság alapján.
- **Szerepek és specializáció:** a játékos a kiskereskedelem mellett más ágba is beléphet, és ezzel ellátási láncok jönnek létre:
  - **termelő:** farm, pékség, tejüzem, üdítőgyár;
  - **nagykereskedő:** raktár, teherautók;
  - **kiskereskedő:** bolt, áruház.
  - A termelők gyártási sorral, nyersanyaggal és gyártási idővel dolgoznak.
  - A boltok tőlük, vagy a drágább, NPC-s „állami nagykerből” szerezhetik be az árut.
- **Logisztika:**
  - a játékosok közötti szállítás időbe és pénzbe kerül, és a távolságtól függ;
  - a teherautókat fejleszteni lehet;
  - a szállítás útja látszik a térképen.

### 4/B.2 Piac (tőzsde-jellegű árupiac)
- **Termékenkénti ajánlati könyv (order book):** vételi és eladási limitajánlatok, azonnali piaci vétel és eladás.
  - Az ajánlatok párosítását a szerver végzi.
  - Látszik a legjobb vételi és eladási ár, a mélység és az utolsó kötések.
- **Ártörténet:** grafikonok órás, napi és heti bontásban (gyertyadiagram és forgalom), városi és országos átlagárral.
- **Díjak:** piaci díj és szállítási költség, hogy a spekuláció ne legyen ingyenes.
- **Romlandóság:** a friss áru a piacon is romlik, ezért lejárat közeli árut csak kedvezménnyel lehet eladni.
- **Árverés:** ritka berendezések, prémium üzlethelyiségek és felszámolt boltok készlete licitálható, időkorláttal.

### 4/B.3 Közvetlen üzletelés
- **Ajánlat két játékos között:** áru, pénz és berendezés cseréje egy ajánlat–ellenajánlat–elfogadás folyamatban.
  - Letét (escrow): mindkét fél tétje zárolódik, és csak a teljes elfogadáskor cserél gazdát.
  - Csalni ezért nem lehet.
- **Szállítási szerződés:** például „heti 500 kenyér, 180 Ft/db, 4 hétig”.
  - Automatikus teljesítés és fizetés.
  - Kötbér nem teljesítéskor.
  - A szerződések listája és állapota mindig látható.
- **Kölcsön és befektetés:** játékosok közötti kölcsön kamattal, valamint tőkebevonás egy másik játékos vállalkozásába, részesedésért.

### 4/B.4 Közösség
- **Láncok (céhek):** több játékos közös márkanévvel üzemel, közös kassza, közös raktár, belső kedvezményes árak és rangok.
- **Chat:** városi, lánc- és privát chat, alap trágárságszűréssel, jelentés és némítás.
- **Verseny:**
  - heti ranglisták városonként és országosan;
  - szezonok (például 4 hetes ciklus) jutalommal;
  - városi közös célok (például „A város karácsonyi forgalma 1 millió Ft”).
- **Hírnév:** a vevői elégedettség, a teljesített szerződések és az értékelések együtt adják. Ettől függ, kik akarnak üzletelni a játékossal.

### 4/B.5 Technika (szerver-hiteles gazdaság)
- **A szerver a hiteles forrás** minden közös adatban: pénz, készlet, piac, szerződés, kötés.
  - A kliens csak kéréseket küld, és megjeleníti az eredményt.
  - A kliens soha nem dönthet pénzről vagy készletről a közös rendszerben.
- **Saját bolt és piac kapcsolata:**
  - a saját bolt helyi szimulációja offline is fut;
  - online szinkronkor a szerver ellenőrzi és elfogadja az eredményt (értékhatárok és időarányos plauzibilitás alapján);
  - piaci műveletet csak online, szinkron állapotban lehet végezni.
- **Valós idejű csatorna:** WebSocket (`wss://bigbossfm.hu/VALAMI/ws`) a piaci árfolyamhoz, a chathez, az ajánlatokhoz és a városi eseményekhez.
  - Kapcsolatvesztéskor automatikus újracsatlakozás.
  - Ami a kapcsolat szünetelése alatt történt, az utólag pótlódik.
- **Tranzakciók:**
  - minden pénz- és készletmozgás atomi és naplózott (főkönyv);
  - ismételt kérés ne könyvelődhessen kétszer (idempotencia-kulcs);
  - piaci kötés és letét adatbázis-tranzakcióban.
- **Visszaélés ellen:**
  - sebességkorlát (rate limit) végpontonként;
  - gyanús kereskedési minták jelzése (például két fiók közti egyirányú, piaci ár alatti pénzmozgatás);
  - admin-napló.
- **Referencia-szerver – ezt is a fejlesztő írja meg, a `server/` mappába:**
  - Python 3.12, SQLite, `aiohttp` (vagy `websockets`), `requirements.txt`, migrációk;
  - a 4/A és 4/B összes végpontja és WebSocket-üzenete;
  - indítható legyen egy paranccsal (`python -m server`), mert ezt fogja a megrendelő üzemeltetni;
  - **célhardver, ehhez kell méretezni:**
    - Samsung Galaxy A51: Exynos 9611, 8 mag (4× A73 2,3 GHz + 4× A53 1,7 GHz), kb. 4 GB RAM, eMMC-tár;
    - Ubuntu 24.04 aarch64 (Mamuthka linux), Python 3.12, systemd;
    - a telefonon más szolgáltatások is futnak, ezért a játékszerver erőforrás-kerete: **≤ 300 MB RAM, átlagosan ≤ 25% CPU 200 kliensnél**;
  - csak aarch64-en is gond nélkül telepíthető, tiszta Python vagy előre fordított aarch64-es wheel-lel rendelkező függőség használható;
  - SQLite WAL módban, kevés lemezírással (az eMMC kímélése: kötegelt írás, nincs másodpercenkénti teljes mentés);
  - a szerver a localhost egy portján hallgat (alapból 127.0.0.1:8795, környezeti változóval állítható), a kívülről jövő forgalom a Cloudflare-alagúton át érkezik. A valódi kliens-IP-t a `CF-Connecting-IP` fejlécből olvassa, a rate limit is erre épül;
  - REST és WebSocket ugyanazon a porton, `/VALAMI` előtaggal, ami konfigurálható (`BASE_PATH`);
  - mellékelve: `deploy/mamuthka-jatekszerver.service` (systemd unit-sablon, User=mamuthka, Restart=on-failure, MemoryMax=400M), `deploy/TELEPITES.md` (lépésről lépésre a telefonra), és napi automatikus adatbázis-mentés rotációval;
  - a frissítés adatvesztés nélkül menjen: verziózott adatbázis-migrációk, indulás előtti automatikus mentés;
  - legyenek hozzá automata tesztek: piaci párosítás, letét, szerződés-teljesítés, idempotencia, versenyhelyzetek;
  - legyen hozzá **terheléses teszt** 200 szimulált klienssel. Az eredményt a célhardveren vagy azzal egyenértékű aarch64 / 4 GB-os környezetben kell mérni (például QEMU vagy egy ARM-es CI-futtató), és a `docs/teljesitmeny.md`-be kell írni;
  - a konfiguráció (adatbázis útvonala, titkos kulcs) környezeti változóból jöjjön, a repóba csak `config.example` kerülhet.
- **Dokumentáció:** a `docs/api.md` a REST végpontokat és a WebSocket-üzeneteket is írja le.

## 5. Biztonság és jog
- A repó **nyilvános**. Aláírókulcs, jelszó, token és API-kulcs **soha** nem kerülhet bele.
  A kiadási aláírás csak GitHub Secretből történhet.
- **Tilos** a Tesco vagy bármely más valós cég neve, logója, színvilága és szlogenje. Saját, kitalált márkát használj.
- Nincs reklám, nincs fizetős tartalom, nincs adatgyűjtés.

## 6. Mérföldkövek
Ez a belső munka sorrendje. Minden mérföldkő végén teljesülnie kell a README minőségkapuinak, és a `CHANGELOG.md`-nek frissülnie kell. Átadás csak a teljes v1.0.0.
1. **M1 – Alap:** projekt, CI, a PBR-anyagokkal és sütött fényekkel kész bolttér (3.1–3.2), kamera, rácsos építés, mentés.
2. **M2 – Élet:** vevők útkereséssel, polcok és készlet, kassza, sorban állás, bevétel.
3. **M3 – Gazdaság:** nagyker, árazás, kiadások, napi zárás, személyzet.
4. **M4 – Fejlődés és online alap:** szintek, feloldások, bővítés, feladatok, offline haladás.
   A 4/A pont teljes egészében (kliens és referencia-szerver), fiókkezelés, felhőmentés, ranglista.
5. **M5 – Multiplayer gazdaság:** a 4/B pont teljes egészében:
   - közös városok, a többiek boltjainak megtekintése, közös vásárlókör;
   - termelő és nagykereskedő szerep, logisztika;
   - piac ajánlati könyvvel és ártörténettel, árverés;
   - közvetlen üzlet letéttel, szerződések, kölcsön;
   - láncok, chat, szezonok;
   - a szerver tesztjei és a 200 klienses terheléses teszt zöld.
6. **M6 – Csiszolás:** a 3.5 pont szerinti grafikai elfogadás, hangok, effektek, oktató rész az elején (a piac és az üzletelés tanítása is), beállítások, teljesítmény-optimalizálás.
7. **M7 – Kiadás:** ünnepek, eredmények, a gazdaság egyensúlyozása (szimulációval: például 100 bot-játékos 30 napon át, infláció és monopólium figyelése), hibajavítás, aláírt kiadási APK.

## 7. Hogyan dolgozz
- A munkát a `main` ágon, vagy mérföldkövenként PR-ban add le.
- Ha egy döntés nem egyértelmű, a jobb játékélményt választd, és írd le a `DONTESEK.md`-be.
- Ne kérdezz vissza apróságokon. Haladj, és a végén foglald össze, mi készült el.

## 8. Mikor kész
Akkor kész a játék, ha mind a hét mérföldkő teljesült, a README „Kötelező ellenőrzés az átadás előtt” listájának minden pontja bizonyítékkal a `docs/ATADAS.md`-ben van, és teljesülnek ezek is:
- a CI zöld;
- az APK telepíthető Android 8 és újabb rendszeren;
- egy új játék oktatással indul, és legalább 10 órányi fejlődés van benne;
- a mentés két verzió között is működik;
- legalább 2 telefonon és 1 PC-n egyszerre játszva a piac, a közvetlen üzlet és a chat hibátlanul működik;
- a referencia-szerver egy paranccsal indul, és a tesztjei zöldek;
- nincs ismert összeomlás.
