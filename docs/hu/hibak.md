# Amit elrontottunk

🇬🇧 [In English](../en/mistakes.md)

Ez a leghasznosabb oldal az egész tárolóban. Mind az öt hiba **némán** bukik el:
nem hibaüzenetet kapsz, hanem rossz eredményt, amiről nem tudod, hogy rossz.

---

## 1. A képgeneráló nem érti a tagadást

Ha azt kéred, hogy valami *ne* legyen a képen, azzal pontosan azt hívod elő. A modell
a szavakat súlyozza, nem a logikai műveletet értelmezi.

```
❌  "no tattoo on the right arm"                          → 0 találat 4-ből
✅  "turned so her tattooed LEFT arm is nearest the camera" → 4 találat 4-ből
```

| megtiltottuk | megmondtuk, mit akarunk |
|---|---|
| ![A tetoválás a rossz karon](../kepek/tagadas-elotte.jpg) | ![A tetoválás a kánon szerinti karon](../kepek/tagadas-utana.jpg) |

*Ugyanaz a kérés, két megfogalmazásban. A karakternek a kánon szerint a bal karján
van a tetoválás — a tiltó változat pont az ellenkezőjét hozta.*

**A minta:** ne tiltsd, hanem rendezd el a kompozíciót úgy, hogy a kérdés fel se
merüljön. Az oldal utólag amúgy is javítható tükrözéssel (`convert -flop`) — az
veszteségmentes művelet.

## 2. A letöltési sorrend nem a tanítási korszakok sorrendje

Több korszakból választva a fájlnevek sorrendje félrevezet. Aki erre hagyatkozik,
rossz modellt tesztel, és nem tud róla.

**A megbízható forrás a fájl saját fejléce:**

```python
# a safetensors fejléce JSON, az elején a hossz little-endian uint64
import json, struct
with open(ut, "rb") as f:
    n = struct.unpack("<Q", f.read(8))[0]
    fej = json.loads(f.read(n))
print(fej["__metadata__"].get("ss_epoch"))
```

Ez a lépés hat felesleges kézi letöltéstől mentett meg.

## 3. A némán elbukó kattintás

Egy webes tanítófelületen a modellválasztó jelölőnégyzet **0×0 képpont méretű,
rejtett mező** volt. Sem koordinátás kattintás, sem elemhivatkozás nem fogta meg.
A felület nem jelzett hibát — csak épp semmi nem lett kijelölve.

**Ami működött:** a `label` elemre kattintani, nem a rejtett bemenetre.

**Az általános tanulság:** automatizálásnál mindig ellenőrizd az *eredményt*, ne a
kattintás visszajelzését. A csendes hiba a legdrágább: nem áll meg, hanem továbbmegy
rossz adattal.

## 4. Az oldal-felismerő szkript nem megbízható

A „melyik karján van" kérdést gépi úton próbáltuk eldönteni. Nem ment. Ami ment:
szemre nézni, kar-sáv kivágással, 380–400 képpont szélesen, 12 kép ívenként. A
230 képpontos kontaktív kevés volt hozzá.

Néha az emberi szem az olcsóbb eszköz.

## 5. Felhős nagy nyelvi modell magyar kreatív szövegre használhatatlan

Több felhős modellt kipróbáltunk magyar márkahang írására. Törik a nyelvet, és más
nyelv szivárog a szövegbe. Rövid, tényszerű feladatra (naplóelemzés, összefoglalás)
kiválóak és olcsók — kreatív magyar szövegre nem.

**Járulékos csapda:** a „gondolkodó" modellek alacsony jogkerettel **üres választ**
adnak, mert a belső gondolatmenet felemészti a keretet. A hiba nem hibaüzenet
formájában jelenik meg, hanem üres mezőként.

```
/api/generate        → "think": false
/v1/chat/completions → "reasoning_effort": "none"
```
