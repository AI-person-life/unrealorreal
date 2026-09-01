# A módszer

🇬🇧 [In English](../en/method.md)

Videókártya nélküli gépen, egy ember munkájával.

## 1. Kánon először, kép utána

Mielőtt egyetlen kép elkészülne, le kell írni, **ki ez a karakter**: külső jegyek,
amik soha nem változnak, és amik igen. Ez unalmas lépés, és pont ezért hagyják ki.

Ha nincs kánon, a képek egymáshoz képest fognak csúszni, és a végén nem lesz
felismerhető alak — csak hasonló emberek gyűjteménye.

Nálunk a kánon gépi olvasható is: minden karakterhez egy JSON, benne a rögzített
jegyek, a tiltott ábrázolások, és az a mondat, amit magáról mond.

## 2. Tanítás felhőben, generálás helyben

A LoRA-tanítás az egyetlen lépés, ami komoly számítást igényel — ezt kell megvenni.
Innen jön a projekt teljes költsége. Minden más a saját gépen fut.

**A korszakok közül nem az utolsó a legjobb.** Több korszakot le kell tölteni és
összehasonlítani, ugyanazzal a próbakéréssel. A választás szemre megy.

## 3. Sok kép, kevés túlélő

488 képből 65 lett használható. Ez nem pazarlás, hanem a munka természete: a
generálás költsége nulla, tehát a válogatás lett a szűk keresztmetszet.

Aki az első tíz képből válogat, az a modell hibáit fogja publikálni.

## 4. Utómunka, ahol olcsóbb

Ami képszerkesztéssel egy paranccsal javítható, azt ne a modellel akard megoldatni.
A tükrözés, a vágás, a színhelyreállítás triviális művelet — a modell rábeszélése
ugyanerre viszont órákba telik, és nem determinisztikus.
