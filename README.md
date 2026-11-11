# 📊 CFO Dashboard

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=flat&logo=googlesheets&logoColor=white)](https://www.google.com/sheets/about/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.0-blue.svg)]()

---

## 📋 Tabla de Contenidos

- [Descripción General](#-descripción-general)
- [Características Principales](#-características-principales)
- [Contenido del Proyecto](#-contenido-del-proyecto)
- [Herramientas y Tecnologías](#️-herramientas-y-tecnologías-utilizadas)
- [Configuración del Entorno](#️-configuración-del-entorno)
- [Estructura del Repositorio](#-estructura-del-repositorio)
- [Medidas DAX Implementadas](#-medidas-dax-implementadas)
- [Vistas Previas](#️-vistas-previas-del-proyecto)
- [Autor](#-autor)
- [Licencia](#-licencia)

---

## 📃 Descripción General

Dashboard ejecutivo para **Directores Financieros (CFO)** desarrollado en Power BI, diseñado para centralizar y analizar el estado financiero de una cadena de restaurantes en tiempo real. Este proyecto se conecta a **Google Sheets** como fuente de datos principal y transforma datos contables en información estratégica mediante:

- 📊 **3 Vistas Analíticas:** Overview, P&C Deep Dive y P&L (Estado de Resultados).
- 💰 **Estado de Resultados:** Ingresos, COGS, gastos operativos, EBIT, EBITDA y utilidad neta.
- 📈 **+30 Medidas DAX:** KPIs financieros avanzados (márgenes, costos, punto de equilibrio, análisis YoY).
- 🎯 **Metas vs Real:** Comparativa de márgenes objetivo vs. alcanzados (COGS Target, Operating Margin Target).
- 📅 **Análisis Temporal:** Tendencias mensuales y comparativas Year over Year (YoY).

---

## ✨ Características Principales

### 🔄 Transformaciones en Power Query
- Procesos de limpieza y modelado de datos para optimizar el rendimiento.
- Creación de tabla calendario personalizada para análisis temporal.
- Conexión directa y optimizada con hojas de cálculo de Google Sheets.

### 📐 Modelo de Datos
- **Tablas de Dimensión:** DimCenterCost, DimDocument, DimEntity, DimProduct, DimSubcategory, DimBranch.
- **Tablas de Hechos:** Fact-Revenue (Ingresos) y Fact-Expenses (Egresos) con información transaccional detallada.
- Relaciones optimizadas (Star Schema) para análisis multidimensional eficiente.

### 🎨 Diseño Interactivo
- Navegación por páginas con paginado intuitivo y marcadores.
- Segmentación de datos dinámica por sucursal y categoría.
- Tarjetas KPI con diseño moderno, gradientes e iconos dinámicos (UDF `SelectedImg`).
- Visualizaciones personalizadas con HTML + DAX para tarjetas de resumen.

---

## 📊 Contenido del Proyecto

| Página | Descripción | Contenido Principal |
|--------|-------------|---------------------|
| **Overview** | Vista general ejecutiva | KPIs globales, estado financiero consolidado, alertas de márgenes. |
| **P&C Deep Dive** | Análisis por sucursal | Desglose de ingresos y gastos por cada local de la cadena de restaurantes. |
| **P&L** | Estado de Resultados | Vista detallada de Pérdidas y Ganancias: Ingresos, COGS, Opex, EBIT, EBITDA y Utilidad Neta. |

---

## 🛠️ Herramientas y Tecnologías Utilizadas

### Software Requerido
- **Power BI Desktop** (versión más reciente)

### Fuente de Datos
- 📄 **Google Sheets** (Conectado directamente o mediante exportación de: DimCentroCosto, DimDocumento, DimEntidad, DimProducto, DimSubcategoria, DimSucursal, Fact_Egresos, Fact_Ingresos)

### Lenguajes y Tecnologías
- **DAX** - Cálculo de medidas y KPIs financieros (+30 fórmulas avanzadas).
- **Power Query (Lenguaje M)** - Transformación, limpieza de datos y conexión a Google Sheets.
- **UDF (User Defined Functions)** - Funciones personalizadas para reutilización de lógica (ej. `FX_OVEROPERATINGREVENUE`, `FX_EXPENSECATEGORY`).

---

## ⚙️ Configuración del Entorno

### Prerequisitos
1. Descargar e instalar [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
2. Tener acceso a las hojas de cálculo de Google Sheets con los datos financieros (o usar los archivos `.xlsx` proporcionados en la carpeta `Data Source`).

### Paso 1: Configurar la Conexión a Google Sheets
1. Abrir la hoja de cálculo en Google Sheets.
2. Ir a **Archivo > Compartir > Publicar en la web**.
3. Seleccionar la hoja específica y el formato **Valores separados por comas (.csv)** (o mantener la integración directa con el conector nativo de Google Sheets de Power BI si está disponible en tu versión).
4. Copiar el enlace generado.

### Paso 2: Conectar Power BI a Google Sheets
1. Abrir Power BI Desktop.
2. Seleccionar **Obtener datos > En línea > Web** (o el conector nativo de Google Sheets).
3. Pegar la URL de la hoja publicada o autenticar con la cuenta de Google.
4. Repetir el proceso para cada una de las tablas del modelo de datos:
   - `DimCentroCosto`
   - `DimDocumento`
   - `DimEntidad`
   - `DimProducto`
   - `DimSubcategoria`
   - `DimSucursal`
   - `Fact_Egresos`
   - `Fact_Ingresos`

### Paso 3: Cargar y Transformar
1. Hacer clic en **Transformar datos** para abrir Power Query.
2. Asegurar que los tipos de datos sean correctos (fechas, decimales, texto).
3. Crear la tabla `Calendar` (Calendario) a partir de las fechas de las tablas de hechos.
4. Cerrar y aplicar para cargar el modelo de datos.

---

## 📂 Estructura del Repositorio

```text
.
├── Data Source/                      # Contiene las tablas fuente en formato .xlsx
│   ├── DimCentroCosto.xlsx
│   ├── DimDocumento.xlsx
│   ├── DimEntidad.xlsx
│   ├── DimProducto.xlsx
│   ├── DimSubcategoria.xlsx
│   ├── DimSucursal.xlsx
│   ├── Fact_Egresos.xlsx
│   └── Fact_Ingresos.xlsx
├── Images/                           # Carpeta con los íconos y recursos visuales
├── Chief Financial Officer.Report/   # Componentes del reporte (formato .pbip)
├── Chief Financial Officer.SemanticModel/ # Modelo semántico
├── Chief Financial Officer.pbip      # Proyecto Power BI (formato nuevo)
├── Chief Financial Officer.pbix      # Archivo ejecutable de Power BI
├── Udf.txt                           # Contiene las funciones definidas por el usuario (UDF)
├── .gitignore                        # Archivo de configuración Git
└── README.md                         # Este archivo de documentación
```

## 🧮 Medidas DAX Implementadas
El proyecto cuenta con más de 30 medidas DAX organizadas por categorías financieras. A continuación, se detallan las principales:

#### 💰 Ingresos y Gastos

<details>
<summary>Ver medidas de Ingresos y Gastos</summary>

```dax
#Revenue = SUMX(Fact_Revenue, Fact_Revenue[Quantity] * RELATED(DimProducts[Price_Unit]))
#Expenses = SUMX(Fact_Expenses, Fact_Expenses[Subtotal])
#Operating Revenue = FX_REVENUECATEGORY({"Revenue"})
#Cost of Goods Sold = FX_EXPENSESUBCATEGORY({"Supplies and Materials", "Packaging", "Beverages"})
#Store Operating Expenses = FX_EXPENSECATEGORY({"Store Operating Expenses"})
#Selling Expenses = FX_EXPENSECATEGORY({"Selling Expenses"})
#Administrative Expenses = FX_EXPENSECATEGORY({"Administrative Expenses"})
#Other Income = FX_REVENUECATEGORY({"Other Income"})
#Financial Expenses = FX_EXPENSECATEGORY({"Financial Expenses"})
```
</details>

#### 📈 Rentabilidad y Márgenes
<details>
<summary>Ver medidas de Rentabilidad</summary>

```dax
#Gross Profit = FX_REVENUECATEGORY({"Revenue"}) - FX_EXPENSECATEGORY({"Cost of Goods Sold"})
#Gross Profit Margin(%) = FX_OVEROPERATINGREVENUE([#Gross Profit])
#Opex = FX_EXPENSELINEITEM({"Operating Expenses"})
#EBIT = [#Gross Profit] - [#Opex]
#Operating Margin(%) = FX_OVEROPERATINGREVENUE([#EBIT])
#Depreciation = FX_EXPENSECATEGORY({"Depreciation Expenses"})
#EBITDA = [#EBIT] + [#Depreciation]
#EBITDA Margin(%) = FX_OVEROPERATINGREVENUE([#EBIT] + [#Depreciation])
#EBT = [#EBIT] + FX_REVENUECATEGORY({"Other Income"}) - FX_EXPENSECATEGORY({"Financial Expenses"})
#Income Tax Expense = [#EBT] * 0.295
#Net Income = [#EBT] - [#Income Tax Expense]
#Net Margin on Op.Rev(%) = FX_OVEROPERATINGREVENUE([#Net Income])
#Net Margin on Total Rev(%) = DIVIDE([#Net Income], [#Revenue], 0)
```

</details>

#### 📊 Costos y Punto de Equilibrio

<details>
<summary>Ver medidas de Costos y BEP</summary>

```dax
#Variable Cost = FX_EXPENSECOSTTYPE({"Variable Cost"})
#Fixed Cost = FX_EXPENSECOSTTYPE({"Fixed Cost"})
#COGS Ratio(%) = FX_OVEROPERATINGREVENUE(FX_EXPENSECATEGORY({"Cost of Goods Sold"}))
#BEP Sales = DIVIDE([#Fixed Cost], 1 - FX_OVEROPERATINGREVENUE([#Variable Cost]), 0)
#Margin of Safety(%) = DIVIDE([#Operating Revenue] - [#BEP Sales], [#Operating Revenue], 0)
```

</details>

#### 📅 Análisis Temporal (YoY)
<details>
<summary>Ver medidas de Análisis Temporal</summary>

```dax
#LY Operating Revenue = FX_LY([#Operating Revenue])
#YoY Operating Rev(%) = FX_YOY([#Operating Revenue], [#LY Operating Revenue])
```

</details>

#### 🎯 Metas y Visuales Dinámicos
<details>
<summary>Ver medidas de Metas y Visuales</summary>

```dax
COGS(%) Target = 0.3
Operating Margin (%) Target = 0.12

#ExpOverOpeRev(%) = DIVIDE(
    [#Expenses],
    CALCULATE([#Operating Revenue], ALL('Calendar'[#Month]), ALL(DimSubcategory[Subcategory])),
    0
)

SelectedImg = 
VAR result =
    SWITCH(
        TRUE(),
        SELECTEDVALUE(DimBranch[Branch], "Seleccionar todo") = "Seleccionar todo", FX_CHOOSEIMG(1),
        SELECTEDVALUE(DimBranch[Branch]) = "Oficina Central", FX_CHOOSEIMG(2),
        SELECTEDVALUE(DimBranch[Branch]) = "T. Dasso", FX_CHOOSEIMG(3),
        SELECTEDVALUE(DimBranch[Branch]) = "T. Arequipa", FX_CHOOSEIMG(4),
        SELECTEDVALUE(DimBranch[Branch]) = "T. Ica", FX_CHOOSEIMG(5),
        SELECTEDVALUE(DimBranch[Branch]) = "T. Chiclayo", FX_CHOOSEIMG(6),
        SELECTEDVALUE(DimBranch[Branch]) = "T. Cusco", FX_CHOOSEIMG(7)
    )
RETURN result

CardOverview = FX_CHOOSECARD(1)
CardBranch = FX_CHOOSECARD(2)
CardPL = FX_CHOOSECARD(3)
```
</details>

## 🖼️ Vistas Previas del Proyecto
<details>
<summary>📸 Ver Capturas de Pantalla</summary>

<img width="1911" alt="image" src="https://github.com/user-attachments/assets/a5976e58-5afd-4d45-ac86-79416ee37b7d" />
<img width="1910" alt="image" src="https://github.com/user-attachments/assets/65107f03-de5f-4e78-89cf-f04146ff8ab0" />
<img width="1905" alt="image" src="https://github.com/user-attachments/assets/7ac6b2d9-ecb9-4628-add3-75c9ee545ac6" />
</details>

<details>
<summary>🎥 Ver Video Demostrativo</summary>

https://www.youtube.com/watch?v=ISpue331_Y8
</details>

## 👤 Autor
Giancarlo Barrantes

Business Intelligence Developer | Power BI Specialist


<a href="https://www.linkedin.com/in/gb25"><img width="70" height="20" alt="image" src="https://github.com/user-attachments/assets/e0d69586-c6f9-4f66-9a3d-bc8dfa146463" /></a>
<a href="https://www.instagram.com/gbarrantes25"><img width="70" height="20" alt="image" src="https://github.com/user-attachments/assets/68773029-5e0f-49da-92e9-fb93a223dd09" />
</a>
<a href="https://www.tiktok.com/@gbarrantes25"><img width="65" height="20" alt="image" src="https://github.com/user-attachments/assets/349c87c0-1f3a-4368-8517-64a6012565c0" /></a>
<a href="https://www.youtube.com/@gbarrantes25"><img width="70" height="20" alt="image" src="https://github.com/user-attachments/assets/d9e2cb69-29ec-47fc-93ae-30896a7b533d" />
</a>

## 📝 Licencia
Este proyecto está bajo la Licencia MIT - ver el archivo <a href="https://mit-license.org/">LICENSE</a> para más detalles.

## ⭐ Agradecimientos
Si este proyecto te resultó útil, considera darle una ⭐ en GitHub y compartirlo con otros profesionales de Business Intelligence.

<div align="center">
   Hecho por <a href="https://gbarrantes25.github.io/">Giancarlo Barrantes</a>
</div>