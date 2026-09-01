# Eszközök

🇬🇧 [In English](../en/tooling.md)

Ezek a projekt közben születtek, mert nem volt rájuk kész megoldás. Mind magyar
nyelvű kimenettel dolgozik, és mind helyben fut.

## Wi-Fi felmérés összesítése

Kismet-naplóból vagy WiGLE-exportból aggregált statisztikát ad. **Szándékosan
képtelen egyedi hálózatot azonosítani:** MAC-címet, hálózatnevet és koordinátát
soha nem ír ki, csak darabszámot és arányt. Így a kimenete közvetlenül publikálható.

Egy 3 416 hálózatos felmérés eredménye ezzel készült.

## WiGLE-kliens

A WiGLE napi lekérdezési kerete csúszó skálán mozog, és friss fiókon nagyon szűk.
A kliens ezért:

- minden választ lemezre gyorsítótáraz (ugyanazt kétszer nem kéri le),
- lapozáskor beállítható lapszámnál megáll,
- a kimenetben csak összesítést ad.

## Helyi magyar diktálás

A használt fejlesztői eszköz beépített diktálása nem ismeri a magyart, és
kliensoldalról nem bővíthető. A megkerülés helyi `faster-whisper large-v3`
modellel történik: magyarul hibátlan, semmit nem küld ki a gépről, cserébe
videókártya nélkül nagyjából négyszeres valós idő.

## Beszédszintézis magyarul, offline

`piper` helyi hangokkal. Egy reggeli bemondás (pontos idő + időjárás) és a
weboldal köszönése is ezzel szólal meg — ugyanaz a hang, ugyanazok a
hangolási értékek.

---

## Két hardveres buktató, ami órákat vitt el

**A mikrofon-előerősítő alapból nulla decibelen állhat.** Egy ALC269VB kódeken a
`Front Mic Boost` gyárilag 0 volt, emiatt a felvételek csúcsértéke a skála két
százaléka lett — gyakorlatilag néma. Egy parancs javítja:

```bash
amixer -c 0 sset 'Front Mic Boost' 2   # ~24 dB
```

**Az `arecord -D plughw:...` elveszi a mikrofont a hangszervertől.** A PipeWire
elejti a rögzítő csomópontot, eltűnik a tálcaikon, és magától nem jön vissza.

```bash
# rossz: kizárólagosan foglalja az eszközt
arecord -D plughw:0,0 ...
# jó: a hangszerveren keresztül megy
arecord -D default ...
# ha már megtörtént:
systemctl --user restart wireplumber
```
