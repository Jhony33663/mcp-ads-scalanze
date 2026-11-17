# mcp-ads-scalanze

Sistema MCP de gestión de pauta pagada para agencia Scalanze.
Conecta Claude Desktop con Meta Ads y TikTok Ads mediante Model Context Protocol.

## Qué hace este sistema

Permite operar campañas publicitarias directamente desde Claude Desktop usando lenguaje natural:

```
"Lista mis campañas activas en Meta Ads con métricas de las últimas 24h"
"Crea una campaña con objetivo LEAD_GENERATION, presupuesto $15/día, estado PAUSED"
"Resume el rendimiento de esta semana y dime qué optimizar"
```

## Servidores MCP incluidos

| Servidor       | Función                                      |
|----------------|----------------------------------------------|
| `github`       | Acceso a repositorios de landing pages       |
| `filesystem`   | Lectura del Brain (vault Obsidian del cliente) |
| `meta-ads`     | Gestión de campañas Meta via Marketing API   |
| `tiktok-ads`   | Gestión de campañas TikTok (opcional)        |

## Instalación rápida

### 1. Clonar y configurar

```bash
git clone https://github.com/Jhony33663/mcp-ads-scalanze.git
cd mcp-ads-scalanze
cp mcp-config-template.json mcp-config.json
```

Editar `mcp-config.json` y reemplazar los 4 tokens marcados como `REEMPLAZAR_CON_...`.

Ver guía completa en [`docs/ARQUITECTURA.md`](docs/ARQUITECTURA.md).

### 2. Pegar en Claude Desktop

Copiar el contenido de `mcp-config.json` al bloque `mcpServers` de:
```
C:\Users\TU_USUARIO\AppData\Roaming\Claude\claude_desktop_config.json
```

### 3. Reiniciar Claude Desktop

Los servidores MCP aparecen automáticamente al reiniciar.

## Estructura

```
mcp-ads-scalanze/
├── mcp-config-template.json     ← plantilla de configuración (tokens vacíos)
├── docs/
│   └── ARQUITECTURA.md          ← arquitectura, flujos, troubleshooting, checklist
└── clientes/
    └── prestasalud/
        └── PAUTA.md             ← rúbrica de creativos, KPIs B2B Colombia, flujo operativo
```

## Requisitos previos

- Node.js 18+ (para servidores MCP npm)
- Python 3.9+ (solo si se usa TikTok Ads)
- Claude Desktop con soporte MCP
- Cuenta Meta Developer con app creada
- GitHub Personal Access Token

## Clientes configurados

| Cliente              | Plataformas        | Archivo de operación                  |
|---------------------|--------------------|---------------------------------------|
| PrestaSalud Colombia | Meta Ads (B2B)     | `clientes/prestasalud/PAUTA.md`       |

---

Desarrollado por [Scalanze](https://scalanze.com) · Powered by Claude + MCP
