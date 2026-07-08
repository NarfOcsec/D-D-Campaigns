# Upstream

Questa cartella è una copia (vendored) del server MCP di D&D Beyond:

- **Fonte:** https://github.com/ddb-mcp/ddb-mcp
- **Licenza:** MIT
- **Versione base:** 1.0.2 (branch `main`)

## Aggiornare da upstream

```bash
# da una checkout pulita
git clone --depth 1 https://github.com/ddb-mcp/ddb-mcp.git /tmp/ddb-mcp-upstream
# copia i sorgenti sopra questa cartella (senza .git / node_modules / dist)
cp -a /tmp/ddb-mcp-upstream/src/. ./src/
cp /tmp/ddb-mcp-upstream/package.json /tmp/ddb-mcp-upstream/tsconfig.json \
   /tmp/ddb-mcp-upstream/eslint.config.js ./
npm install && npm run build
```

Le automazioni CI/release dell'upstream (`.github/`) sono state rimosse di proposito:
questa è una baseline personale, non un pacchetto da pubblicare su npm.
