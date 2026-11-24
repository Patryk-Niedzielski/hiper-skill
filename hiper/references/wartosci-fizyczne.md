# Wartości fizyczne — ściąga do /hiper

Wartości przybliżone, w przestrzeni liniowej (albedo w sRGB podane w nawiasie). Traktuj je jako punkt
startowy i zawsze dodawaj zmienność (±10–20%) przez maski i szumy.

## Albedo (base color) i roughness — materiały niemetaliczne

| Materiał | Albedo liniowe (≈ sRGB) | Roughness |
|---|---|---|
| Świeży śnieg | 0,80–0,90 (≈ 230–243) | 0,6–0,9 |
| Biała farba ścienna | 0,70–0,80 (≈ 218–231) | 0,7–0,9 |
| Beton | 0,25–0,40 (≈ 137–170) | 0,75–0,95 |
| Asfalt (stary) | 0,08–0,12 (≈ 79–97) | 0,8–0,95 |
| Asfalt (świeży) | 0,03–0,05 (≈ 49–63) | 0,7–0,9 |
| Cegła czerwona | ok. 0,25 / 0,10 / 0,07 | 0,8–0,95 |
| Drewno surowe | 0,20–0,45 | 0,6–0,9 |
| Drewno lakierowane | jak surowe | 0,2–0,45 |
| Trawa | ok. 0,08 / 0,15 / 0,04 | 0,6–0,85 |
| Gleba / ziemia | 0,08–0,20 | 0,85–1,0 |
| Piasek | 0,30–0,45 | 0,8–0,95 |
| Granit / skała | 0,10–0,35 | 0,5–0,9 |
| Skóra człowieka | 0,25–0,60 (zależnie od karnacji) | 0,35–0,6 + SSS |
| Tkanina bawełniana | dowolne, rzadko > 0,7 | 0,8–1,0 (+ sheen) |
| Plastik | dowolne | 0,3–0,7 (nigdy jednolite) |
| Węgiel drzewny (najciemniejsze) | 0,02–0,04 | 0,8–1,0 |

Zasada: nic naturalnego nie ma albedo poniżej ok. 0,02 ani powyżej ok. 0,9.

## Metale (metallic = 1, kolor to reflektancja F0)

| Metal | F0 (≈ sRGB) | Typowa roughness |
|---|---|---|
| Aluminium | ok. 245 / 246 / 246 | 0,2–0,5 |
| Stal / żelazo | ok. 196 / 199 / 199 | 0,3–0,6 |
| Chrom | ok. 140 / 141 / 141 (lustrzane) | 0,05–0,2 |
| Miedź | ok. 250 / 208 / 192 | 0,25–0,5 |
| Złoto | ok. 255 / 226 / 155 | 0,15–0,4 |
| Rdza (to już NIE metal) | albedo ok. 0,20 / 0,08 / 0,04, metallic 0 | 0,8–1,0 |

## IOR (dla szkła, wody, przezroczystych)

Woda 1,33 · Lód 1,31 · Szkło 1,5 · Plastik 1,45–1,55 · Diament 2,42 · Skóra ok. 1,4

## Światło

| Źródło | Temperatura | Natężenie (orientacyjnie) |
|---|---|---|
| Słońce w południe | 5000–5800 K | ok. 100 000 lx |
| Złota godzina | 3000–4000 K | ok. 1 000–10 000 lx |
| Niebo zachmurzone | 6500–7500 K | ok. 1 000–20 000 lx |
| Cień na zewnątrz (oświetla go niebo) | 7000–10 000 K | ok. 10–20% słońca |
| Żarówka / ciepły LED | 2700 K | 800 lm (≈ 60 W) |
| Świetlówka biurowa | 4000 K | 300–500 lx na biurku |
| Świeca | 1800–1900 K | ok. 12 lm |
| Latarnia sodowa | ok. 2000 K | 10–20 lx na ulicy |

## Typowe wymiary (1 jednostka = 1 m)

| Element | Wymiar |
|---|---|
| Wzrost człowieka | 1,60–1,85 m (oczy ok. 1,6 m) |
| Drzwi wewnętrzne | 0,80–0,90 × 2,00–2,10 m |
| Wysokość kondygnacji | 2,7–3,0 m (mieszkanie), 3,5–4,5 m (stary budynek) |
| Grubość ściany zewnętrznej | 0,25–0,50 m |
| Stopień schodów | wys. 0,15–0,18 m, głęb. 0,28–0,30 m |
| Blat kuchenny | 0,90 m wysokości, 0,60 m głębokości |
| Siedzisko krzesła | 0,45 m |
| Parapet | 0,85–0,90 m od podłogi |
| Klamka | 1,00–1,05 m |
| Krawężnik | 0,12–0,15 m |
| Pas ruchu | 2,75–3,5 m |
| Samochód osobowy | ok. 4,5 × 1,8 × 1,5 m |
| Słup latarni | 6–10 m |
| Pień dorosłego drzewa | średnica 0,3–1,0 m |
| Cegła | 0,25 × 0,12 × 0,065 m |

## Fazki (bevel)

Meble i przedmioty: 1–5 mm · Kamień ciosany i beton: 1–3 cm · Metal cięty: 0,5–1 mm ·
Plastik formowany: 1–3 mm (zaokrąglony).
