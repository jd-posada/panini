# 📒 Álbum Panini FIFA World Cup 2026

Tracker personal para el álbum Panini Mundial 2026. Controla tus láminas (tenidas, faltantes, repetidas) por chat con Claude y visualiza tu colección en un dashboard web.

**Demo:** https://jd-posada.github.io/panini/

---

## ¿Cómo funciona?

- **`stickers.json`** — base de datos de tus 980 láminas
- **`index.html`** — dashboard visual publicado en GitHub Pages
- **Claude Code** — interfaz de chat para actualizar tu colección por voz o texto

---

## Instalación paso a paso

### 1. Haz Fork del repositorio
En GitHub, entra a `github.com/jd-posada/panini` y haz click en **Fork**.

### 2. Activa GitHub Pages
En tu fork: **Settings → Pages → Branch: master → / (root) → Save**

Tu dashboard quedará en: `https://TU-USUARIO.github.io/panini/`

### 3. Clona el repo en tu computador
```bash
git clone https://github.com/TU-USUARIO/panini.git
cd panini
```

### 4. Instala las herramientas necesarias

**Claude Code:**
```bash
npm install -g @anthropic-ai/claude-code
```

**GitHub CLI** (para hacer push desde Claude):
```bash
# macOS
brew install gh

# Windows
winget install GitHub.cli
```

**Autentícate en GitHub:**
```bash
gh auth login
```

### 5. Resetea la colección a cero
Edita `stickers.json` y cambia `metadata.collected` a `0`.  
O pídele a Claude: *"resetea toda mi colección a cero"*.

### 6. Abre Claude Code en la carpeta del proyecto
```bash
cd panini
claude
```

---

## Cómo usarlo

Habla con Claude en español. Ejemplos:

| Comando | Efecto |
|---|---|
| `completa estas: ARG7, BRA3, COL1` | Marca láminas como obtenidas |
| `valida estas: MEX1, ESP5` | Verifica si las tienes sin modificar nada |
| `¿qué me falta de Brasil?` | Lista láminas faltantes de un equipo |
| `mis repetidas` | Lista todas tus láminas extras |
| `¿cómo voy?` | Resumen general del progreso |
| `marca como que no las tengo: ARG3, COL7` | Quita láminas (las cambiaste o perdiste) |

Después de cada actualización, Claude hace `git push` automáticamente y el dashboard se actualiza en ~30 segundos.

---

## Modo Trading 📲

En el dashboard hay un botón **"Modo Trading"** — ideal para intercambios en vivo:
- Pantalla completa, input grande
- Escribe o dicta un código (`ARG7`, `Colombia 7`, `Argentina 7`)
- Muestra al instante si la tienes ✅ o te falta ❌, con foto de la lámina
- Enter limpia el campo para la siguiente consulta

---

## Sistema de IDs

| Formato | Ejemplo | Descripción |
|---|---|---|
| `XX1` | `ARG1` | Escudo (foil) |
| `XX2` | `ARG2` | Foto del equipo |
| `XX3–XX20` | `ARG3` a `ARG20` | Jugadores 1–18 |
| `FWC1–FWC20` | `FWC1` | Láminas especiales |

48 equipos × 20 láminas + 20 especiales FWC = **980 láminas en total**

---

## Requisitos

- Node.js 18+
- Cuenta GitHub
- Suscripción a Claude (plan Pro o Max recomendado)
