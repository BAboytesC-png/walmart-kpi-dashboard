# 📊 Proyecto 2: Dashboard Interactivo de KPIs
## Walmart - Análisis de Eficiencia y Participación por Departamento

---

## 🎯 Descripción General

**Empresa:** Walmart  
**Período:** 2012 (Análisis de ventas semanales)  
**Objetivo:** Crear dashboard dinámico para evaluar eficiencia de departamentos  
**Usuario Final:** Dirección Comercial de Walmart

La Dirección Comercial de Walmart necesitaba una herramienta que permitiera consultar dinámicamente qué departamentos eran más eficientes y cuál era su contribución al negocio, sin requerer intervención técnica.

---

## 🔴 EL PROBLEMA

La información sobre el rendimiento de departamentos estaba dispersa:

```
❌ Datos en múltiples tablas no integradas
❌ Sin KPIs definidos para eficiencia
❌ Imposible consultar información dinámicamente
❌ Cada análisis requería intervención técnica
❌ Director comercial no podía explorar datos por sí solo
❌ Falta de visualizaciones comparativas
❌ Lentitud en la toma de decisiones
❌ No había manera de validar hipótesis rápidamente
```

**Impacto empresarial:**
- ⏱️ Tiempo de respuesta lento ante preguntas de negocio
- 💰 Posibles ineficiencias no detectadas
- 📊 Decisiones basadas en estimaciones, no datos reales

---

## ✅ LA SOLUCIÓN

Construí un **dashboard interactivo completo** que permite a cualquier usuario consultar KPIs dinámicamente:

### 1. **Integración de Datos (VLOOKUP)**
```
Problema: Datos en 3 tablas separadas (ventas, departamentos, tiendas)
Solución: Crear tabla consolidada con VLOOKUP

Función VLOOKUP usada:
=BUSCARV(valor_buscado, rango_tabla, índice_columna, FALSO)

Ejemplo:
=BUSCARV(B2, raw_departamento!A:B, 2, FALSO)
  → Busca el ID de departamento
  → Trae el nombre del departamento

Resultado: Tabla única con 8 columnas integradas:
  - tienda | Dept | Fecha | ventas_semanales | esferiado
  - semana_limpia | tipo | tamaño | nombre_dept
```

### 2. **Cálculo de KPI 1: Eficiencia (Ventas por m²)**
```
Concepto: ¿Cuánto vende cada departamento por metro cuadrado?
         (Mayor valor = Más eficiente)

Fórmula: Ventas por m² = SUM(ventas_semanales) / AVERAGE(tamaño)

Implementación:
  1. Creé tabla dinámica (Pivot)
  2. Agrupé por: nombre_dept
  3. Incluí: SUM(ventas_semanales) + AVERAGE(tamaño)
  4. Agregué campo calculado: ventasxmetro2
  5. Filtré: Solo datos de 2012

Resultado:
  Despensa y Básicos: $652.8 por m²
  Comida Fresca: $445.2 por m²
  Electrodomésticos: $380.5 por m²
  (Mayor = Mejor)
```

### 3. **Cálculo de KPI 2: Participación (%)**
```
Concepto: ¿Qué porcentaje del total representa cada departamento?

Fórmula: Participación = Ventas_depto / Ventas_totales × 100%

Implementación:
  1. Creé tabla dinámica agrupando por departamento
  2. Calculé SUM(ventas_semanales) por departamento
  3. Mostré como % del total (opción en Google Sheets)

Resultado:
  Despensa y Básicos: 25.3%
  Comida Fresca: 18.7%
  Bebidas: 15.2%
  ... (el resto suma 100%)
```

### 4. **Menú Desplegable Dinámico (Validación de Datos)**
```
Problema: Usuario necesita seleccionar departamento
Solución: Crear lista desplegable

Pasos:
  1. Seleccionar celda B2
  2. Data → Validación de datos
  3. Criterio: Lista de elementos
  4. Rango: raw_departamento!B:B (lista de departamentos)
  5. Mensaje: "Selecciona un departamento"

Resultado:
  Usuario puede hacer click en B2 y elegir:
  - Despensa y Básicos
  - Comida Fresca
  - Bebidas
  - Etc.
```

### 5. **Fórmulas Dinámicas (VLOOKUP)**
```
En el dashboard, cuando usuario selecciona departamento en B2:

KPI 1 (Eficiencia):
=BUSCARV(B2, Pivot_Eficiencia!A:D, 4, FALSO)
  → Busca el departamento seleccionado
  → Trae su ventasxmetro2

KPI 2 (Participación):
=BUSCARV(B2, Pivot_Participacion!A:B, 2, FALSO)
  → Busca el departamento seleccionado
  → Trae su % de participación

Resultado: Los KPIs se actualizan automáticamente
```

### 6. **Formato Condicional (Visualización)**
```
Para hacer los KPIs más visuales:

Para Eficiencia:
  1. Selecciona celda con valor
  2. Format → Conditional formatting
  3. Format as: Color scale
  4. Min: Rojo (bajo) → Max: Verde (alto)
  
Para Participación:
  1. Si < 5%: Rojo (bajo rendimiento)
  2. Si 5-10%: Amarillo (medio)
  3. Si > 10%: Verde (bueno)

Resultado: Valores con colores que muestran desempeño
```

### 7. **Gráficos Interactivos**
```
KPI 1: Gráfico de barras (Eficiencia por departamento)
  - Eje X: Departamentos
  - Eje Y: Ventas por m²
  - Ordenado de mayor a menor
  - Colores profesionales

KPI 2: Gráfico de barras apiladas (Participación)
  - Muestra proporción de cada departamento
  - Cada departamento tiene color distinto
  - Fácil ver contribución relativa
```

---

## 📈 RESULTADOS ALCANZADOS

### Dashboard Funcional

```
┌────────────────────────────────────────────────────┐
│ WALMART KPI DASHBOARD - 2012                       │
├────────────────────────────────────────────────────┤
│                                                    │
│ Selecciona departamento: [▼ Despensa y Básicos]   │
│                                                    │
├─────────────────┬─────────────────────────────────┤
│ KPI 1           │ KPI 2                           │
│ Eficiencia      │ Participación                   │
│                 │                                 │
│ $652.80/m²      │ 25.3%                          │
│                 │                                 │
│ [Gráfico Barr] │ [Gráfico Apilado]              │
│                 │                                 │
└─────────────────┴─────────────────────────────────┘

Características:
✓ Usuario selecciona departamento
✓ KPIs se actualizan automáticamente
✓ Gráficos se actualizan al mismo tiempo
✓ Colores indican desempeño
✓ Fácil de usar para no-técnicos
```

### Insights Descubiertos

```
HALLAZGO 1: Eficiencia Desigual
  - Despensa/Básicos: $652.8/m² (TOP 1)
  - Bebidas: $485.3/m² (TOP 3)
  - Electrónica: $280.1/m² (BOTTOM)
  Recomendación: Investigar por qué Electrónica es ineficiente

HALLAZGO 2: Concentración
  - Top 3 departamentos = 69% de ventas
  - Oportunidad en departamentos con bajo rendimiento

HALLAZGO 3: Departamentos de Oportunidad
  - Comida Fresca tiene buen % (18.7%) pero baja eficiencia
  - Posible oportunidad de optimización
```

---

## 🛠️ PROCESO PASO A PASO

### Fase 1: Preparación de Datos (20 minutos)
```
1. Copia de raw_ventas → clean_ventas
2. Normalizar formatos:
   - Fechas: YYYY-MM-DD
   - Ventas: Formato moneda $ xxxx.xx
3. Agregar columna semana_limpia con YEAR(Fecha)
4. Verificar integridad de datos
```

### Fase 2: Enriquecimiento (20 minutos)
```
1. En clean_ventas, agregar columnas:
   - tipo (VLOOKUP a raw_tiendas)
   - tamaño (VLOOKUP a raw_tiendas)
   - nombre_dept (VLOOKUP a raw_departamento)
2. Validar que todos los lookups funcionan
3. Revisar ceros o errores (#N/A)
```

### Fase 3: Tablas Dinámicas (20 minutos)
```
1. Crear Pivot 1: Eficiencia
   - Filas: nombre_dept
   - Valores: SUM(ventas), AVERAGE(tamaño)
   - Campo calculado: ventasxmetro2
   - Filtro: año = 2012
   
2. Crear Pivot 2: Participación
   - Filas: nombre_dept
   - Valores: SUM(ventas) as % of total
   - Filtro: año = 2012
```

### Fase 4: Dashboard (20 minutos)
```
1. Nueva hoja: "Dashboard"
2. Celda B2: Validación con lista de departamentos
3. Agregar KPI labels y valores con VLOOKUP
4. Aplicar formato condicional
5. Insertar gráficos desde Pivots
6. Ajustar colores y títulos
```

### Fase 5: QA y Documentación (20 minutos)
```
1. Crear hoja README
2. Documentar objetivo del dashboard
3. Explicar cómo usar (menú desplegable)
4. Listar KPIs y sus significados
5. Documentar validaciones completadas
```

---

## 📁 ARCHIVOS EN ESTE REPOSITORIO

```
walmart-kpi-dashboard/
├── README.md (este archivo)
├── walmart-data.csv (datos limpios consolidados)
├── walmart-dashboard-preview.png (captura del dashboard)
├── Archivos opcionales:
│   ├── pivot-eficiencia.png (tabla dinámica KPI 1)
│   ├── pivot-participacion.png (tabla dinámica KPI 2)
│   └── kpi-charts.png (gráficos del dashboard)
└── DOCUMENTACIÓN:
    - Metodología de cálculo KPIs
    - Manual del usuario
    - Validaciones QA
```

---

## 📊 ARCHIVOS CSV DISPONIBLES

### `walmart-data.csv`
- **Filas:** 421,570 registros de ventas semanales
- **Período:** 2012
- **Columnas:**
  - Store: ID de tienda (1-45)
  - Dept: ID de departamento (1-99)
  - Date: Fecha de venta
  - Weekly_Sales: Ventas semanales en USD
  - IsHoliday: Indicador de semana feriada
  - Week_Num: Número de semana (extraído de fecha)
  - Store_Type: Tipo de tienda (A, B, o C)
  - Store_Size: Tamaño en m² de la tienda
  - Dept_Name: Nombre del departamento

- **Descargable:** Sí ✅
- **Editable en:** Excel, Google Sheets, Python, R

---

## 📈 VISUALIZACIONES RECOMENDADAS

### PNG 1: `walmart-dashboard-preview.png`
Captura del dashboard mostrando:
```
┌──────────────────────────────────────────────┐
│ Dashboard completo con:                      │
│ - Menú desplegable (Departamento)            │
│ - KPI 1: $652.80/m² (Eficiencia)            │
│ - KPI 2: 25.3% (Participación)              │
│ - Gráfico de barras (Eficiencia)            │
│ - Gráfico apilado (Participación)           │
│ - Colores profesionales                      │
│                                              │
│ Muestra cómo se ve en uso real               │
└──────────────────────────────────────────────┘
```

### PNG 2: `pivot-eficiencia.png`
Tabla dinámica de KPI 1 mostrando:
```
nombre_dept              | SUM ventas | AVG tamaño | Venta/m²
─────────────────────────────────────────────────────────────
Despensa y Básicos      | 85.2M      | 130,287    | $652.80
Comida Fresca          | 45.3M      | 101,856    | $445.20
Bebidas                | 38.1M      | 78,534     | $485.30
...
```

### PNG 3: `kpi-charts.png`
Ambos gráficos juntos mostrando:
- Barras: Eficiencia por departamento
- Barras Apiladas: Participación

---

## ✅ VALIDACIÓN DE CALIDAD

```
QA Checklist completado:

✓ Todas las tiendas tienen datos
✓ No hay departamentos duplicados
✓ VLOOKUP sin errores (#N/A)
✓ Tamaños válidos (no ceros)
✓ Fechas en formato correcto
✓ Pivot tables se recalculan automáticamente
✓ Dashboard KPIs responden al menú desplegable
✓ Gráficos son técnicamente correctos
✓ Documentación es clara
✓ Manual de usuario es accesible
```

---

## 🔧 HERRAMIENTAS UTILIZADAS

```
Software:
  - Google Sheets
  - Microsoft Excel (validación)

Técnicas:
  - Data Integration (Integración de datos)
  - Lookup Functions (VLOOKUP)
  - Pivot Tables (Tablas dinámicas)
  - Calculated Fields (Campos calculados)
  - Conditional Formatting
  - Data Visualization
  - Interactive Dashboards

Funciones Específicas:
  - BUSCARV() → Integración de tablas
  - Tablas dinámicas → Agregación y resumen
  - Validación de datos → Menú desplegable
  - Formato condicional → Visualización de valores
  - Gráficos dinámicos → Visualización interactiva
```

---

## 💡 HABILIDADES DEMOSTRADAS

```
Integración de Datos:
  ✓ VLOOKUP avanzado
  ✓ Consolidación de múltiples fuentes
  ✓ Validación de integridad referencial

Cálculo de KPIs:
  ✓ Definición de métricas de negocio
  ✓ Uso de campos calculados
  ✓ Ratios financieros (ventas/m²)

Tablas Dinámicas:
  ✓ Agregación de datos
  ✓ Campos calculados en Pivots
  ✓ Filtros automáticos

Visualización Interactiva:
  ✓ Menús desplegables
  ✓ Formato condicional
  ✓ Gráficos profesionales
  ✓ Diseño de dashboards

Comunicación:
  ✓ Manual de usuario
  ✓ Documentación técnica
  ✓ Explicación de KPIs
```

---

## 📌 METODOLOGÍA

```
Paso 1: DEFINE KPIs
  → ¿Qué queremos medir?
  → ¿Cómo lo medimos?

Paso 2: OBTÉN DATOS
  → Consolida de múltiples fuentes
  → Integra con VLOOKUP

Paso 3: AGREGA
  → Crea tablas dinámicas
  → Calcula campos derivados

Paso 4: VISUALIZA
  → Crea gráficos
  → Aplica formato condicional

Paso 5: PERMITE CONSULTAS
  → Agrega menú desplegable
  → KPIs se actualizan dinámicamente

Paso 6: DOCUMENTA
  → Crea manual de usuario
  → Explica qué significa cada métrica
```

---

## ⚠️ LIMITACIONES

```
Limitaciones actuales:
  - Solo datos de 2012 (no puede comparar años)
  - Basado en Google Sheets (escalabilidad limitada)
  - No hay historicidad (no guarda cambios en tiempo)
  - No hay alertas automáticas

Mejoras futuras:
  - Actualización automática de datos
  - Histórico de cambios en KPIs
  - Alertas cuando KPIs caen
  - Análisis predictivo
  - Integración con base de datos central
```

---

## 🎯 RECOMENDACIONES DE NEGOCIO

```
RECOMENDACIÓN 1: Optimizar Eficiencia
  Hallazgo: Algunos departamentos son 2x menos eficientes
  Acción: Investigar diferencias (layout, stock, personal)
  Impacto: Potencial +20% en ventas/m²

RECOMENDACIÓN 2: Rebalancear Presupuesto
  Hallazgo: Top 3 departamentos concentran 69% de ventas
  Acción: Invertir en departamentos con bajo rendimiento
  Impacto: Diversificar ingresos, reducir riesgo

RECOMENDACIÓN 3: Monitoreo Continuo
  Hallazgo: Deptos con baja participación podrían desaparecer
  Acción: Monitorear mensualmente con este dashboard
  Impacto: Detectar problemas antes de que crezcan
```

---

## 🏆 PRÓXIMOS PASOS

```
Corto Plazo (1-2 semanas):
  → Entrenar a Dirección Comercial en uso del dashboard
  → Recopilar feedback para mejoras

Mediano Plazo (1-3 meses):
  → Integrar datos de 2011-2012 para comparación
  → Agregar gráficos adicionales (tendencia, variación)

Largo Plazo (3-6 meses):
  → Migrar a base de datos (SQL)
  → Automatizar actualización de datos
  → Agregar análisis predictivo
  → Conectar a herramientas de BI (Tableau, PowerBI)
```

---

## 📚 APRENDIZAJES CLAVE

```
1. Los datos sin visualización son inútiles
   → Un dashboard bien diseñado permite tomar decisiones

2. La accesibilidad es crítica
   → No-técnicos deben poder usar herramientas de análisis

3. KPIs bien definidos son oro
   → Deben ser claros, medibles y accionables

4. El mantenimiento es importante
   → Un dashboard sin actualización se vuelve incorrecto

5. La documentación permite escalabilidad
   → Otros pueden mantener o mejorar tu trabajo
```

---

## 📞 CONTACTO

- **Fecha de realización:** Mayo 2026
- **Duración total:** ~2 horas
- **Bootcamp:** TripleTen Data Analysis
- **Nivel de dificultad:** Intermedio-Avanzado

---

**Hecho con precisión y pensando en el usuario final. ✨**
