---
name: hiper
description: Tryb hiperrealizmu dla gier, silników (Unity, Unreal, Godot, własne silniki) i Blendera. Wynik ma wyglądać jak zdjęcie z prawdziwego życia, a nie jak coś wygenerowanego przez AI, przy możliwie małej liczbie części, obiektów i trójkątów. Na koniec agent sprawdza każdą niedokładność na zrzutach ekranu i poprawia w pętli, aż wynik będzie bez zarzutu. Użyj, gdy użytkownik wpisze /hiper albo poprosi o coś „hiperrealistycznego”, „jak w prawdziwym życiu” lub „fotorealistycznego”.
---

# /hiper — hiperrealizm przy małej liczbie części

Cel: ktoś, kto patrzy na wynik, ma pomyśleć „to zdjęcie”, a nie „to render” ani „to zrobiło AI”.
Ograniczenie: używaj **jak najmniej części** (obiektów, meshy, materiałów, trójkątów, draw calli).
Realizm ma pochodzić z **jakości** (proporcje, materiały, światło, niedoskonałości), a nie z **ilości**.

Obowiązują ograniczenia projektu i użytkownika. Jeśli pamięć lub CLAUDE.md coś zabrania
(np. ray tracingu), nie wprowadzaj tego, nawet jeśli poprawiłoby realizm.

---

## Faza 0: Zanim cokolwiek zbudujesz

1. **Ustal prawdziwy odpowiednik.** Co to jest w realnym świecie? Z jakiego materiału, ile ma lat,
   gdzie stoi, jaki jest klimat, pora dnia i pogoda? Zapisz to w 3–5 zdaniach. To jest Twój „brief”.
2. **Weź prawdziwe wymiary.** Wszystko w skali 1 jednostka = 1 metr. Sprawdź wymiary realnego obiektu
   (drzwi ok. 2,0–2,1 m, stopień schodów 17–18 cm, blat 90 cm, człowiek 1,75 m). Typowe wartości są w
   [references/wartosci-fizyczne.md](references/wartosci-fizyczne.md). Zła skala jest najczęstszą
   przyczyną tego, że coś „wygląda jak gra”.
3. **Zaplanuj budżet części.** Zanim zaczniesz, wypisz listę elementów i przy każdym zapytaj:
   *czy to musi być osobna geometria, czy wystarczy normal map, decal, maska albo shader?*
   - Geometria tylko tam, gdzie zmienia się **sylwetka** albo gdzie kamera podejdzie blisko.
   - Drobny detal (śruby, fugi, rysy, szwy, pęknięcia) robisz w teksturze, normal mapie lub decalu.
   - Powtarzalne elementy: instancing, a nie kopie.
   - Jeden materiał z maskami (vertex color, mapa ID) zamiast wielu materiałów.
   - Wypisz budżet (np. „budynek: maks. 6 meshy, 2 materiały, ok. 8k tri”) i trzymaj się go.

## Faza 1: Budowa — co odróżnia „prawdziwe” od „AI/gra”

### Kształt
- **Żadnych idealnie ostrych krawędzi.** W realu każda krawędź ma fazkę lub zaokrąglenie
  (bevel 1–5 mm na meblach, 1–2 cm na betonie). Jedna fazka z weighted normals daje więcej niż 10 tys. trójkątów.
- **Żadnej idealnej symetrii ani prostych linii tam, gdzie natura ich nie ma.** Lekko przechyl,
  ugnij, przesuń: deska nie jest idealnie prosta, kamień nie jest kulą, dach lekko się ugina.
- **Proporcje z realnego świata**, nie „na oko”: grubość ścian 25–40 cm, grubość blatu, średnica pnia.
- **Zróżnicuj skalę i obrót** powtarzalnych elementów (±5–15% skali, losowy obrót) — kopie 1:1 wyglądają sztucznie.

### Materiały (PBR)
- Używaj **fizycznie poprawnych wartości** albedo i roughness (tabela w references). Albedo nigdy
  nie jest czystą bielą (maks. ok. 0,9) ani czystą czernią (min. ok. 0,02).
- Metal jest albo metalem (metallic = 1), albo nie (0). Wartości pośrednie tylko na przejściach (rdza, brud).
- **Roughness to najważniejsza mapa.** Nigdy nie zostawiaj jej jednolitej: odciski palców, kurz,
  wytarte miejsca, zacieki. Zmienność roughness sprzedaje realizm bardziej niż albedo.
- **Warstwy zużycia zgodne z logiką świata:** brud gromadzi się w zagłębieniach i na dole,
  krawędzie są wytarte, zacieki biegną w dół od parapetów, mech rośnie od strony wilgotnej,
  słońce wybiela górne powierzchnie.
- **Zabij powtarzalność tekstur:** macro-variation (druga, duża skala szumu), triplanar lub stochastic
  sampling, decale łamiące kafelki. Widoczny tiling od razu zdradza CG.

### Światło (tu jest większość realizmu)
- **Jedno dominujące źródło + realistyczne odbite światło.** Słońce o prawdziwej intensywności,
  niebo jako światło wypełniające. Nie dodawaj „ładnych” lamp tylko po to, żeby coś doświetlić.
- **Temperatury barwowe z realu:** słońce w południe ok. 5500 K, złota godzina ok. 3500 K,
  cień na zewnątrz jest niebieskawy (niebo), żarówka 2700 K.
- **Kontaktowe cienie i AO** w miejscach styku. Obiekt bez cienia kontaktowego „lewituje”.
- **Ekspozycja jak w aparacie**, tonemapping filmowy (ACES / AgX / Filmic), a nie liniowy ani „Standard”.
- Atmosfera: lekka mgła lub aerial perspective na dalszym planie. Dal jest jaśniejsza i mniej nasycona.

### Kamera i postprocess (z umiarem)
- Ogniskowa jak w prawdziwym aparacie (24–50 mm dla widoku z poziomu oczu), kamera na wysokości ok. 1,6–1,7 m.
- Subtelnie: winieta, aberracja chromatyczna na brzegach, ziarno, delikatny bloom tylko na jasnych
  źródłach, głębia ostrości zgodna z przysłoną. **Przesadzony postprocess to typowy znak AI.**

### Czego unikać — znaki „zrobione przez AI / generyczne CG”
- wszystko czyste, nowe, nasycone i równo oświetlone,
- idealnie równe odstępy, rzędy i siatki,
- plastikowy połysk na wszystkim (jednolita, niska roughness),
- zbyt wiele „ciekawych” detali naraz, chaos bez logiki,
- przesycone kolory, przepalony bloom, HDR-owy look,
- obiekty lewitujące lub wbite w podłoże, bez cienia kontaktowego,
- powtarzające się kopie tego samego assetu w tej samej orientacji,
- tekst, logo i napisy z bełkotem.

Realizm = **opowieść o czasie**: każdy obiekt powinien wyglądać, jakby był używany, wystawiony na
pogodę i miał historię, ale w sposób logiczny, a nie losowy.

## Faza 2: Narzędzia — jak to robić konkretnie

- **Blender (skryptem `bpy`):** Bevel + Weighted Normal zamiast gęstej siatki; Principled BSDF z teksturami
  w szumach i maskach (Noise, Musgrave/Voronoi, Pointiness / AO jako maska brudu); Geometry Nodes lub
  instancing do rozrzucania; Filmic/AgX; render podglądowy przez `blender -b ... -o ... -f 1`.
- **Silnik gry (Unity, Unreal, Godot, własny silnik):** LOD-y, instancing, atlas lub jeden materiał z maskami, decale,
  baked AO/normal z high-poly na low-poly, kontrola draw calli i liczby trójkątów. Przestrzegaj zasad
  projektu z CLAUDE.md i pamięci (np. zakaz ray tracingu, budowa tylko przez skrypty edytora).
- Jeśli edytor nie ma czegoś, czego potrzebujesz do realizmu (np. decali, scattera), dodaj to jako
  funkcję wielokrotnego użytku, zgodnie z zasadami projektu.

## Faza 3: Obowiązkowa pętla weryfikacji (nie pomijaj)

Nie kończ zadania, dopóki nie przejdziesz tej pętli. Po każdej zmianie:

1. **Zrób zrzut ekranu lub render** z kilku ujęć: z poziomu oczu, z bliska (detal), z daleka
   (sylwetka) i pod światło. Obejrzyj je narzędziem Read (obrazy). Nie oceniaj na podstawie kodu.
2. **Sprawdź wynik krytycznie, jak wymagający art director**, punkt po punkcie:
   - [ ] Skala i proporcje zgadzają się z realnymi wymiarami (porównaj z drzwiami lub człowiekiem).
   - [ ] Brak lewitujących lub przenikających się obiektów; są cienie kontaktowe.
   - [ ] Krawędzie mają fazki i łapią światło.
   - [ ] Roughness jest zróżnicowana; nic nie wygląda jak plastik.
   - [ ] Brak widocznego tilingu i identycznych kopii.
   - [ ] Zużycie i brud są logiczne (grawitacja, wilgoć, dotyk, słońce).
   - [ ] Światło: jedno dominujące źródło, właściwe temperatury barwowe, poprawna ekspozycja,
         nic nie jest przepalone ani zbyt ciemne.
   - [ ] Kolory nie są przesycone; dal ma atmosferę.
   - [ ] Brak artefaktów: z-fighting, migotanie, dziury w siatce, odwrócone normalne, szwy UV,
         rozciągnięte tekstury, peter-panning cieni, aliasing, banding.
   - [ ] Budżet części dotrzymany (podaj liczby: obiekty, materiały, trójkąty).
   - [ ] **Test „zdjęcia”:** czy nieznajomy wziąłby to za zdjęcie? Jeśli nie, nazwij konkretnie, co to zdradza.
3. **Wypisz każdą znalezioną niedokładność** jako konkretną listę (co, gdzie, dlaczego źle).
4. **Napraw wszystkie**, zaczynając od tych, które najbardziej psują realizm (zwykle: skala, światło,
   materiały, a dopiero potem detal).
5. **Wróć do kroku 1.** Powtarzaj, aż lista z kroku 3 będzie pusta i test „zdjęcia” przechodzi.

Zasady pętli:
- Nie ogłaszaj „gotowe” ani „perfekcyjne” bez świeżego zrzutu ekranu po ostatniej poprawce.
- Jeśli poprawka czegoś nie poprawiła albo pogorszyła, cofnij ją i spróbuj innej metody,
  zamiast dokładać kolejne warstwy.
- Jeśli coś utknęło po kilku próbach albo osiągnięcie realizmu wymagałoby złamania zasad projektu,
  powiedz to wprost i opisz, co ogranicza wynik, zamiast udawać, że jest idealnie.
- Na koniec pokaż użytkownikowi finalne ujęcia oraz krótko: co poprawiono w pętli, ile iteracji
  było potrzebnych i jaki jest końcowy budżet części.
