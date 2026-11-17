# Arquitectura MCP — Sistema de Pauta PrestaSalud Colombia

Centro de comando técnico del sistema de pauta pagada conectado a Claude Desktop vía MCP.
Audiencia objetivo: Juan Martín Puerta / Scalanze. Última actualización: 2026-11-17.

---

## 1. Mapa del sistema

```
┌─────────────────────────────────────────────────────────────────┐
│                        CLAUDE DESKTOP                           │
│                                                                 │
│  ┌──────────┐  ┌────────────┐  ┌───────────┐  ┌────────────┐  │
│  │  github  │  │ filesystem │  │ meta-ads  │  │ tiktok-ads │  │
│  │   MCP    │  │    MCP     │  │    MCP    │  │    MCP     │  │
│  └────┬─────┘  └─────┬──────┘  └─────┬─────┘  └─────┬──────┘  │
└───────┼──────────────┼───────────────┼───────────────┼──────────┘
        │              │               │               │
        ▼              ▼               ▼               ▼
  github.com    C:\Type_b\        Meta Marketing   TikTok Ads
  /Jhony33663   Brain-Ngrok-main  API v21.0        API (Python)
  /psaludlp     (vault Obsidian)  act_14250...     tiktok_ads_mcp
```

### Archivo de configuración central

```
C:\Users\JMATA\AppData\Roaming\Claude\claude_desktop_config.json
                               ↑
           pegar contenido de: mcp-config-pauta.json
                               (raíz del Brain)
```

---

## 2. Estado actual de credenciales

| Servidor     | Variable                    | Estado              | Acción pendiente               |
|--------------|-----------------------------|---------------------|--------------------------------|
| `github`     | `GITHUB_PERSONAL_ACCESS_TOKEN` | ❌ FALTA         | Ver sección 4-A                |
| `filesystem` | (sin credenciales)          | ✅ Listo            | —                              |
| `meta-ads`   | `META_ACCESS_TOKEN`         | ❌ FALTA            | Ver sección 4-B                |
| `meta-ads`   | `META_AD_ACCOUNT_ID`        | ✅ `act_1425624883112131` | —                     |
| `tiktok-ads` | `TIKTOK_APP_ID`             | ❌ FALTA            | Ver sección 4-C                |
| `tiktok-ads` | `TIKTOK_SECRET`             | ❌ FALTA            | Ver sección 4-C                |
| `tiktok-ads` | `TIKTOK_ACCESS_TOKEN`       | ❌ FALTA            | Ver sección 4-C                |
| `tiktok-ads` | `TIKTOK_ADVERTISER_ID`      | ❌ FALTA            | Ver sección 4-C                |

---

## 3. Flujo de activación completo

```
PASO 1 — GitHub PAT
  └── github.com → Settings → Developer settings → Personal access tokens (Classic)
  └── Scopes: repo + read:org
  └── Pegar en mcp-config-pauta.json → GITHUB_PERSONAL_ACCESS_TOKEN

PASO 2 — Meta Access Token
  └── App "PrestaSalud-Pauta" creada en developers.facebook.com ✅
  └── Ir a Graph API Explorer → Herramientas → Explorador de la API de Graph
  └── Seleccionar app: PrestaSalud-Pauta
  └── Generar token de usuario → permisos: ads_management + ads_read
  └── Pegar en mcp-config-pauta.json → META_ACCESS_TOKEN
  └── NOTA: token de usuario expira en 60 días → ver sección 5 para token de larga duración

PASO 3 — TikTok Ads (opcional, solo si el cliente aprueba)
  └── Ver sección 4-C

PASO 4 — Instalar servidor TikTok (solo si se usa TikTok)
  └── pip install tiktok-ads-mcp

PASO 5 — Pegar config en Claude Desktop
  └── Copiar contenido de mcp-config-pauta.json
  └── Abrir: C:\Users\JMATA\AppData\Roaming\Claude\claude_desktop_config.json
  └── Reemplazar mcpServers con el bloque nuevo
  └── Guardar y REINICIAR Claude Desktop

PASO 6 — Verificar conexión
  └── En Claude Desktop: "Lista mis campañas activas en Meta Ads para PrestaSalud Colombia"
  └── Debe responder con campañas o "no hay campañas activas"
```

---

## 4. Guías de obtención de credenciales

### 4-A. GitHub Personal Access Token

1. Ir a: `github.com` → tu avatar → **Settings**
2. Scroll hasta el final → **Developer settings**
3. **Personal access tokens** → **Tokens (classic)**
4. **Generate new token (classic)**
5. Nombre: `Claude-MCP-PrestaSalud`
6. Expiración: 90 días (o No expiration si prefieres)
7. Scopes a marcar:
   - ☑ `repo` (acceso completo a repositorios)
   - ☑ `read:org` (leer organización)
8. Clic **Generate token** → copiar el `ghp_...` inmediatamente
9. Pegar en `mcp-config-pauta.json`:
   ```json
   "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_TUTOKEN"
   ```

### 4-B. Meta Access Token (Marketing API)

**Pre-requisito:** App "PrestaSalud-Pauta" creada en `developers.facebook.com` ✅

1. Ir a: `developers.facebook.com` → **Herramientas** → **Explorador de la API de Graph**
2. En el menú desplegable superior derecho, seleccionar **PrestaSalud-Pauta**
3. Clic **Generar token de acceso**
4. Meta pedirá confirmar permisos — aceptar
5. En el panel de permisos, agregar:
   - `ads_management`
   - `ads_read`
   - `business_management` (opcional, para datos de Business Manager)
6. Copiar el token generado (`EAA...`)
7. Pegar en `mcp-config-pauta.json`:
   ```json
   "META_ACCESS_TOKEN": "EAAxxx..."
   ```

**Token de larga duración (recomendado — expira en 60 días en lugar de 1h):**

Una vez obtenido el token corto, extenderlo con esta llamada en Graph API Explorer:
```
GET /oauth/access_token
  ?grant_type=fb_exchange_token
  &client_id={APP_ID_DE_PRESTASALUD_PAUTA}
  &client_secret={APP_SECRET}
  &fb_exchange_token={TOKEN_CORTO}
```
El App ID y App Secret están en: `developers.facebook.com` → PrestaSalud-Pauta → Configuración → Básica.

### 4-C. TikTok Ads (activar solo si el cliente aprueba formato video)

**Pre-requisito:** Cuenta en `ads.tiktok.com` con acceso a TikTok for Business

1. Ir a: `business.tiktok.com` → **Developer Portal**
2. Crear app → tipo: **Web App**
3. Nombre: `PrestaSalud-MCP`
4. Anotar:
   - **App ID** → `TIKTOK_APP_ID`
   - **App Secret** → `TIKTOK_SECRET`
5. Para el **Access Token**:
   - En la app, ir a **Manage Accounts** → tu cuenta de anunciante
   - Generar token con scope `advertiser:read` y `advertiser:write`
   - → `TIKTOK_ACCESS_TOKEN`
6. **Advertiser ID**: visible en la URL de `ads.tiktok.com` al entrar a tu cuenta
   - → `TIKTOK_ADVERTISER_ID`
7. Instalar el servidor MCP:
   ```bash
   pip install tiktok-ads-mcp
   ```

---

## 5. Archivo de configuración completo

Ruta: `C:\Type_b\Brain-Ngrok-main\mcp-config-pauta.json`

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "REEMPLAZAR_CON_ghp_TU_TOKEN"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\Type_b\\Brain-Ngrok-main"
      ]
    },
    "meta-ads": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-facebook-ads"],
      "env": {
        "META_ACCESS_TOKEN": "REEMPLAZAR_CON_TU_TOKEN_META",
        "META_AD_ACCOUNT_ID": "act_1425624883112131"
      }
    },
    "tiktok-ads": {
      "command": "python",
      "args": ["-m", "tiktok_ads_mcp"],
      "env": {
        "TIKTOK_APP_ID": "REEMPLAZAR_CON_APP_ID",
        "TIKTOK_SECRET": "REEMPLAZAR_CON_SECRET",
        "TIKTOK_ACCESS_TOKEN": "REEMPLAZAR_CON_ACCESS_TOKEN",
        "TIKTOK_ADVERTISER_ID": "REEMPLAZAR_CON_ADVERTISER_ID"
      }
    }
  }
}
```

**Para pegar en Claude Desktop:**
```
C:\Users\JMATA\AppData\Roaming\Claude\claude_desktop_config.json
```
Reemplazar solo el bloque `"mcpServers": { ... }` si ya hay otras configuraciones.

---

## 6. Flujo operativo de pauta (una vez conectado)

### Lanzar una campaña nueva

```
1. Completar Brief en PAUTA.md sección 6
   └── Prompt: "Evalúa este creativo para PrestaSalud Colombia [adjuntar]"
   └── Esperar puntaje ≥70/100 en rúbrica de 8 dimensiones

2. Crear campaña vía Claude Desktop (MCP activo):
   "Crea una campaña en Meta Ads para la cuenta act_1425624883112131
    llamada PREVENCIÓN_RIESGOS_NOV2026 con objetivo LEAD_GENERATION,
    presupuesto diario $15 USD, estado PAUSED"

3. Revisar configuración en Ads Manager antes de activar

4. Activar y registrar en PAUTA.md sección 8

5. A las 48h:
   "Lista métricas de las últimas 48h de la campaña PREVENCIÓN_RIESGOS_NOV2026"
   → Comparar con umbrales de PAUTA.md sección 4
```

### Revisar landing antes de lanzar

```
"Lee el archivo index.html del repositorio psaludlp y dime si el CTA
 principal es coherente con el brief de campaña [nombre]"
```

### Reporte semanal

```
"Resume el rendimiento de todas las campañas activas de PrestaSalud Colombia
 esta semana. Incluye CTR, CPM, CPC, Frecuencia y CPL.
 Compara con los umbrales de PAUTA.md y da 3 acciones concretas."
```

---

## 7. Mapa de archivos del sistema de pauta

```
C:\Type_b\Brain-Ngrok-main\
│
├── mcp-config-pauta.json          ← config lista para pegar en Claude Desktop
│
└── Clientes\
    └── PrestaSalud Colombia\
        ├── PAUTA.md               ← rúbrica, KPIs, flujo operativo, brief template
        ├── ARQUITECTURA_MCP_PAUTA.md  ← este archivo
        ├── METRICAS.md            ← (crear) registro de campañas y resultados
        └── README.md              ← índice del cliente
```

---

## 8. Checklist de activación (imprimir y tachar)

```
[ ] PASO 1: Crear GitHub PAT en github.com/settings/tokens
            → pegar en mcp-config-pauta.json

[ ] PASO 2: Confirmar contraseña en modal de Meta (pestaña abierta)
            → se creará la app PrestaSalud-Pauta

[ ] PASO 3: Ir a developers.facebook.com → Herramientas → Explorador de la API de Graph
            → Seleccionar PrestaSalud-Pauta → Generar token
            → Permisos: ads_management + ads_read
            → pegar en mcp-config-pauta.json

[ ] PASO 4 (opcional): TikTok Ads — solo si el cliente aprueba
            → pip install tiktok-ads-mcp
            → obtener credenciales en business.tiktok.com
            → pegar en mcp-config-pauta.json

[ ] PASO 5: Abrir C:\Users\JMATA\AppData\Roaming\Claude\claude_desktop_config.json
            → reemplazar mcpServers con el contenido de mcp-config-pauta.json
            → guardar

[ ] PASO 6: Reiniciar Claude Desktop completamente

[ ] PASO 7: Verificar conexión con prompt:
            "Lista mis campañas activas en Meta Ads para PrestaSalud Colombia"

[ ] PASO 8: Primer Brief en PAUTA.md → primera campaña
```

---

## 9. Solución de problemas comunes

| Error | Causa probable | Solución |
|-------|---------------|----------|
| `Error: GITHUB_PERSONAL_ACCESS_TOKEN not set` | Token vacío en config | Verificar que el ghp_ está completo sin espacios |
| `Error: (#190) Access token has expired` | Token Meta expirado | Generar token de larga duración (sección 4-B) |
| `Error: (#100) Invalid ad account id` | ID de cuenta incorrecto | Usar `act_1425624883112131` con prefijo `act_` |
| MCP no aparece en Claude Desktop | Config no guardada o Claude no reiniciado | Guardar y reiniciar completamente |
| `ModuleNotFoundError: tiktok_ads_mcp` | Paquete Python no instalado | `pip install tiktok-ads-mcp` |
| Campañas no aparecen | Token sin permiso `ads_read` | Regenerar token con ese scope |

---

## 10. Tokens y renovación

| Token | Duración | Cuándo renovar | Dónde |
|-------|----------|----------------|-------|
| GitHub PAT | 90 días (si se configura así) | Antes de vencer | github.com/settings/tokens |
| Meta token corto | 1 hora | No usar en config | Solo para testing |
| Meta token largo | 60 días | Mes 2 de cada trimestre | Graph API Explorer + exchange |
| TikTok access token | Según configuración | Al expirar | business.tiktok.com |

**Recordatorio de renovación Meta:** Programar alarma cada 55 días para renovar el token largo.

---

## Referencias rápidas

- Ad Account ID: `act_1425624883112131`
- App Meta: `PrestaSalud-Pauta` (en `developers.facebook.com`)
- Repositorio sitio: `github.com/Jhony33663/psaludlp`
- Brain local: `C:\Type_b\Brain-Ngrok-main`
- Config Claude Desktop: `C:\Users\JMATA\AppData\Roaming\Claude\claude_desktop_config.json`
- Graph API Explorer: `developers.facebook.com/tools/explorer`
