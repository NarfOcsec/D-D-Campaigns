# D&D Campaigns — istruzioni di progetto

Questo repo genera **campagne di D&D 5ª edizione** con una squadra di agenti, usando il server MCP
`dndbeyond` (cartella `ddb-mcp/`) come fonte dati ufficiale quando disponibile.

## Regola d'oro sui dati 5e

Quando ti servono regole, statistiche di mostri, incantesimi, oggetti magici, razze, classi o talenti:

1. **Prima** prova i tool del server MCP `dndbeyond` (`ddb_search`, `ddb_read_book`, `ddb_get_character`,
   `ddb_get_campaign`, ecc.). Sono la fonte ufficiale collegata all'account D&D Beyond dell'utente.
2. **Se il server non è connesso** (i tool `ddb_*` non esistono in questa sessione), usa la tua
   conoscenza dello **SRD 5.1** come fallback e **segnalalo esplicitamente** nel testo generato
   (es. `> ⚠️ generato da SRD, non verificato su D&D Beyond`), così l'utente sa cosa ricontrollare.

Non inventare mai statistiche di mostri o incantesimi "plausibili": o vengono da `ddb_*`, o dallo SRD
noto, o le marchi come da verificare.

## Verifica se il server è attivo

I tool `ddb_*` appaiono nella lista strumenti solo se `dndbeyond` è connesso. Se l'utente sta lavorando
da questo progetto e i tool mancano, ricordagli: `cd ddb-mcp && npm run build`, poi riavvia Claude Code
da questa cartella e chiama `ddb_login`.

## Struttura di una campagna

Ogni campagna vive in `campaigns/<nome-campagna>/`. Parti sempre da `campaigns/_template/`.
File attesi dentro una campagna:

- `campaign.md` — bibbia della campagna (premessa, trama, fazioni, PNG, luoghi)
- `npcs/` — un file per PNG rilevante
- `encounters/` — incontri con budget XP/GS calcolato
- `loot.md` — tesori e progressione ricompense
- `sessions/` — la campagna spezzata in sessioni giocabili
- `README.md` — indice + stato di avanzamento

## Squadra di agenti

Sono definiti in `.claude/agents/`. Orchestrali con lo slash command `/genera-campagna`, oppure
invocali singolarmente col tool Task quando serve rifinire un pezzo:

| Agente | Responsabilità |
|--------|----------------|
| `loremaster` | Ambientazione, cosmologia, geografia, fazioni, temi |
| `plot-architect` | Arco narrativo in atti, ganci, biforcazioni, climax |
| `npc-smith` | PNG con motivazioni, voce, blocchi statistica |
| `encounter-designer` | Incontri bilanciati (budget XP/GS), tattiche |
| `loot-warden` | Tesori, oggetti magici, ritmo delle ricompense |
| `session-planner` | Suddivisione in sessioni da ~3-4 ore |
| `continuity-editor` | Coerenza, aggancio dei fili narrativi, controllo regole |

## Principi di bilanciamento

- Calcola il **budget XP per incontro** in base a numero e livello dei PG (linee guida DMG).
- Annota sempre **GS (CR)** e **XP** di ogni mostro; segnala incontri Deadly.
- Distribuisci gli oggetti magici secondo il ritmo del livello, non tutti insieme.

## Lingua

Le campagne si scrivono in **italiano** salvo diversa richiesta. I termini di gioco possono restare
in inglese quando è la forma con cui l'utente li cerca su D&D Beyond (es. "Fireball", "CR").
