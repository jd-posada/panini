# Álbum Panini FIFA World Cup 2026 — Instrucciones para Claude

## Qué es esto
Base de datos del álbum Panini Mundial 2026 (980 láminas).
El archivo `stickers.json` guarda el estado de la colección.
El `index.html` es el dashboard visual — se actualiza en GitHub Pages con cada push.

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
- Equipos: `COD` = 3 letras + número 1–20
  - `X1` = Escudo (foil), `X2` = Foto equipo, `X3`–`X20` = Jugadores 1–18
- Especiales: `FWC1`–`FWC20`

## Comandos que el usuario puede decirte

### Marcar láminas nuevas como obtenidas
**El usuario dice:** "tengo ARG7, ARG12, BRA3" / "conseguí el MEX5 y MEX8" / "me dieron COL1, COL2, COL3"
**Tú haces:** poner `owned: true` en cada ID mencionado. Si ya era `true`, no cambiar nada.
**Responde:** "✅ Marcadas como obtenidas: ARG7, ARG12, BRA3. Total: X/980 (X.X%)"

### Verificar si tiene una lámina (durante un intercambio)
**El usuario dice:** "¿tengo ARG7?" / "ARG7?" / "¿me falta el COL5?"
**Tú haces:** revisar `owned` en stickers.json
**Responde:** "✅ Sí, tienes ARG7 (Jugador 5 · Argentina)" O "❌ No, te falta ARG7"

### Consulta rápida de varios números (durante intercambio)
**El usuario dice:** "ARG7, BRA3, COL8 — ¿cuáles me faltan?"
**Tú haces:** verificar cada uno
**Responde:** lista clara de cuáles tiene y cuáles faltan

### Marcar repetidas
**El usuario dice:** "repetidas: ARG7, MEX3" / "tengo de más el COL5" / "ARG7 está repetida"
**Tú haces:** incrementar `duplicates` en 1 para cada ID
**Responde:** "🔁 Marcadas como repetidas: ARG7 (ahora tienes 2), MEX3 (ahora tienes 2)"

### Ver qué falta de un equipo
**El usuario dice:** "¿qué me falta de Colombia?" / "¿qué tengo de Argentina?"
**Tú haces:** listar stickers de COL/ARG con owned=false (o true)
**Responde:** lista numerada con IDs y nombres

### Ver repetidas disponibles para intercambio
**El usuario dice:** "¿qué tengo para cambiar?" / "mis repetidas"
**Tú haces:** listar todos los stickers con duplicates > 0
**Responde:** lista agrupada por equipo

### Estadísticas
**El usuario dice:** "¿cómo voy?" / "resumen" / "progreso"
**Tú haces:** calcular totales
**Responde:** resumen completo con progreso por grupo

## Después de CUALQUIER modificación al archivo:
1. Leer stickers.json completo con Read tool
2. Hacer los cambios
3. Actualizar `metadata.lastUpdated` con la fecha de hoy
4. Recalcular `metadata.collected` = count de stickers con owned=true
5. Escribir con Write tool
6. Ejecutar:
   ```bash
   cd /Users/jd/Sites/panini && git add stickers.json && git commit -m "update: láminas [FECHA]" && git push
   ```
7. Confirmar al usuario que el dashboard se actualizará en ~30 seg

## Reglas importantes
- Responder SIEMPRE en español
- Ser MUY conciso durante intercambios (el usuario necesita respuestas rápidas)
- Para consultas de sí/no durante trading: responder solo "✅ Sí, tienes X" o "❌ No, falta X"
- Si el usuario da un ID inválido, avisarle: "⚠️ No encontré el código XXX"
- NUNCA borrar láminas marcadas como owned sin confirmación explícita
- Los IDs no son case-sensitive — normalizar a mayúsculas (col1 → COL1)
