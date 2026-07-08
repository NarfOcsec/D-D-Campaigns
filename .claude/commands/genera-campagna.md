---
description: Genera una campagna D&D 5e completa orchestrando la squadra di agenti
---

Genera una **campagna di D&D 5ª edizione completa** coordinando la squadra di agenti definita in
`.claude/agents/`. Premessa fornita dall'utente:

> $ARGUMENTS

Se la premessa è vuota o manca un dato essenziale (tono, numero e livello dei PG di partenza, livello
d'arrivo desiderato, durata approssimativa in sessioni), **chiedilo prima di iniziare** con una domanda
sola e sintetica. Non inventare questi parametri.

## Preparazione

1. Verifica il server dati: se i tool `ddb_*` sono presenti, li userai per regole/mostri/incantesimi
   ufficiali. Se mancano, avvisa l'utente che genererai da SRD 5.1 con le parti marcate «da verificare»
   (vedi `CLAUDE.md`), poi procedi comunque.
2. Scegli uno slug per la campagna e crea la cartella copiando il template:
   `campaigns/<slug>/` a partire da `campaigns/_template/`.

## Pipeline degli agenti (usa il tool Task per delegare)

Esegui in quest'ordine, passando a ogni agente il contesto prodotto dai precedenti. Dopo ogni fase,
riporta all'utente un riassunto di 2-3 righe prima di procedere alla successiva.

1. **loremaster** → ambientazione, fazioni, temi in `campaign.md`.
2. **plot-architect** → arco in atti, biforcazioni, progressione di livello. Ottieni la lista di
   PNG/incontri/luoghi da produrre.
3. In parallelo (una singola risposta con più Task), sui rispettivi elenchi:
   - **npc-smith** → i file in `npcs/`
   - **encounter-designer** → i file in `encounters/` con budget XP
4. **loot-warden** → `loot.md` lungo la progressione.
5. **session-planner** → le sessioni in `sessions/`.
6. **continuity-editor** → passaggio finale: `README.md` con stato, report di coerenza e lista
   «da verificare».

## Chiusura

Presenta all'utente:
- la struttura di file creata,
- lo stato dal `continuity-editor`,
- le voci «da verificare» su D&D Beyond,
- e chiedi se vuole rifinire una sezione (puoi rilanciare il singolo agente col tool Task).

Scrivi tutto in italiano. Non duplicare contenuti tra file: usa link relativi.
