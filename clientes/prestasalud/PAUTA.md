# PAUTA — PrestaSalud Colombia

Gestión de pauta pagada para PrestaSalud Colombia SAS en Meta (Instagram/Facebook) y TikTok.
Audiencia objetivo: Directores de RRHH, líderes SST, C-level de empresas medianas/grandes (200+ colaboradores).

---

## 1. Stack MCP (conexión desde Claude Desktop)

Ver `mcp-config-pauta.json` en la raíz del Brain para el bloque listo para pegar en `claude_desktop_config.json`.

Servidores activos:
- `github` → acceso al repositorio `psaludlp` para revisar landing pages
- `filesystem` → acceso de lectura al Brain para contexto de marca
- `meta-ads` → gestión de campañas Meta via MCP (autenticación por navegador)
- `tiktok-ads` → gestión de campañas TikTok via servidor local Python

---

## 2. Estructura de Campaña Recomendada

### Meta Ads (Instagram + Facebook)

```
Cuenta publicitaria: PrestaSalud Colombia
│
├── Campaña: PREVENCIÓN_RIESGOS_[mes]
│   ├── Ad Set: Decisores RRHH CO (intereses: Recursos Humanos, ARL, Bienestar laboral)
│   │   ├── Creativo A: Video testimonio (15-30s) — Hook empresa → problema → solución PS
│   │   └── Creativo B: Carrusel — 3 frentes del negocio
│   └── Ad Set: Retargeting visitantes web (píxel)
│
├── Campaña: SALUD_MENTAL_LABORAL_[mes]
│   └── Ad Set: C-level 200+ empleados
│
└── Campaña: TALENTO_Y_SELECCION_[mes]
    └── Ad Set: Directores Gestión Humana
```

### TikTok Ads

Solo usar si el cliente aprueba formato video corto.
Audiencia B2B en TikTok es estrecha en Colombia: validar primero con campaña de alcance mínimo ($20 USD/semana).

---

## 3. Evaluación Pre-Lanzamiento de Creativos

Antes de cualquier inversión, el creativo pasa por este checklist:

### Rúbrica de 8 dimensiones (B2B salud organizacional)

| # | Dimensión | Pregunta clave | Peso |
|---|-----------|----------------|------|
| 1 | **Hook** (primeros 3s) | ¿Nombra un dolor real del decisor en los primeros 3 segundos? | 25% |
| 2 | **Mensaje rector** | ¿Aparece "bienestar organizacional" o los 3 frentes? | 15% |
| 3 | **Prueba social** | ¿Hay empresa real, número verificable o caso? | 15% |
| 4 | **Ritmo / duración** | Video ≤30s para cold, ≤60s para retargeting | 10% |
| 5 | **CTA** | ¿Es específico? ("Conoce el diagnóstico gratuito" > "Contáctanos") | 15% |
| 6 | **Voz de marca** | ¿Corporativo + humano? ¿Evita tono clínico frío? | 10% |
| 7 | **Visual** | ¿Respeta paleta cyan/azul? ¿Personas reales en entorno laboral? | 5% |
| 8 | **Compliance** | ¿Evita "depresión" como etiqueta visible? ¿No dice "para empresas" en hero? | 5% |

Puntaje mínimo para lanzar: **70/100**.
Puntaje Hook mínimo: **18/25** (si el hook falla, el creativo falla).

**Prompt de evaluación (pegar en Claude con el creativo adjunto):**
```
Evalúa este creativo para PrestaSalud Colombia usando la rúbrica de 8 dimensiones de PAUTA.md.
Audiencia: directores de RRHH y C-level de empresas colombianas con 200+ colaboradores.
Voz: corporativa, humana, estratégica. No clínica.
Dame puntaje por dimensión, puntaje total y 3 mejoras concretas si no llega a 70.
```

---

## 4. KPIs y Umbrales Meta B2B Colombia

| Métrica | Umbral mínimo | Umbral bueno | Acción si no llega |
|---------|--------------|--------------|-------------------|
| CTR (clic al link) | >0.8% | >1.5% | Cambiar creativo o copy |
| CPM | <$8 USD | <$5 USD | Revisar audiencia y placement |
| CPC | <$1.5 USD | <$0.80 USD | Mejorar relevance score |
| Frecuencia | <2.5 | <2.0 | Rotar creativos |
| Hook Rate (3s video) | >30% | >50% | Rehacer los primeros 3s |
| Hold Rate (25% video) | >25% | >40% | Acortar video o mejorar ritmo |
| CPL (costo por lead) | <$15 USD | <$8 USD | Optimizar landing o CTA |

Evaluar a las **48h** del lanzamiento y a los **7 días**.

---

## 5. Flujo Operativo de Pauta

```
1. BRIEF
   └── Completar plantilla de Brief (sección 6) → guardar en Brain

2. CREATIVO
   └── Grabar/diseñar → Evaluar con rúbrica (sección 3)
   └── Si puntaje ≥70 → continuar
   └── Si puntaje <70 → iterar hasta pasar

3. CONFIGURACIÓN DE CAMPAÑA
   └── Usar prompt MCP: "Crea campaña [nombre] en Meta Ads con ad set [parámetros]"
   └── Estado inicial: PAUSADO
   └── Revisar configuración antes de activar

4. LANZAMIENTO
   └── Activar con presupuesto mínimo de prueba ($10-20 USD/día)
   └── Registrar fecha de inicio en METRICAS.md

5. REVISIÓN 48h
   └── Prompt: "Lista métricas de las últimas 48h de campaña [nombre] en Meta Ads"
   └── Comparar con umbrales (sección 4)
   └── Decisión: escalar / rotar creativo / pausar

6. REPORTE SEMANAL
   └── Prompt: "Resume rendimiento de campañas activas de PrestaSalud Colombia esta semana"
   └── Registrar hallazgos en METRICAS.md
```

---

## 6. Plantilla de Brief por Campaña

```markdown
## Brief: [nombre campaña] — [mes/año]

**Objetivo de negocio:** [ej. generar leads para frente de Prevención de Riesgos]
**Objetivo de campaña Meta:** [Conversiones / Alcance / Tráfico]
**Presupuesto:** $[X] USD / [X] días
**Audiencia principal:** [ej. Directores RRHH, empresas 200+ empleados, Colombia]
**Plataforma:** [Meta Instagram / Meta Facebook / TikTok]
**Formato creativo:** [Video 15s / Carrusel / Imagen estática]
**Mensaje central:** [1 frase que siga el mensaje rector de la marca]
**CTA:** [texto exacto del botón]
**Destino:** [URL de la landing o formulario]
**Hook propuesto (primeros 3s):** [describe qué se ve/escucha]
**Restricciones de marca activas:**
  - No usar "para empresas" en copy visible
  - No mostrar "Depresión" como etiqueta
  - No tono clínico
**Métricas de éxito a 7 días:** CTR>X%, CPL<$X
```

---

## 7. Prompts de Operación MCP

### Ver campañas activas
```
Lista mis campañas activas en Meta Ads para la cuenta de PrestaSalud Colombia con métricas de las últimas 24h.
```

### Evaluar creativo antes de lanzar
```
Soy el equipo de marketing de PrestaSalud Colombia (bienestar organizacional B2B, Colombia).
Voy a subir este creativo [adjuntar]. Antes de lanzarlo, evalúalo con la rúbrica de 8 dimensiones
de PAUTA.md y dime si tiene puntaje ≥70 para aprobar lanzamiento.
```

### Diagnóstico de campaña activa
```
La campaña [nombre] lleva [X] días activa. CTR=[X]%, CPM=$[X], Frecuencia=[X].
Según los umbrales de PAUTA.md para PrestaSalud Colombia, ¿qué acción tomar?
```

### Duplicar campaña ganadora
```
Duplica la campaña [nombre] en Meta Ads con presupuesto $[X]/día, misma audiencia,
nuevo creativo [descripción]. Estado: PAUSADO para revisión previa.
```

---

## 8. Registro de Campañas

| Campaña | Plataforma | Fecha inicio | Presupuesto | CPL | Estado | Nota |
|---------|-----------|-------------|-------------|-----|--------|------|
| — | — | — | — | — | — | Registrar aquí cada campaña lanzada |

---

## 9. Historial de Creativos

| ID | Formato | Hook (primeros 3s) | Puntaje rúbrica | CTR real | Resultado |
|----|---------|-------------------|----------------|----------|-----------|
| — | — | — | — | — | — |
