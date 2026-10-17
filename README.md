# Saját Boltom 3D (munkanév)

**Többjátékos** 3D bolt-menedzser és gazdasági mobiljáték a 2015-ös „Én kicsi Tescóm” (My Little Tesco) mintájára, de annál jobb.

---

## ⚠️ A FEJLESZTŐNEK (Grok) – ELSŐ ÁTADÁS = HIBÁTLAN, KIADHATÓ VERZIÓ

**Ez a megrendelés nem prototípusra, nem demóra, nem „első körre” szól.** A megrendelő egyetlen átadást fogad el: a teljes, hibátlan, azonnal kiadható játékot (`v1.0.0`).
Javítási kör, „majd a következő verzióban” vagy „ez csak alap” nem fogadható el. Ami nincs kész és nincs letesztelve, az nincs kész.

### Mielőtt egy sort is írsz
1. Olvasd el a **[FELADAT.md](FELADAT.md)** fájlt az elejétől a végéig. Ez a teljes, kötelező megrendelés, minden pontja követelmény.
2. Írj egy **`TERV.md`** fájlt:
   - architektúra, jelenetek, autoloadok, adatformátumok;
   - a szerver API-ja;
   - az asset-lista forrással és licenccel;
   - a tesztelési terv;
   - a FELADAT.md minden pontjához egy sor arról, hol és hogyan valósul meg (megfelelési táblázat).
3. Csak ezután kezdd a kódolást.

### Kötelező minőségkapuk – minden commit előtt, kivétel nélkül
- [ ] A projekt hiba és figyelmeztetés nélkül nyílik meg és fordul:
  - `godot --headless --check-only` / import, 0 error, 0 warning;
  - GDScript statikus típusokkal, a figyelmeztetések hibaként kezelve.
- [ ] Minden automata teszt zöld (GUT), a szerveré is (`pytest`).
- [ ] A játék headless módban elindul, és egy automatizált **füstteszt** végigmegy az alapcikluson:
  új játék → építés → vevők vásárolnak → napi zárás → mentés → betöltés → piaci kötés a teszt-szerverrel.
  Ennek a tesztnek a kódja is a repóban van (`tests/fustteszt/`).
- [ ] A CI (GitHub Actions) zöld:
  - tesztek;
  - Linux build;
  - aláírt Android APK.
- [ ] Nincs a kódban `TODO`, `FIXME`, `placeholder`, ideiglenes asset, kikommentezett kód, debug-kiírás vagy beégetett tesztadat.
- [ ] Nincs angol szöveg a felületen, minden szöveg a fordítási fájlból jön.
- [ ] Nincs titok a repóban (kulcs, jelszó, token).

### Kötelező ellenőrzés az átadás előtt
- **Funkcionális:** a FELADAT.md minden pontját egyenként kipipálva, bizonyítékkal, a `docs/ATADAS.md`-ben. A bizonyíték egy teszt neve, egy képernyőkép vagy egy videó.
- **Grafika:** a FELADAT.md 3.5 pontja szerinti képernyőképek a `docs/kepek/` mappában. Nincs szürke, textúra nélküli felület, nincs hiányzó árnyék, nincs villódzás, nincs Z-fighting.
- **Teljesítmény:** mért FPS-napló Android eszközön (cél: Galaxy A51 / Mali-G72 vagy azzal egyenértékű), 30 vevővel, a `docs/teljesitmeny.md`-ben.
- **Tartósság:**
  - 2 órás automatizált futtatás összeomlás és memóriaszivárgás nélkül;
  - mentés, betöltés, verzióváltás és megszakított mentés (áramszünet-szimuláció) után is ép adat.
- **Multiplayer:**
  - 200 szimulált kliens terheléses tesztje;
  - versenyhelyzetek a piacon és a letétnél;
  - kapcsolatvesztés és újracsatlakozás;
  - semmi nem könyvelődhet kétszer, és nem veszhet el pénz vagy áru.
- **Egyensúly:** gazdasági szimuláció (100 bot × 30 nap) eredménye grafikonnal. Nincs elszabaduló infláció, nincs végtelen pénz-kiskapu, a fejlődés legalább 10 óra.
- **Kezelhetőség:**
  - kis és nagy kijelzőn, álló és fekvő módban minden olvasható és elérhető;
  - nincs levágott szöveg;
  - érintésre minden gomb legalább 48 dp.
- **Telepítés:** az aláírt APK tiszta Android 8, 12 és 14+ rendszeren települ, és elsőre indul, az oktató résszel.

### Átadás
- Egyetlen GitHub Release: `v1.0.0`.
  - Benne: aláírt APK, Linux build, `CHANGELOG.md`, `docs/ATADAS.md` (a teljes ellenőrzőlista bizonyítékokkal).
  - Mellé: a `server/` referencia-szerver indítási leírása.
- Az átadás csak akkor érvényes, ha a fenti lista **minden** pontja teljesül. Ha egy is hiányzik, a munka nincs kész. Ilyenkor ne adj át, hanem folytasd.
- Belső munka közben a mérföldkövek (FELADAT.md 6. pont) a haladás belső rendjét adják, nem külön átadások.
  Minden mérföldkő végén ugyanazok a minőségkapuk érvényesek, vagyis a `main` ág soha nem lehet törött.

### Munkamód
- Ne kérdezz vissza apróságokon. Döntsd el a jobb játékélmény szerint, és írd le a `DONTESEK.md`-be.
- Ne egyszerűsíts a követelményeken, és ne hagyj ki pontot azzal, hogy „később jön”. Ha valami valóban lehetetlen, azt az indoklással és a választott, egyenértékű megoldással együtt írd a `DONTESEK.md`-be.
- Kis, logikus commitok, magyar commitüzenetekkel.

---

## Cél
- Saját bolt felépítése és berendezése 3D-ben: polcok rácsra pakolása, áthúzása, dekoráció.
- Árurendelés, polcfeltöltés, árazás.
- Sétáló vevők útkereséssel, kosár, sorban állás, kasszák.
- Forgatható, nagyítható kamera.
- Multiplayer: közös városok, piac, játékosok közötti üzletelés, szerződések, láncok (FELADAT.md 4/B).
- A saját bolt offline is fut. Az online rész (felhőmentés, ranglista, barátok) a bigbossfm.hu szerverrel működik, lásd FELADAT.md 4/A.
- Magyar felület.

## Technika
- Motor: Godot 4.4+ (GDScript, statikus típusokkal).
- Platform: Android (aláírt APK, Gradle export), Linux.
- Grafika: valósághű, PBR-anyagos 3D, sütött fényekkel (lásd FELADAT.md 3. pont).
- Szerver: Python 3.12 referencia-szerver a `server/` mappában (lásd FELADAT.md 4/B.5).
- Csak CC0 vagy saját asset (Poly Haven, ambientCG, Quaternius), a forrásuk a `CREDITS.md`-ben.

## Szabályok
- Tesco- vagy más védjegyes név, logó és arculat nem használható.
- Aláírókulcs, jelszó és token nem kerülhet a repóba (lásd .gitignore). A kiadási aláírás csak GitHub Secretből történhet.
