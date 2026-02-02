

---

## 1️⃣ Gross Profit (Bruttoresultat)

**Definition:**
`Gross Profit = Revenue – COGS`

* Det viser, hvor meget virksomheden tjener **før driftsomkostninger**.
* Brugt til at vurdere, hvor profitabelt selve salget af varer/tjenester er.
* I Power BI: Du kan lave et measure:

```dax
GrossProfit = [Revenue] - [COGS]
```

---

## 2️⃣ Operating Profit (Driftsresultat / EBIT)

**Definition:**
`Operating Profit = Gross Profit – Operating Expenses`

* Operating Expenses = alle driftsomkostninger, fx løn, husleje, marketing, outside services osv.
* Viser virksomhedens profit **før finansielle poster og skat**.
* I Power BI:

```dax
OperatingProfit = [GrossProfit] - [OperatingExpenses]
```

---

## 3️⃣ Outside Services (Eksterne tjenester)

**Definition:**

* Udgifter til eksterne leverandører eller konsulenter, fx IT-support, konsulenttimer, transport.
* En del af **Operating Expenses**, ofte opdelt i rapporter.
* I Power BI kan du filtrere på konti/posteringer der hedder “Outside Services” i GL-Entry.

---

## 4️⃣ Revenue (Omsætning)

**Definition:**

* Alle indtægter fra salg af varer eller tjenester.
* Typisk posteringer i GL-Entry med konti til omsætning.
* Power BI measure eksempel:

```dax
Revenue = SUM('Fact - GL Entry'[Amount])
```

* Filter evt. på konto-nummer eller Dimension for at få kun omsætning.

---

## 5️⃣ Refreshed

**I Power BI-kontekst:**

* Henviser typisk til **sidst data blev opdateret**.
* Bruges i rapporter til at vise brugeren, om data er “up-to-date”.
* Kan sættes med DAX:

```dax
LastRefresh = NOW()
```

Eller i Power Query: brug **“DateTime.LocalNow()”** til at indsætte et felt med tid for refresh.

---

### 🔹 Opsummering i Power BI measures

| Measure            | Formel/ide                           | Brug                                 |
| ------------------ | ------------------------------------ | ------------------------------------ |
| Revenue            | SUM på Revenue-konti                 | Grundlag for profit                  |
| COGS               | SUM på kostpris-konti                | Bruttoresultat                       |
| Gross Profit       | Revenue – COGS                       | Viser profit før drift               |
| Operating Expenses | SUM på driftsomkostninger            | Basis for driftsresultat             |
| Operating Profit   | Gross Profit – Operating Expenses    | Resultat før finansposter            |
| Outside Services   | SUM på specifikke eksterne tjenester | Del af driftsomkostninger            |
| Last Refresh       | NOW() eller Power Query DateTime     | Viser hvornår rapporten er opdateret |

---


