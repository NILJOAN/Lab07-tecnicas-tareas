# Tarea: Mi prompt avanzado

## Tarea elegida

Generar casos de prueba para un formulario de registro de usuarios (correo, contrasena y confirmacion de contrasena). Es una tarea util en desarrollo de software porque una buena lista de casos evita errores antes de entregar el sistema.

## Version 1: prompt basico

```text
Dame casos de prueba para un registro de usuarios.
```

- **Tecnica agregada:** ninguna (punto de partida).
- **Por que:** para ver como responde la IA sin ayuda.
- **Que paso en la respuesta:** (completa: ej. lista general, sin formato fijo, pocos casos limite)

## Version 2

```text
<rol>Actua como analista de pruebas de software (QA) que trabaja en una tienda en linea.</rol>
<contexto>Formulario de registro web con correo, contrasena (minimo 8 caracteres, al menos un numero) y confirmacion de contrasena.</contexto>
<tarea>Escribe 8 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado.</formato>
```

- **Tecnicas agregadas:** role prompting + prompt estructurado.
- **Por que:** v1 era generica y desordenada; el rol fija el punto de vista (QA) y las etiquetas separan contexto, tarea y formato.
- **Que mejoro:** (completa: ej. ahora hay tabla con 4 columnas y los casos son mas tecnicos)

## Version 3: prompt final

```text
<rol>Actua como analista de pruebas de software (QA) que trabaja en una tienda en linea y explica a un desarrollador junior.</rol>
<contexto>Formulario de registro web con correo, contrasena (minimo 8 caracteres, al menos un numero) y confirmacion de contrasena.</contexto>
<ejemplo>
| ID | Escenario | Datos de entrada | Resultado esperado |
|----|-----------|------------------|--------------------|
| CP-01 | Registro valido | correo: ana@mail.com, clave: Clave1234, confirmacion: Clave1234 | Cuenta creada |
</ejemplo>
<tarea>Piensa paso a paso que puede fallar (campos vacios, formato de correo, longitud, coincidencia de contrasenas) y escribe 8 casos de prueba. Despues revisa tu propia tabla: indica si falta algun caso limite, agrega los que falten y marca cuales agregaste.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado. Responde en espanol.</formato>
```

- **Tecnicas agregadas:** few-shot (un ejemplo del formato), chain of thought y autocritica.
- **Por que:** el ejemplo hace que todas las filas tengan el mismo formato; pensar paso a paso ayuda a cubrir mas fallas; la autocritica obliga a revisar casos limite.
- **Que mejoro:** (completa: ej. formato identico en todas las filas, aparecieron casos limite y se indicaron los agregados)

## Tecnicas usadas en el prompt final

| Parte del prompt | Tecnica |
|------------------|---------|
| `<rol>` ... QA que explica a un desarrollador junior | Role prompting |
| Etiquetas `<rol>`, `<contexto>`, `<tarea>`, `<formato>` | Prompt estructurado |
| `<ejemplo>` con la fila CP-01 | Few-shot (one-shot) |
| "Piensa paso a paso que puede fallar" | Chain of thought |
| "Despues revisa tu propia tabla... agrega los que falten" | Autocritica |

## Evaluacion del resultado

| Criterio | Cumple (Si / No) |
|----------|------------------|
| Usa el rol especifico pedido (QA) | (Si/No) |
| La tabla tiene las 4 columnas pedidas | (Si/No) |
| Todas las filas siguen el formato del ejemplo | (Si/No) |
| Incluye casos con campos vacios y correo sin @ | (Si/No) |
| Indica que casos agrego en la autocritica | (Si/No) |
| No hay casos repetidos ni sin sentido | (Si/No) |

## Por que elegi estas tecnicas

Elegi role prompting y prompt estructurado porque la tarea necesita un punto de vista tecnico (QA) y varias partes que no se mezclen: contexto, reglas de la contrasena y formato de salida. Use few-shot porque quiero que todos los casos tengan exactamente el mismo formato de tabla, de modo que pueda copiarlos a una hoja de calculo sin corregirlos. Agregue chain of thought para que la IA piense primero que puede fallar y cubra mas escenarios, y autocritica para que revise casos limite que suelen olvidarse. No use descomposicion porque la tarea es pequena y cabe en un solo pedido; dividirla en varios mensajes solo la habria hecho mas lenta.