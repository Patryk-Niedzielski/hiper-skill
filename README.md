# /hiper: skill hiperrealizmu dla Claude Code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

`/hiper` to skill dla [Claude Code](https://claude.com/claude-code). Kiedy go włączysz, agent tworzy
sceny, modele i materiały w grach (Unity, Unreal, Godot, własne silniki) i w Blenderze tak, żeby
wyglądały **jak zdjęcie z prawdziwego życia**, a nie jak typowa grafika z gry albo coś wygenerowanego przez AI.

*English summary below.*

## Co robi

Skill daje agentowi trzy główne zasady.

### 1. Realizm z jakości, nie z ilości
Agent ma używać **jak najmniej części**: obiektów, meshy, materiałów, trójkątów i draw calli.
Przed budową planuje budżet i przy każdym elemencie pyta, czy to musi być geometria, czy wystarczy
normal map, decal albo maska w shaderze. Geometria jest tylko tam, gdzie zmienia się sylwetka.

### 2. Unikanie wyglądu „zrobione przez AI”
Skill zawiera konkretne reguły tego, co odróżnia zdjęcie od renderu:
- prawdziwa skala (1 jednostka = 1 m) i realne wymiary,
- fazki na krawędziach, brak idealnej symetrii, zróżnicowane kopie,
- fizycznie poprawne wartości PBR (albedo, roughness, metal, IOR),
- logiczne zużycie i brud (grawitacja, wilgoć, dotyk, słońce),
- światło o prawdziwych temperaturach barwowych i natężeniach, filmowy tonemapping,
- umiarkowany postprocess.

Jest też lista typowych błędów, które zdradzają generyczne CG albo AI: wszystko czyste i nowe,
plastikowy połysk, przesycone kolory, równe rzędy, lewitujące obiekty i widoczny tiling.

W [`hiper/references/wartosci-fizyczne.md`](hiper/references/wartosci-fizyczne.md) jest ściąga
z wartościami albedo i roughness, metalami, IOR, temperaturami i natężeniami światła, typowymi
wymiarami obiektów i rozmiarami fazek.

### 3. Obowiązkowa pętla poprawek
Na końcu agent **nie może po prostu ogłosić, że jest gotowe**. Musi:
1. zrobić zrzuty ekranu lub rendery z kilku ujęć (blisko, daleko, z poziomu oczu, pod światło),
2. przejść checklistę: skala, cienie kontaktowe, materiały, tiling, światło, artefakty, budżet części,
   oraz test „czy nieznajomy wziąłby to za zdjęcie?”,
3. wypisać każdą znalezioną niedokładność,
4. wszystko naprawić,
5. powtarzać, aż lista będzie pusta.

Jeśli coś utknie albo wymagałoby złamania zasad projektu (np. z `CLAUDE.md`), agent mówi to wprost,
zamiast udawać, że wynik jest perfekcyjny.

## Instalacja

```bash
git clone https://github.com/Patryk-Niedzielski/hiper-skill.git
mkdir -p ~/.claude/skills
cp -R hiper-skill/hiper ~/.claude/skills/
```

Możesz też skopiować folder `hiper` do `.claude/skills/` w konkretnym projekcie.

## Użycie

W Claude Code wpisz:

```
/hiper zrób w Blenderze starą drewnianą ławkę w parku o zachodzie słońca
```

Skill włącza się też sam, gdy poprosisz o coś „hiperrealistycznego”, „fotorealistycznego” albo
„jak w prawdziwym życiu”.

---

## English summary

`/hiper` is a Claude Code skill for **photorealistic** work in game engines and Blender. It tells the agent to:

- **Keep the part count low**: realism comes from proportions, materials, lighting and believable wear,
  not from piling on geometry. Detail goes into normal maps, decals and shader masks.
- **Avoid the "AI-generated / generic CG" look**: real-world scale, bevelled edges, physically based
  material values, logical weathering, real light temperatures and intensities, restrained post-processing.
- **Run a mandatory verification loop**: take screenshots from several angles, check them against a
  checklist, list every inaccuracy, fix them all, and repeat until nothing is left. The agent may not
  call the result done without a fresh screenshot.

The skill text is written in Polish. Claude reads it fine and answers in whatever language you use.

Install it by copying the `hiper` folder into `~/.claude/skills/`, then type `/hiper <task>`.

## Licencja / License

[MIT](LICENSE) © 2026 Patryk Niedzielski
