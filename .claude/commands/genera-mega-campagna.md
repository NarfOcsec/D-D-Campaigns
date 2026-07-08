---
description: Genera una MEGA-campagna 5e (scala tipo "The Enemy Within") in modo ricorsivo, a capitoli, ripartibile
---

Genera una **mega-campagna di D&D 5ª edizione** di grande scala e complessità (riferimento: campagne
epiche multi-capitolo alla *The Enemy Within*), orchestrando la squadra in `.claude/agents/`.

A differenza di `/genera-campagna`, qui **non** si scrive tutto in un colpo: si costruisce la
macro-struttura, poi si **itera capitolo per capitolo**, verificando la coerenza a ogni livello e
salvando lo stato per poter **fermarsi e riprendere**.

Premessa dell'utente:

> $ARGUMENTS

## 0. Parametri (chiedi solo ciò che manca, in una domanda sola)

Servono: tono/tema, numero e livello dei PG (partenza → arrivo), **numero di parti e capitoli**
desiderati (o una durata totale in sessioni), e quante **iterazioni fare in questo lancio** (default: la
struttura completa + i primi 2 capitoli, poi ci si ferma per non esplodere costi/tempo). Non inventare
questi valori.

Verifica il server dati: se i tool `ddb_*` ci sono, usali per mostri/incantesimi/oggetti/regole ufficiali;
altrimenti genera da SRD 5.1 marcando le parti «da verificare» (vedi `CLAUDE.md`) e avvisa l'utente.

## 1. Macro-struttura (una volta sola)

Crea `campaigns/<slug>/` da `campaigns/_template/`, poi:

1. **loremaster** (alto livello) → mondo, cosmologia, fazioni maggiori, temi portanti in `campaign.md`.
2. **plot-architect** (alto livello) → arco complessivo in **Parti → Capitoli**, con per ogni capitolo:
   titolo, premessa, fazioni coinvolte, livello atteso dei PG in ingresso/uscita, e i colpi di scena che
   lo collegano al precedente/successivo.
3. Scrivi il **file di stato** `campaigns/<slug>/INDEX.md` (vedi formato sotto): l'indice di tutte le
   parti/capitoli con stato `[ ] da fare`. Questo file è la fonte di verità del progresso.

## 2. Iterazione per capitolo (ripeti fino al limite di questo lancio)

Per ogni capitolo ancora `[ ] da fare` in `INDEX.md`, in ordine:

1. Crea `campaigns/<slug>/capitoli/NN-<slug-capitolo>/` con dentro `capitolo.md`, `npcs/`,
   `encounters/`, `sessions/`.
2. **loremaster** (locale) → approfondisci solo i luoghi/fazioni di questo capitolo.
3. **plot-architect** (locale) → i beat di trama del capitolo, le biforcazioni, e la lista di
   PNG/incontri/luoghi da produrre.
4. In parallelo (una risposta, più Task): **npc-smith** → `npcs/`, **encounter-designer** →
   `encounters/` con budget XP.
5. **loot-warden** → ricompense del capitolo, coerenti con la progressione globale.
6. **session-planner** → le sessioni del capitolo in `sessions/`.
7. **continuity-editor** → passaggio di coerenza **sul capitolo E rispetto ai capitoli precedenti**:
   aggancia i fili aperti, verifica che i livelli combacino, elenca i «da verificare».
8. Aggiorna `INDEX.md`: segna il capitolo `[x] fatto` e annota i **fili aperti** che i capitoli
   successivi devono pagare.

Dopo ogni capitolo, riporta all'utente 3-4 righe di sintesi. **Fermati** quando raggiungi il numero di
iterazioni concordato o quando l'utente lo chiede.

## 3. Ripresa

All'inizio, se `campaigns/<slug>/INDEX.md` esiste già, **non rigenerare la struttura**: leggi lo stato,
riprendi dal primo capitolo `[ ] da fare` e continua dal punto 2.

## Formato di `INDEX.md`

```markdown
# <Titolo Campagna> — Indice

Premessa: ...
PG: N personaggi, liv. X → Y
Dati: ddb_* attivi | SRD (da verificare)

## Stato capitoli
### Parte I — <titolo>
- [x] Cap. 01 — <titolo>  (liv. 1→3)  · fili aperti: <...>
- [ ] Cap. 02 — <titolo>  (liv. 3→4)

### Parte II — <titolo>
- [ ] Cap. 03 — <titolo>  (liv. 4→6)

## Fili narrativi globali (da pagare)
- <filo> — introdotto in Cap. 01 — da chiudere entro Cap. ...
```

## Regole trasversali

- **Coerenza prima di tutto**: mai contraddire `campaign.md`, `INDEX.md` o i capitoli precedenti. In caso
  di conflitto, `INDEX.md` vince e vai corretto a monte.
- **Niente statistiche inventate**: `ddb_*`, SRD noto, o marcato «da verificare».
- **Non duplicare**: linka a PNG/incontri/luoghi esistenti con path relativi.
- **Budget XP** calcolato per ogni incontro, coerente col livello del capitolo.
- Scrivi in italiano.
