# CFO-Dashboard-PowerBI

## 📃 Descripción General
Este proyecto fue elaborado con información ficticia de una empresa del rubro fastfood con una oficina central y 5 tiendas en Perú. Los datos empleados son ficticios.
La finalidad de este proyecto es revisar la salud financiera de la empresa.


## 📊 Contenido del proyecto
- Página "Overview": Contiene una vista general del análisis.
- Página "P&C Deep Dive": Contiene el análisis por sucursal de la cadena.
- Página "P&L": Contiene el esdtado de resultados de la empresa.


## 🛠️ Herramientas y Tecnologías Utilizadas
- Visualización: Power BI Desktop.
- Fuente de Datos:
  - [DimCenterCost.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vQ6PFClLjBQaGVinUTTAXvkXU9rfNwU-3kiuux2qMyL-WZl7p7AjV3gJRHp8UhZhuz_MLB8PLyaL8z1/pubhtml?gid=1517220622&single=true)
  - [DimDocument.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vTQruzVx53OPt6nqljWb9V0UhzNSA-1QDAqtTc7W8rvEqivT19gLfvHCmXC9q5nKpwBjT9Qh_1o7595/pubhtml?gid=775268392&single=true)
  - [DimEntity.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vQdYxEJXUrJNWj8e_BcxMak93N0ovnqHPsCZdokK_iZm4UyWCMBerzpfZDUcbVO4ma1ZKAYeFwKGh-V/pubhtml?gid=1860496830&single=true)
  - [DimProduct.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vSTxIpD47q6jTtZ_hJNbzMUgSWeUIa_PyrKuRk5f5HUcqzEKc27Lz0iDX_Jb09mLrxgNussrsBu7dT5/pubhtml?gid=248530384&single=true)
  - [DimSubcategory.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vR-7RPQosGMlYZkvK3IKwo5y0rfu0R30MR5KTvHY-vq-UF476O0ussKhzuJ5_hfCZowZ9Zo9WmoyX_C/pubhtml?gid=571723735&single=true)
  - [DimBranch.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vRrnh7vDS9fWZCEFM33f8N4ONGDAaNxtVZlCXSu1Xkk4VzeMw6crpXo3oNwvHwDch6I3bS6qCbjt-tq/pubhtml?gid=1431746542&single=true)
  - [Fact-Expenses.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vReNKN_bEEzeArYYgZhg6byO1LisSmkZIlHusdULdL_1yk5RSIeJKxbUInfsExxUYYQ9YbpSXchhv7X/pubhtml?gid=354373430&single=true)
  - [Fact-Revenue.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vRebdrai3eny64GPrLjRAXhuSBqzXV18BFVRNUi4ULliH3oE4Py7HgJ5Ri4DK8QM9S6jmMKjRUqZrcG/pubhtml?gid=147943285&single=true)

    
- Lenguajes: DAX para las medidas calculadas y Power Query (Lenguaje M) para la transformación de datos.


## ⚙️ Configuración del Entorno
- Software Necesario: Power BI Desktop.
- Instalación:
  - Descargar [Chief Financial Officer.pbix](https://github.com/Gbarrantes25/CFO-Dashboard-PowerBI/blob/main/Chief%20Financial%20Officer.pbix) con Power BI Desktop.
  - Entrar a Inicio y darle click a "Actualizar".


## 📂 Estructura del Repositorio
<code>.
  ├── Data Source                        # Contiene las tablas en formato .xlsx
  |── Images                             # Carpeta donde se alojan los íconos.
  ├── Chief Financial Officer.pbix       # Archivo que será ejecutado con Power BI Desktop.
  └── README.md                          # Este archivo.
</code>


## ✅ Características Principales
- Transformaciones en Power Query: Se realizaron procesos de limpieza y modelado de datos para optimizar el rendimiento.
- Creación de tabla calendario.
- Medidas DAX:
  - <details><summary>Abrir</summary>
    <code>
      MEASURE '_Measures'[#Revenue] = SUMX(
    			Fact_Revenue,
    			Fact_Revenue[Quantity] * RELATED(DimProducts[Price_Unit])
    		)
    	MEASURE '_Measures'[#Expenses] = SUMX(
    			Fact_Expenses,
    			Fact_Expenses[Subtotal]
    		)
    	MEASURE '_Measures'[#Net Income] = [#EBT] - [#Income Tax Expense]
    	MEASURE '_Measures'[#Net Margin on Op.Rev(%)] = FX_OVEROPERATINGREVENUE([#Net Income])
    	MEASURE '_Measures'[#BEP Sales] = DIVIDE(
    			[#Fixed Cost],
    			1 - FX_OVEROPERATINGREVENUE([#Variable Cost]),
    			0
    		)
    	MEASURE '_Measures'[#COGS Ratio(%)] = FX_OVEROPERATINGREVENUE(FX_EXPENSECATEGORY({
    			"Cost of Goods Sold"
    		}))
    	MEASURE '_Measures'[#Gross Profit] = FX_REVENUECATEGORY({
    			"Revenue"
    		}) - FX_EXPENSECATEGORY({
    			"Cost of Goods Sold"
    		})
    	MEASURE '_Measures'[#Opex] = FX_EXPENSELINEITEM({
    			"Operating Expenses"
    		})
    	MEASURE '_Measures'[#EBIT] = [#Gross Profit] - [#Opex]
    	MEASURE '_Measures'[#Income Tax Expense] = [#EBT] * 0.295
    	MEASURE '_Measures'[#EBT] = [#EBIT] + FX_REVENUECATEGORY({
    			"Other Income"
    		}) - FX_EXPENSECATEGORY({
    			"Financial Expenses"
    		})
    	MEASURE '_Measures'[#Depreciation] = FX_EXPENSECATEGORY({
    			"Depreciation Expenses"
    		})
    	MEASURE '_Measures'[#EBITDA] = [#EBIT] + [#Depreciation]
    	MEASURE '_Measures'[#Variable Cost] = FX_EXPENSECOSTTYPE({
    			"Variable Cost"
    		})
    	MEASURE '_Measures'[#Fixed Cost] = FX_EXPENSECOSTTYPE({
    			"Fixed Cost"
    		})
    	MEASURE '_Measures'[#Gross Profit Margin(%)] = FX_OVEROPERATINGREVENUE([#Gross Profit])
    	MEASURE '_Measures'[#Operating Margin(%)] = FX_OVEROPERATINGREVENUE([#EBIT])
    	MEASURE '_Measures'[#EBITDA Margin(%)] = FX_OVEROPERATINGREVENUE([#EBIT] + [#Depreciation])
    	MEASURE '_Measures'[#Operating Revenue] = FX_REVENUECATEGORY({
    			"Revenue"
    		})
    	MEASURE '_Measures'[#Cost of Goods Sold] = FX_EXPENSESUBCATEGORY({
    			"Supplies and Materials",
    			"Packaging",
    			"Beverages"
    		})
    	MEASURE '_Measures'[#Store Operating Expenses] = FX_EXPENSECATEGORY({
    			"Store Operating Expenses"
    		})
    	MEASURE '_Measures'[#Selling Expenses] = FX_EXPENSECATEGORY({
    			"Selling Expenses"
    		})
    	MEASURE '_Measures'[#Administrative Expenses] = FX_EXPENSECATEGORY({
    			"Administrative Expenses"
    		})
    	MEASURE '_Measures'[#Other Income] = FX_REVENUECATEGORY({
    			"Other Income"
    		})
    	MEASURE '_Measures'[#Financial Expenses] = FX_EXPENSECATEGORY({
    			"Financial Expenses"
    		})
    	MEASURE '_Measures'[#Net Margin on Total Rev(%)] = DIVIDE(
    			[#Net Income],
    			[#Revenue],
    			0
    		)
    	MEASURE '_Measures'[#Margin of Safety(%)] = DIVIDE(
    			[#Operating Revenue] - [#BEP Sales],
    			[#Operating Revenue],
    			0
    		)
    	MEASURE '_Measures'[#LY Operating Revenue] = FX_LY([#Operating Revenue])
    	MEASURE '_Measures'[#YoY Operating Rev(%)] = FX_YOY(
    			[#Operating Revenue],
    			[#LY Operating Revenue]
    		)
    	MEASURE '_Measures'[SelectedImg] = VAR result =
    		SWITCH(
    			TRUE(),
    			SELECTEDVALUE(
    				DimBranch[Branch],
    				"Seleccionar todo"
    			) = "Seleccionar todo", FX_CHOOSEIMG(1),
    			SELECTEDVALUE(DimBranch[Branch]) = "Oficina Central", FX_CHOOSEIMG(2),
    			SELECTEDVALUE(DimBranch[Branch]) = "T. Dasso", FX_CHOOSEIMG(3),
    			SELECTEDVALUE(DimBranch[Branch]) = "T. Arequipa", FX_CHOOSEIMG(4),
    			SELECTEDVALUE(DimBranch[Branch]) = "T. Ica", FX_CHOOSEIMG(5),
    			SELECTEDVALUE(DimBranch[Branch]) = "T. Chiclayo", FX_CHOOSEIMG(6),
    			SELECTEDVALUE(DimBranch[Branch]) = "T. Cusco", FX_CHOOSEIMG(7)
    		)
    		RETURN
    			result
    	MEASURE '_Measures'[CardBranch] = FX_CHOOSECARD(2)
    	MEASURE '_Measures'[CardOverview] = FX_CHOOSECARD(1)
    	MEASURE '_Measures'[COGS(%) Target] = 0.3
    	MEASURE '_Measures'[Operating Margin (%) Target] = 0.12
    	MEASURE '_Measures'[CardPL] = FX_CHOOSECARD(3)
    	MEASURE '_Measures'[#ExpOverOpeRev(%)] = DIVIDE(
    			[#Expenses],
    			CALCULATE(
    				[#Operating Revenue],
    				ALL('Calendar'[#Month]),
    				ALL(DimSubcategory[Subcategory])
    			),
    			0
    		)
    </code>
    </details> 
- Funciones Definidas por el Usuario (UDF):
  - <details><summary>Abrir</summary>
    <code>
      /// Inserta una medida numérica básica para calcular el valor del año anterior.
      FUNCTION fx_LY = (_measure: expr) =>
    		CALCULATE(
    			_measure,
    			SAMEPERIODLASTYEAR('Calendar'[Date])
    		)
      /// Inserta una medida numérica básica para calcular la variación % respecto al año anterior.
      FUNCTION fx_YoY = (_currentyear: expr, _lastyear: expr) =>
    		DIVIDE(
    			_currentyear - _lastyear,
    			_lastyear,
    			0
    		)
      /// Ingresa las categorías "Cost of Goods Sold", "Operating Expenses" o "Financial Expenses") de egresos entre llaves {"Line1","Line2"} para calcular los egresos.
      FUNCTION fx_ExpenseLineItem = (_LineItemTable: table) =>
          VAR _value = CALCULATE([#Expenses],DimSubcategory[Line_Item] IN _LineItemTable)
          VAR _result = IF(ISBLANK(_value) || _value = 0,0,_value)
          RETURN _result
      /// Ingresa una o varias subcategorías entre llaves "{ }" y calcula el gasto.
      FUNCTION fx_ExpenseSubcategory = (_SubcategoryTable: table) =>
          VAR _value = CALCULATE([#Expenses],DimSubcategory[Subcategory] IN _SubcategoryTable)
          VAR _result = IF(ISBLANK(_value) || _value = 0,0,_value)
          RETURN _result
      /// Ingresa "Cost of Goods Sold", "Store Operating Expenses", "Selling Expenses", "Administrative Expenses" o "Financial Expenses" entre llave "{ }" para calcular los gastos por categoría.
      FUNCTION fx_ExpenseCategory = (_CategoryTable: table) =>
          VAR _value = CALCULATE([#Expenses],DimSubcategory[Category] IN _CategoryTable)
          VAR _result = IF(ISBLANK(_value) || _value = 0,0,_value)
          RETURN _result
      /// Ingresa el tipo de costo "Variable Cost" o "Fixed Cost" entre llaves "{ }" para calcular el tipo de costo.
      FUNCTION fx_ExpenseCostType = (_CostTypeTable: table) =>
          VAR _value = CALCULATE([#Expenses],DimSubcategory[Cost_Type] IN _CostTypeTable)
          VAR _result = IF(ISBLANK(_value) || _value = 0,0,_value)
          RETURN _result
      /// Ingresa el tipo de categoría Revenue ("Revenue" u "Other Income") entre llames "{ }" para calcular el tipo de ingreso.
      FUNCTION fx_RevenueCategory = (_RevenueCategoryTable: table) =>
          VAR _value = CALCULATE([#Revenue],DimSubcategory[Category] IN _RevenueCategoryTable)
          VAR _result = IF(ISBLANK(_value) || _value = 0,0,_value)
          RETURN _result
      /// Inserta una medida para calcular el % respecto a los ingresos operativos.
      FUNCTION fx_OverOperatingRevenue = (_exp: expr) => DIVIDE(_exp,fx_RevenueCategory({"Revenue"}),0)
      /// Opción; 1- Card Overview, 2- Card Branch, 3- Statement Income
      FUNCTION fx_ChooseCard = (_Option: int64) =>
      -- Card Overview --
      VAR _CardOverview =
        VAR _MeasureTop1 = FX_FORMATERNUMBERPEN([#Revenue])
    		VAR _MeasureTop2 = FORMAT(
    			[#EBITDA Margin(%)],
    			"0.00%",
    			"En-Us"
    		)
    		VAR _MeasureTop3 = FORMAT(
    			[#Gross Profit Margin(%)],
    			"0.00%",
    			"En-Us"
    		)
    		VAR _MeasureTop4 = FORMAT(
    			[#Operating Margin(%)],
    			"0.00%",
    			"En-Us"
    		)
    		VAR _MeasureTop5 = FORMAT(
    			[#Margin of Safety(%)],
    			"0.00%",
    			"En-Us"
    		)
    		VAR _MeasureTop6 = FORMAT(
    			[#Net Margin on Op.Rev(%)],
    			"0.00%",
    			"En-Us"
    		)
    		VAR _MeasureBottom1 = FX_FORMATERNUMBERPEN([#Expenses])
    		VAR _MeasureBottom2 = FX_FORMATERNUMBERPEN([#EBITDA])
    		VAR _MeasureBottom3 = FX_FORMATERNUMBERPEN([#Gross Profit])
    		VAR _MeasureBottom4 = FX_FORMATERNUMBERPEN([#EBIT])
    		VAR _MeasureBottom5 = FX_FORMATERNUMBERPEN([#BEP Sales])
    		VAR _MeasureBottom6 = FX_FORMATERNUMBERPEN([#Net Income])
    		VAR _TextMarginTop1 = "Total Revenue"
    		VAR _TextMarginTop2 = "EBITDA Margin %"
    		VAR _TextMarginTop3 = "Gross Profit Margin %"
    		VAR _TextMarginTop4 = "Operating Margin %"
    		VAR _TextMarginTop5 = "Margin of Safety %"
    		VAR _TextMarginTop6 = "Net Margin %"
    		VAR _TextMarginBotton1 = "Total Expenses"
    		VAR _TextMarginBotton2 = "EBITDA"
    		VAR _TextMarginBotton3 = "Gross Profit"
    		VAR _TextMarginBotton4 = "EBIT"
    		VAR _TextMarginBotton5 = "Break-Even Point"
    		VAR _TextMarginBotton6 = "Net Income"
    		VAR _TextStatus1 = ""
    		VAR _TextStatus2 = SWITCH(
    			TRUE(),
    			[#EBITDA Margin(%)] < 0.08, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='24px'>",
    			[#EBITDA Margin(%)] < 0.15, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='24px'>",
    			[#EBITDA Margin(%)] < 0.22, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='24px'>",
    			[#EBITDA Margin(%)] >= 0.22, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='24px'>"
    		)
    		VAR _TextStatus3 = SWITCH(
    			TRUE(),
    			[#Gross Profit Margin(%)] < 0.6, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='24px'>",
    			[#Gross Profit Margin(%)] < 0.64, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='24px'>",
    			[#Gross Profit Margin(%)] < 0.72, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='24px'>",
    			[#Gross Profit Margin(%)] >= 0.72, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='24px'>"
    		)
    		VAR _TextStatus4 = SWITCH(
    			TRUE(),
    			[#Operating Margin(%)] < 0.06, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='24px'>",
    			[#Operating Margin(%)] < 0.12, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='24px'>",
    			[#Operating Margin(%)] < 0.18, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='24px'>",
    			[#Operating Margin(%)] >= 0.18, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='24px'>"
    		)
    		VAR _TextStatus5 = SWITCH(
    			TRUE(),
    			[#Margin of Safety(%)] < 0.10, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='24px'>",
    			[#Margin of Safety(%)] < 0.21, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='24px'>",
    			[#Margin of Safety(%)] < 0.50, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='24px'>",
    			[#Margin of Safety(%)] >= 0.50, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='24px'>"
    		)
    		VAR _TextStatus6 = SWITCH(
    			TRUE(),
    			[#Net Margin on Op.Rev(%)] < 0.03, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='24px'>",
    			[#Net Margin on Op.Rev(%)] < 0.08, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='24px'>",
    			[#Net Margin on Op.Rev(%)] < 0.13, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='24px'>",
    			[#Net Margin on Op.Rev(%)] >= 0.13, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='24px'>"
    		)
    		VAR _card1 =
    		"
                <style>
                    body {
                        margin: 0;
                        font-family: 'Segoe UI', Arial, sans-serif;
                    }
                    .kpi-card {
                        width: 1258px;
                        height: 180px;
                        border-radius: 16px;
                        background: linear-gradient(135deg, #00C0FF, #4218B8);
                        box-shadow: 0 10px 24px rgba(0,0,0,0.25);
                        padding: 14px;
                        box-sizing: border-box;
                        display: flex;
                        align-items: center;
                    }
                    .kpi-grid {
                        width: 100%;
                        display: grid;
                        grid-template-columns: repeat(6, 1fr);
                        gap: 12px;
                        height: 100%;
                    }
                    .kpi-item {
                        background: rgba(255,255,255,0.14);
                        border-radius: 12px;
                        backdrop-filter: blur(6px);
                        display: flex;
                        flex-direction: column; /* ← división vertical */
                        overflow: hidden;
                        transition: all 0.25s ease;
                        color: #ffffff; /* ← TODO blanco */
                    }
                    .kpi-item:hover {
                        background: rgba(255,255,255,0.22);
                        transform: translateY(-2px);
                    }
                    /* parte superior */
                    .kpi-top {
                        flex: 1;
                        padding: 8px 6px;
                        text-align: center;
                        border-bottom: 1px solid rgba(255,255,255,0.25);
                        display: flex;
                        flex-direction: column;
                        justify-content: center;
                        color: #ffffff;
                    }
                    .margin-value {
                        font-size: 16px;
                        font-weight: 700;
                        line-height: 1.1;
                        color: #ffffff;
                    }
                    .margin-category {
                        font-size: 12px;
                        margin-top: 2px;
                        color: #ffffff;
                    }
                    .margin-status {
                        font-size: 12px;
                        margin-top: 2px;
                        color: #ffffff;
                        opacity: 0.95;
                    }
                    /* parte inferior */
                    .kpi-bottom {
                        flex: 1;
                        padding: 8px 6px;
                        text-align: center;
                        display: flex;
                        flex-direction: column;
                        justify-content: center;
                        color: #ffffff;
                    }
                    .amount-value {
                        font-size: 16px;
                        font-weight: 700;
                        line-height: 1.1;
                        color: #ffffff;
                    }
                    .amount-category {
                        font-size: 12px;
                        margin-top: 2px;
                        color: #ffffff;
                    }
                </style>
                </head>
                <body>
                <div class='kpi-card'>
                    <div class='kpi-grid'>
                        <!-- KPI 1 -->
                        <div class='kpi-item'>
                            <div class='kpi-top'>
                                <div class='margin-value'>" & _MeasureTop1 & "</div>
                                <div class='margin-category'>" & _TextMarginTop1 & "</div>
                                <div class='margin-status'>" & _TextStatus1 & "</div>
                            </div>
                            <div class='kpi-bottom'>
                                <div class='amount-value'>" & _MeasureBottom1 & "</div>
                                <div class='amount-category'>" & _TextMarginBotton1 & "</div>
                            </div>
                        </div>
                        <!-- KPI 2 -->
                        <div class='kpi-item'>
                            <div class='kpi-top'>
                                <div class='margin-value'>" & _MeasureTop2 & "</div>
                                <div class='margin-category'>" & _TextMarginTop2 & "</div>
                                <div class='margin-status'>" & _TextStatus2 & "</div>
                            </div>
                            <div class='kpi-bottom'>
                                <div class='amount-value'>" & _MeasureBottom2 & "</div>
                                <div class='amount-category'>" & _TextMarginBotton2 & "</div>
                            </div>
                        </div>
                        <!-- KPI 3 -->
                        <div class='kpi-item'>
                            <div class='kpi-top'>
                                <div class='margin-value'>" & _MeasureTop3 & "</div>
                                <div class='margin-category'>" & _TextMarginTop3 & "</div>
                                <div class='margin-status'>" & _TextStatus3 & "</div>
                            </div>
                            <div class='kpi-bottom'>
                                <div class='amount-value'>" & _MeasureBottom3 & "</div>
                                <div class='amount-category'>" & _TextMarginBotton3 & "</div>
                            </div>
                        </div>
                        <!-- KPI 4 -->
                        <div class='kpi-item'>
                            <div class='kpi-top'>
                                <div class='margin-value'>" & _MeasureTop4 & "</div>
                                <div class='margin-category'>" & _TextMarginTop4 & "</div>
                                <div class='margin-status'>" & _TextStatus4 & "</div>
                            </div>
                            <div class='kpi-bottom'>
                                <div class='amount-value'>" & _MeasureBottom4 & "</div>
                                <div class='amount-category'>" & _TextMarginBotton4 & "</div>
                            </div>
                        <Zdiv>
                        <!-- KPI 5 -->
                        <div class='kpi-item'>
                            <div class='kpi-top'>
                                <div class='margin-value'>" & _MeasureTop5 & "</div>
                                <div class='margin-category'>" & _TextMarginTop5 & "</div>
                                <div class='margin-status'>" & _TextStatus5 & "</div>
                            </div>
                            <div class='kpi-bottom'>
                                <div class='amount-value'>" & _MeasureBottom5 & "</div>
                                <div class='amount-category'>" & _TextMarginBotton5 & "</div>
                            </div>
                        </div>
                        <!-- KPI 6 -->
                        <div class='kpi-item'>
                            <div class='kpi-top'>
                                <div class='margin-value'>" & _MeasureTop6 & "</div>
                                <div class='margin-category'>" & _TextMarginTop6 & "</div>
                                <div class='margin-status'>" & _TextStatus6 & "</div>
                            </div>
                            <div class='kpi-bottom'>
                                <div class='amount-value'>" & _MeasureBottom6 & "</div>
                                <div class='amount-category'>" & _TextMarginBotton6 & "</div>
                            </div>
                        </div>
                    </div>
                </div>
                </body>
        "
    		RETURN
    			_card1
    -- Card Branch--
    VAR _CardBranch =
    		VAR _statusEBITDA =
    		IF(
    			SELECTEDVALUE(
    				DimBranch[Branch],
    				""
    			) = "Oficina Central",
    			"",
    			SWITCH(
    				TRUE(),
    				[#EBITDA Margin(%)] < 0.08, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='18px'>",
    				[#EBITDA Margin(%)] < 0.15, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='18px'>",
    				[#EBITDA Margin(%)] < 0.22, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='18px'>",
    				[#EBITDA Margin(%)] >= 0.22, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='18px'>"
    			)
    		)
    		VAR _statusGrossProfit =
    		IF(
    			SELECTEDVALUE(
    				DimBranch[Branch],
    				""
    			) = "Oficina Central",
    			"",
    			SWITCH(
    				TRUE(),
    				[#Gross Profit Margin(%)] < 0.6, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='18px'>",
    				[#Gross Profit Margin(%)] < 0.64, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='18px'>",
    				[#Gross Profit Margin(%)] < 0.72, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='18px'>",
    				[#Gross Profit Margin(%)] >= 0.72, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='18px'>"
    			)
    		)
    		VAR _statusOperating =
    		IF(
    			SELECTEDVALUE(
    				DimBranch[Branch],
    				""
    			) = "Oficina Central",
    			"",
    			SWITCH(
    				TRUE(),
    				[#Operating Margin(%)] < 0.06, "Poor " & "<img src='" & FX_CHOOSEIMG(8) & "' width='18px'>",
    				[#Operating Margin(%)] < 0.12, "Fair " & "<img src='" & FX_CHOOSEIMG(9) & "' width='18px'>",
    				[#Operating Margin(%)] < 0.18, "Healthy " & "<img src='" & FX_CHOOSEIMG(10) & "' width='18px'>",
    				[#Operating Margin(%)] >= 0.18, "Excellent " & "<img src='" & FX_CHOOSEIMG(11) & "' width='18px'>"
    			)
    		)
    		VAR _revenue =
    		IF(
    			[#Revenue] = 0 || ISBLANK([#Revenue]),
    			FORMAT(
    				0,
    				"""S/""" & "0.00",
    				"En-Us"
    			),
    			FX_FORMATERNUMBERPEN([#Revenue])
    		)
    		VAR _expenses =
    		IF(
    			[#Expenses] = 0 || ISBLANK([#Expenses]),
    			FORMAT(
    				0,
    				"""S/""" & "0.00",
    				"En-Us"
    			),
    			FX_FORMATERNUMBERPEN([#Expenses])
    		)
    		VAR _card2 =
		    "
            <style>
                body {
                    margin: 0;
                    font-family: 'Segoe UI', Arial, sans-serif;
                }
                .kpi-card {
                    width: 544px;
                    height: 192px;
                    padding: 18px 22px;
                    border-radius: 16px;
                    background: linear-gradient(135deg, #00C0FF, #4218B8);
                    color: #ffffff;
                    box-sizing: border-box;
                    box-shadow: 0 8px 20px rgba(0,0,0,0.25);
                    display: flex;
                    flex-direction: column;
                    justify-content: center;
                }
                .kpi-title {
                    font-size: 16px;
                    font-weight: 600;
                    opacity: 0.95;
                    margin-bottom: 14px;
                    letter-spacing: 0.4px;
                }
                .kpi-grid {
                    display: grid;
                    grid-template-columns: repeat(3, 1fr);
                    gap: 14px;
                }
                .kpi-item {
                    background: rgba(255,255,255,0.14);
                    border-radius: 10px;
                    padding: 10px 6px;
                    text-align: center;
                    backdrop-filter: blur(5px);
                    transition: all 0.25s ease;
                }
                .kpi-item:hover {
                    background: rgba(255,255,255,0.22);
                    transform: translateY(-1px);
                }
                .kpi-label {
                    font-size: 11px;
                    opacity: 0.9;
                    margin-bottom: 4px;
                    line-height: 1.2;
                }
                .kpi-value {
                    font-size: 16px;
                    font-weight: 700;
                    letter-spacing: 0.3px;
                }
            </style>
            <div class='kpi-card'>
                <div class='kpi-title'>Financial Margins</div>
                <div class='kpi-grid'>
                    <div class='kpi-item'>
                        <div class='kpi-label'>Revenue</div>
                        <div class='kpi-value'>" & _revenue & "</div>
                        <div class='kpi-label'>Expenses</div>
                        <div class='kpi-value'>" & _expenses & "</div>
                    </div>
                    <div class='kpi-item'>
                        <div class='kpi-label'>EBITDA Margin %</div>
                        <div class='kpi-value'>"
                    & FORMAT(
                        [#EBITDA Margin(%)],
                        "0.00%",
                        "En-Us"
                    ) & "</div>
                        <div class='kpi-label'>" & _statusEBITDA & "</div>
                    </div>
                    <div class='kpi-item'>
                        <div class='kpi-label'>Gross Profit Margin %</div>
                        <div class='kpi-value'>"
                    & FORMAT(
                        [#Gross Profit Margin(%)],
                        "0.00%",
                        "En-Us"
                    ) & "</div>
                        <div class='kpi-label'>" & _statusGrossProfit & "</div>
                    </div>
                </div>
            </div>
        "
		    RETURN
			    _card2
    -- PL --
    VAR _CardPL =
    		VAR _time = IF(
    			HASONEVALUE('Calendar'[#Year]),
    			SELECTEDVALUE('Calendar'[#Year]),
    			"Todo / All"
    		)
    		VAR _OperatingRevenue = FX_FORMATERNUMBER2([#Operating Revenue])
    		VAR _OtherIncome = FX_FORMATERNUMBER2([#Other Income])
    		VAR _COGS = FX_FORMATERNUMBER2([#Cost of Goods Sold])
    		VAR _GrossProfit = FX_FORMATERNUMBER2([#Gross Profit])
    		VAR _GrossMargin = FORMAT(
    			[#Gross Profit Margin(%)],
    			"0.00%",
    			"En-Us"
    		)
    		VAR _StoreOperating = FX_FORMATERNUMBER2([#Store Operating Expenses])
    		VAR _SellingExpenses = FX_FORMATERNUMBER2([#Selling Expenses])
    		VAR _AdmExpenses = FX_FORMATERNUMBER2([#Administrative Expenses])
    		VAR _Depreciation = FX_FORMATERNUMBER2([#Depreciation])
    		VAR _EBIT = FX_FORMATERNUMBER2([#EBIT])
    		VAR _OperatingMargin = FORMAT(
    			[#Operating Margin(%)],
    			"0.00%",
    			"En-Us"
    		)
    		VAR _FinancialExpenses = FX_FORMATERNUMBER2([#Financial Expenses])
    		VAR _EBT = FX_FORMATERNUMBER2([#EBT])
    		VAR _TaxIncome = FX_FORMATERNUMBER2([#Income Tax Expense])
    		VAR _NetIncome = FX_FORMATERNUMBER2([#Net Income])
    		VAR _NetMargin = FORMAT(
    			[#Net Margin on Op.Rev(%)],
    			"0.00%",
    			"En-Us"
    		)
		    VAR _card3 =
		    "
            <style>
                body{
                    margin:0;
                    font-family:'Segoe UI', Arial, sans-serif;
                    background:transparent;
                }
                .is-card{
                    width:600px;
                    height:540px;
                    background:linear-gradient(135deg,#00C0FF,#4218B8);
                    padding:14px 16px;
                    box-sizing:border-box;
                    color:#ffffff;
                    overflow:hidden;
                    display:flex;
                    flex-direction:column;
                    /* ✨ NUEVO */
                    border-radius:16px;
                    box-shadow:0 14px 32px rgba(0,0,0,0.28);
                }
                .is-title{
                    font-size:18px;
                    font-weight:700;
                    margin-bottom:8px;
                    letter-spacing:.3px;
                    flex-shrink:0;
                }
                table{
                    width:100%;
                    border-collapse:collapse;
                    font-size:12px;
                    line-height:1.15;
                }
                thead th{
                    text-align:left;
                    padding:12px 5px;
                    font-weight:600;
                    border-bottom:1px solid rgba(255,255,255,.40);
                    color:#ffffff;
                }
                thead th:last-child{
                    text-align:right;
                }
                tbody td{
                    padding:6px 5px;
                    border-bottom:1px solid rgba(255,255,255,.14);
                    color:#ffffff;
                }
                tbody td:last-child{
                    text-align:right;
                    font-variant-numeric:tabular-nums;
                    font-weight:500;
                }
                .bold-row td{
                    font-weight:700;
                    background:rgba(255,255,255,.08);
                }
                .percent{
                    letter-spacing:.25px;
                }
                tbody tr:hover{
                    background:rgba(255,255,255,.05);
                }
            </style>
            </head>
            <body>
            <div class='is-card'>
                <div class='is-title'>
                    Estado de Resultados / Income Statement (PEN)
                </div>
                <table>
                    <thead>
                        <tr>
                            <th>Concepto (Español / English)</th>
                            <th>" & _time & "</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>Ingresos Operativos / Operating Revenue</td>
                            <td>" & _OperatingRevenue & "</td>
                        </tr>
                        <tr>
                            <td>Costo de Ventas / Cost of Goods Sold (COGS)</td>
                            <td>" & _COGS & "</td>
                        </tr>
                        <tr class='bold-row'>
                            <td>Utilidad Bruta / Gross Profit</td>
                            <td>" & _GrossProfit & "</td>
                        </tr>
                        <tr class='bold-row'>
                            <td>Margen Bruto % / Gross Margin %</td>
                            <td class='percent'>" & _GrossMargin & "</td>
                        </tr>
                        <tr>
                            <td>Gastos Operativos de Tienda / Store Operating Expenses</td>
                            <td>" & _StoreOperating & "</td>
                        </tr>
                        <tr>
                            <td>Gastos de Venta / Selling Expenses</td>
                            <td>" & _SellingExpenses & "</td>
                        </tr>
                        <tr>
                            <td>Gastos Administrativos / Administrative Expenses</td>
                            <td>" & _AdmExpenses & "</td>
                        </tr>
                        <tr>
                            <td>Depreciación y Amortización / Depreciation & Amortization</td>
                            <td>" & _Depreciation & "</td>
                        </tr>
                        <tr class='bold-row'>
                            <td>Utilidad Operativa / EBIT</td>
                            <td>" & _EBIT & "</td>
                        </tr>
                        <tr class='bold-row'>
                            <td>Margen Operativo % / Operating Margin %</td>
                            <td class='percent'>" & _OperatingMargin & "</td>
                        </tr>
                        <tr>
                            <td>Otros Ingresos No Operativos / Other Income</td>
                            <td>" & _OtherIncome & "</td>
                        </tr>
                        <tr>
                            <td>Gastos Financieros / Interest Expense</td>
                            <td>" & _FinancialExpenses & "</td>
                        </tr>
                        <tr class='bold-row'>
                            <td>Utilidad antes de Impuestos / EBT</td>
                            <td>" & _EBT & "</td>
                        </tr>
                        <tr>
                            <td>Impuesto a la Renta 29.5 % / Income Tax Expense</td>
                            <td>" & _TaxIncome & "</td>
                        </tr>
                        <tr class='bold-row'>
                            <td>Utilidad Neta / Net Income</td>
                            <td>" & _NetIncome & "</td>
                        </tr>
                        <tr class='bold-row'>
                            <td>Margen Neto % / Net Profit Margin %</td>
                            <td class='percent'>" & _NetMargin & "</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            </body>
      "
		  RETURN
			  _card3
  VAR _card = SWITCH(TRUE(),
                  _Option=1,_CardOverview,
                  _Option=2,_CardBranch,
                  _Option=3,_CardPL,
                  "")
  
  RETURN _card
        /// Ingresa una medida para formatear el valor a "K" o "M" en moneda S/.
        FUNCTION fx_FormaterNumberPEN = (_exp: expr) => 
          VAR Valor = _exp
          VAR AbsValor = ABS(Valor)
          RETURN
          SWITCH( TRUE(),
              AbsValor >= 1000000, FORMAT(Valor, """S/"" #,##0,,.00 ""M"";-""S/"" #,##0,,.00 ""M"""),
              AbsValor >= 1000, FORMAT(Valor, """S/"" #,##0,.00 ""K"";-""S/"" #,##0,.00 ""K"""),
              FORMAT(Valor, """S/"" #,##0.00;-""S/"" #,##0.00")
          )
          /// Ingresa una medida para formatear el valor a "K" o "M" sin moneda.
          FUNCTION fx_FormaterNumber = (_exp : expr) =>
            VAR Valor = _exp
            VAR AbsValor = ABS(Valor)
            RETURN
            SWITCH( TRUE(),
                AbsValor >= 1000000, FORMAT(Valor, "#,##0,,.00 ""M"";-#,##0,,.00 ""M"""),
                AbsValor >= 1000, FORMAT(Valor, "#,##0,.00 ""K"";-#,##0,.00 ""K"""),
                FORMAT(Valor, "#,##0.00;-#,##0.00")
            )
        /// Ingresa una medida para formatear el valor sin moneda y solo dos decimales.
            FUNCTION fx_FormaterNumber2 = (_exp:expr) =>
            FORMAT(_exp,"#,,,0.00","En-Us")
        /// Opción: 1-Store Default, 2-Head Office, 3-Tienda2, 4-Tienda3, 5-Tienda4, 6-Tienda5, 7-Tienda6, 8-Poor Icon, 9-Fair Icon, 10-Healthy Icon, 11-Excellent Icon
        FUNCTION fx_ChooseImg = (_Option:int64) =>
          	VAR _result =
          	SWITCH(
          		TRUE(),
          		/// Images Store
          		_Option = 1, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Default.png",
          		_Option = 2, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/OficinaCentral.jpg",
          		_Option = 3, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Tienda2.jpg",
          		_Option = 4, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Tienda3.jpg",
          		_Option = 5, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Tienda4.jpg",
          		_Option = 6, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Tienda5.jpg",
          		_Option = 7, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Tienda6.jpg",
              // Status Icons
              _Option = 8, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Poor.png",
              _Option = 9, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Fair.png",
              _Option = 10, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Healthy.png",
              _Option = 11, "https://raw.githubusercontent.com/Gbarrantes25/CFO-Dashboard-PowerBI/refs/heads/main/Images/Excellent.png"
          	)
        	RETURN
        		_result
        </code>
    </details>
 
- Diseño Interactivo: Uso de paginado para navegación, marcadores y segmentación de datos.

## 🖼️ Vistas Previas del proyecto
<details>
  <summary>Capturas</summary>
    <img width="1911" height="1064" alt="image" src="https://github.com/user-attachments/assets/a5976e58-5afd-4d45-ac86-79416ee37b7d" />
    <img width="1910" height="1067" alt="image" src="https://github.com/user-attachments/assets/65107f03-de5f-4e78-89cf-f04146ff8ab0" />
    <img width="1905" height="1060" alt="image" src="https://github.com/user-attachments/assets/7ac6b2d9-ecb9-4628-add3-75c9ee545ac6" />

</details>

<details>
  <summary>Video</summary>
  https://youtu.be/NiG0zqzfKKQ?si=KqO9DkIukfFloHaU
</details>



## 👤 Autor
- Giancarlo Barrantes
- Lima, Perú
- [Linkedin](https://www.linkedin.com/in/gb25/)
