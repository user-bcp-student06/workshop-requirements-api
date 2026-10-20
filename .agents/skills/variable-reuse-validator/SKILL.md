---
name: variable-reuse-validator
description: Analiza variables duplicadas o reasignadas en codigo, comprueba si su reutilizacion es intencional y reporta evidencia por ambito. Usar al revisar nombres repetidos, variables sobrescritas o posible reutilizacion accidental.
---

# Validador de reutilizacion de variables

## Objetivo

Encontrar variables asignadas mas de una vez y determinar, con el contexto de su ambito y uso, si la reutilizacion es esperada o puede causar errores. Una asignacion repetida es una senal para revisar, no un defecto por si sola.

## Procedimiento

1. Confirma el lenguaje y limita el analisis a los archivos solicitados.
2. Para Python en este proyecto, ejecuta desde `workshop-requirements-api`:

   ```bash
   python -m app.variable_reuse <archivo-o-directorio>
   ```

   El comando devuelve `1` si encuentra reasignaciones y `0` si no encuentra ninguna. Trata `1` como resultado del analisis, no como una falla de ejecucion.
3. Si el codigo no es Python, usa el parser, analizador o linter correspondiente al lenguaje. No apliques el analizador Python ni deduzcas ambitos con busquedas de texto o expresiones regulares.
4. Para cada resultado, revisa la declaracion/asignacion inicial, las asignaciones posteriores, las lecturas entre ellas y el ambito real. Considera parametros, bloques, cierres y reglas del lenguaje.
5. Clasifica cada caso como:
   - **Intencional**: por ejemplo, un acumulador, un estado que evoluciona o una conversion claramente secuencial.
   - **Sospechoso**: la asignacion anterior deja de ser necesaria, se pierde un valor que se usa despues, o el nombre oculta conceptos diferentes.
   - **No concluyente**: falta contexto o pruebas para decidir.
6. Reporta solo casos con evidencia. No cambies codigo salvo que el usuario tambien solicite la correccion.

## Reglas de analisis

- Compara asignaciones dentro del mismo ambito; el mismo nombre en funciones independientes no es duplicacion por si mismo.
- Cuenta los parametros como vinculaciones iniciales cuando el lenguaje los trate como variables locales.
- Distingue una nueva declaracion de una reasignacion segun las reglas del lenguaje.
- No marques automaticamente como defecto los acumuladores, contadores ni variables de estado.
- Si el analizador disponible no cubre una construccion o ambito, declara esa limitacion y valida el caso con una herramienta adecuada.
- No concluyas que existe un error solo por encontrar el mismo identificador varias veces.

## Formato de salida

```markdown
### Reutilizacion de variables
- Archivo y lineas:
- Ambito:
- Variable:
- Asignaciones observadas:
- Clasificacion: Intencional | Sospechoso | No concluyente
- Evidencia y posible impacto:
- Validacion recomendada:
```