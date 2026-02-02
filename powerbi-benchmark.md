Benchmark
---

## 1️⃣ Benchmark vs Actuals

* **Actuals:** Faktiske tal fra `Fact - GL Entry` / Vendor / Customer Ledger.
* **Benchmark:** Referenceværdi – kan være budget, forecast, LY eller et fast mål, du vil sammenligne med.

I din liste ser det ud til, at du har en række nøgleposter i benchmark, som typisk er measures i Power BI.

---

## 2️⃣ Forklaring af hvert element

| Navn                              | Hvad det betyder                                            | Hvordan bruges i Power BI                                                           |
| --------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **COGS**                          | Cost of Goods Sold – benchmark for kostpris af solgte varer | Measure: SUM af COGS-konti i benchmark tabellen                                     |
| **Gross Profit**                  | Revenue – COGS                                              | Measure: `[Benchmark Revenue] – [Benchmark COGS]`                                   |
| **GP %**                          | Gross Profit procent                                        | Measure: `[Gross Profit] / [Revenue]`                                               |
| **Net Income**                    | Nettoresultat (efter alle drifts- og finansposter)          | Measure: SUM af alle relevante konti                                                |
| **Bench Net Profit %**            | Nettoresultat i procent af Revenue                          | `[Net Income] / [Revenue]`                                                          |
| **Bench Operating Expenses**      | Driftsomkostninger i benchmark                              | SUM af alle driftsomkostningskonti                                                  |
| **Bench Operating Profit %**      | Operating Profit som % af Revenue                           | `([Gross Profit] – [Operating Expenses]) / Revenue`                                 |
| **Bench Outside Services**        | Eksterne tjenester i benchmark                              | SUM af konti for konsulent/leverandørtjenester                                      |
| **Bench Outside Services diff %** | Afvigelse Actuals vs Benchmark                              | `([Actual Outside Services] – [Bench Outside Services]) / [Bench Outside Services]` |
| **Bench Revenue**                 | Omsætning i benchmark                                       | SUM af Revenue konti i benchmark                                                    |
| **Blank**                         | Plads til tom celle i visuelle for layout                   | Kan ignoreres                                                                       |
| **Headline Forecast**             | Overordnet forecast (f.eks. kvartal/år)                     | Kan være mål baseret på budget/forecast                                             |
| **Headline Overview**             | Opsummering af KPI’er                                       | Typisk samlemeasure, fx Total Revenue, Total Profit                                 |
| **SelectedBench**                 | Valgt benchmark (budget, LY, forecast)                      | Dynamisk parameter for at skifte benchmark i visuals                                |

---

## 3️⃣ Hvordan det implementeres i Power BI

### A) Lav separate **Benchmark measures**

Eksempel for Revenue og COGS:

```dax
BenchRevenue = SUM('Benchmark Table'[Revenue])
BenchCOGS = SUM('Benchmark Table'[COGS])
BenchGrossProfit = [BenchRevenue] - [BenchCOGS]
BenchGPPercent = DIVIDE([BenchGrossProfit], [BenchRevenue], 0)
```

* Samme logik kan bruges for Operating Expenses, Outside Services osv.
* Afvigelser vs Actuals:

```dax
BenchOutsideServicesDiffPercent = 
DIVIDE(
    [ActualOutsideServices] - [BenchOutsideServices],
    [BenchOutsideServices],
    0
)
```

---

### B) Kombiner med Actuals

* X-akse: `Calendar[YearMonth]`
* Værdier: `Actual Revenue`, `Bench Revenue`, `Actual Gross Profit`, `Bench Gross Profit`
* % measures vises som linjer eller KPI-cards

---

### C) Dynamisk benchmark

* `SelectedBench` kan være en **parameter tabel** med fx LY, Budget, Forecast
* Brug **SWITCH / SELECTEDVALUE** i measures for at vælge det benchmark, der skal vises

Eksempel:

```dax
RevenueSelectedBench =
SWITCH(
    SELECTEDVALUE('Benchmark Selector'[BenchType]),
    "LY", [Revenue_LY],
    "Budget", [BenchRevenue],
    "Forecast", [Headline Forecast],
    BLANK()
)
```

---

### 4️⃣ Tips til rapportopbygning

1. **Hold Actuals og Benchmark adskilt** i measures
2. Brug **Calendar table** som fælles dimension
3. Lav **procentvise afvigelser** (diff %) for KPI’er
4. Brug **Headline measures** til overblik på dashboards

---

💡 **Kort sagt:**

* De første 10–12 punkter i din liste er **standard finans-KPI’er** for at sammenligne actuals med benchmark.
* “Blank”, “Headline Forecast”, “Headline Overview” og “SelectedBench” bruges til layout og dynamiske visuals.

---

