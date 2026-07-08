# D&D Campaigns

Baseline per la generazione di campagne di **Dungeons & Dragons 5ª edizione**, costruita sopra il
server MCP di **D&D Beyond** ([`ddb-mcp`](./ddb-mcp)).

L'idea: collegare Claude direttamente al tuo account D&D Beyond (personaggi, campagne, manuali,
incantesimi, mostri, oggetti) e usarlo come *fonte di verità* delle regole 5e mentre generi e gestisci
le tue campagne dentro `campaigns/`.

```
D-D-Campaigns/
├── ddb-mcp/          # server MCP D&D Beyond (vendored da ddb-mcp/ddb-mcp, MIT)
├── campaigns/        # le tue campagne generate (una cartella per campagna)
│   └── _template/    # scheletro da copiare per una nuova campagna
├── .mcp.json         # config progetto: Claude Code carica il server MCP da qui
└── README.md
```

## Setup locale (una volta sola)

Serve **Node.js 18+** (testato con 22).

```bash
cd ddb-mcp
npm install
npx playwright install chromium   # il login a D&D Beyond usa un browser Chromium
npm run build                     # compila TypeScript -> dist/
```

## Collegare il server MCP a Claude Code

`.mcp.json` in root è già configurato: aprendo Claude Code **da questa cartella**, il server
`dndbeyond` viene caricato automaticamente (dopo aver fatto `npm run build`).

In alternativa, registralo a mano:

```bash
claude mcp add dndbeyond node "$(pwd)/ddb-mcp/dist/index.js"
```

## Login a D&D Beyond

Al primo utilizzo chiama il tool `ddb_login` (autenticazione Wizards ID, si apre il browser).
La sessione viene salvata in `~/.config/ddb-mcp/session.json` — per uscire, cancella quel file.

## Tool disponibili

| Tool | Cosa fa |
|------|---------|
| `ddb_login` | Login Wizards ID (setup iniziale) |
| `ddb_list_characters` / `ddb_get_character` | Elenco e dettaglio personaggi |
| `ddb_list_campaigns` / `ddb_get_campaign` | Gestione campagne |
| `ddb_list_library` / `ddb_read_book` | Accesso ai manuali |
| `ddb_search` | Cerca incantesimi, mostri, oggetti, razze, classi, talenti |
| `ddb_navigate` / `ddb_interact` / `ddb_current_page` | Controllo del browser |
| `ddb_download_character` | Esporta i dati di un personaggio |

## Generare una campagna

1. Copia lo scheletro: `cp -r campaigns/_template campaigns/nome-campagna`
2. Chiedi a Claude di popolarla usando i tool `ddb_*` per attingere a regole, mostri e incantesimi 5e ufficiali.
3. Versiona: `git add campaigns/nome-campagna && git commit`

## Attribuzione

`ddb-mcp/` è una copia (vendored) del progetto [ddb-mcp/ddb-mcp](https://github.com/ddb-mcp/ddb-mcp),
licenza MIT. Vedi [`ddb-mcp/UPSTREAM.md`](./ddb-mcp/UPSTREAM.md) per la fonte e come aggiornarlo.
