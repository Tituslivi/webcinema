# Sessions de cinema en català — dades obertes

Base de dades de les sessions de cinema **en català** (doblades o en versió
original catalana) programades als cinemes de Catalunya. S'actualitza
diàriament a partir de les cartelleres públiques dels cinemes.

Generada pel projecte [webcinema.cat](https://webcinema.cat).

## Com consumir-ho

Punt d'entrada estable: [`manifest.json`](manifest.json). Seguiu-ne els camps
`href`; la resta de rutes poden canviar entre versions de l'esquema.

```js
const base = "https://raw.githubusercontent.com/Tituslivi/webcinema/main";
const manifest = await fetch(`${base}/manifest.json`).then((r) => r.json());
const upcoming = await fetch(`${base}/${manifest.upcoming.href}`).then((r) => r.json());
console.log(upcoming.summary, upcoming.sessions);
```

Les mateixes rutes se serveixen també per GitHub Pages:
`https://tituslivi.github.io/webcinema/manifest.json`. Tots dos orígens
envien `Access-Control-Allow-Origin: *`, així que es poden consultar des del
navegador de qualsevol domini.

## Fitxers

| Fitxer | Contingut |
|---|---|
| `manifest.json` | Índex general: setmana vigent, sessions futures, índexs |
| `upcoming.json` | **Totes les sessions futures conegudes** (d'avui endavant) — el tall més útil per a una web externa |
| `latest.json` | La setmana vigent (divendres a dijous), autocontinguda |
| `weeks/AAAA-MM-DD_AAAA-MM-DD.json` | Snapshots setmanals immutables (històric) |
| `films/index.json` + `films/<slug>.json` | Índex de pel·lícules i sessions per pel·lícula |
| `cinemas/index.json` + `cinemas/<slug>.json` | Índex de cinemes i sessions per cinema |
| `municipis/index.json` + `municipis/<slug>.json` | Índex de municipis i sessions per municipi |

## Format d'una sessió

```json
{
  "id": "1a2b3c4d5e6f7a8b",
  "filmSlug": "toy-story-5-en-catala",
  "filmTitle": "Toy Story 5",
  "cinemaSlug": "cinemes-majestic",
  "cinemaName": "Cinemes Majèstic",
  "municipiSlug": "tarrega",
  "municipiName": "Tàrrega",
  "sessionDate": "2026-07-04",
  "sessionTime": "18:45",
  "languageLabel": "Doblada al català",
  "bookingUrl": "https://…",
  "pickerUrl": "https://…",
  "sourceUrl": "https://…"
}
```

- Dates en ISO (`AAAA-MM-DD`), hores locals (`HH:MM`).
- `id` és estable per a la mateixa sessió entre actualitzacions.
- `languageLabel` pren tres valors: `Doblada al català`, `VO en català`, `Català`.
- Només s'hi inclouen sessions amb **àudio en català**; les versions
  subtitulades de films en altres llengües no hi són.

## Cadència i qualitat

- Actualització **diària** (el gran recanvi de cartellera és divendres).
- Les dades s'extreuen automàticament de les webs dels cinemes i poden
  contenir errors o quedar desfasades si un cinema canvia la programació:
  confirmeu sempre l'horari a l'enllaç de compra (`bookingUrl`).
- El camp `generatedAt` de cada fitxer indica el moment de l'extracció.
- `sourceIssues` llista les fonts que no s'han pogut consultar aquell dia.

## Ús i atribució

Dades recopilades de cartelleres públiques. Ús lliure; s'agraeix l'atribució
amb un enllaç a [webcinema.cat](https://webcinema.cat).
