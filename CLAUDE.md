# Álbum Panini FIFA World Cup 2026 — Instrucciones para Claude

## Archivos
- `stickers.json` — base de datos del estado de la colección
- `index.html` — dashboard visual en GitHub Pages

## Estructura de stickers.json
```
stickers["ARG7"] = {
  "team": "Argentina",
  "group": "J",
  "name": "Jugador 5",
  "type": "base" | "foil",
  "owned": true | false,
  "duplicates": 0   ← número de EXTRAS (si duplicates=2, tienes 3 en total)
}
```

## Formato de IDs
- Equipos: 2–3 letras + número 1–20 (ej: ARG7, COL1, MEX20)
  - X1 = Escudo (foil), X2 = Foto equipo, X3–X20 = Jugadores 1–18
- Especiales: FWC1–FWC20
- IDs no son case-sensitive → normalizar a mayúsculas (col1 → COL1)

---

## COMANDOS

### "valida estas [lista]"
Verificar si cada lámina está o no en la colección. NO modifica nada.

**Respuesta por lámina:**
- ✅ ARG7 — ya la tienes (Jugador 5 · Argentina)
- ❌ COL3 — te falta (Jugador 1 · Colombia)

**Respuesta compacta si son muchas (>5):**
```
✅ Tienes: ARG7, BRA3, MEX1
❌ Te faltan: COL3, ESP8, FRA2
```

---

### "completa estas [lista]"
Marcar láminas. Regla doble:
- Si `owned = false` → poner `owned = true` ✅
- Si `owned = true` → lámina repetida: incrementar `duplicates` en 1 🔁

**Respuesta:**
```
✅ Nuevas (3): ARG7, BRA3, COL1
🔁 Repetidas (2): MEX1 (ahora tienes 2), ARG1 (ahora tienes 2)
Total colección: 47/980 (4.8%)
```

Después de este comando: actualizar metadata, hacer git commit + push.

---

### "¿qué me falta de [equipo]?"
Listar stickers del equipo con owned=false.
Ejemplo: "¿qué me falta de Colombia?" → listar todos los COL con owned=false

### "¿qué tengo de [equipo]?"
Listar stickers del equipo con owned=true.

### "mis repetidas" / "¿qué tengo para cambiar?"
Listar todos los stickers con duplicates > 0, agrupados por equipo.

### "resumen" / "¿cómo voy?"
Mostrar progreso general y por grupo.

---

## Después de CUALQUIER modificación:
1. Leer stickers.json con Read tool
2. Aplicar cambios
3. Actualizar `metadata.lastUpdated` (fecha de hoy) y `metadata.collected` (count owned=true)
4. Escribir con Write tool
5. Ejecutar:
   ```bash
   cd /Users/jd/Sites/panini && git add stickers.json && git commit -m "update láminas $(date +%Y-%m-%d)" && git push
   ```
6. Decirle al usuario que el dashboard se actualiza en ~30 seg

## Reglas generales
- Responder SIEMPRE en español
- Ser MUY conciso — el usuario puede estar en medio de un intercambio
- Si un ID no existe en el JSON: "⚠️ No reconozco el código XXX"
- NUNCA revertir láminas ya obtenidas sin confirmación explícita
