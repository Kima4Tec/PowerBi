# Logbog

## Demo opgave
#### 1. Hente data fra excel
#### 2. Transform 
   - vælger fire tabeller ud af 17, som er power bis forslag: Dim-Account, Fact - GL Entry, VendorLedgerEntries, CustLedgerEntries
     
   GL-Entry → hovedbogen (det samlede regnskab) 
   
   VendorLedgerEntries → leverandører (kreditorer)
    
   CustLedgerEntries → kunder (debitorer)
#### 3. Renser data 
   1. Dim-account:   
    Der er oversættelser i kolonnerne dk-eng. Jeg beholder dem. 
    Hvis der var flere sporg kunne man lave en sprog-tabel. 
    Sletter de 4 nederste rækker.
   2. Fact-GL Entry
      Sletter 5 kolonner med null-værdier og column-navne, og sletter 4 nederste rækker
   3. Fact - VendorLedgerEntries 
      Sletter kolonne med overskrift Source_Code, da den kun består af null-værdier og det samme med 5 nederste rækker.
   4. Fact - CustLedgerEntries
      Beholder det hele. Der er ingen null-værdier, men nogle med 0, som måske kunne slettes, men som jeg ikke er sikker på, om jeg vil bruge i senere sammeligninger.
   5. Forsøgte at lukke og anvende. GL Entry drillede fordi den havde error i 206 rækker af DocumentNo. Rettede det ved at fjerne step: Ændret type.
   6. Lavet en Calendar ift GL Entry, men skulle ændre Posting Date til type Dato for at det kunne lykkes med denne DAX:
   ```
Calendar = 
ADDCOLUMNS(
    CALENDAR(
        MIN('Fact - GL Entry'[Posting Date]), 
        MAX('Fact - GL Entry'[Posting Date])
    ),
    "Year", YEAR([Date]),
    "MonthNumber", MONTH([Date]),
    "MonthName", FORMAT([Date], "MMMM"),
    "YearMonth", FORMAT([Date], "YYYY-MM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "DateKey", [Date]
)
```

Lavede to measures for at kunne sammenligne sidste års amount med dette år:
Current Year
```
Amount_CY = 
CALCULATE(
    SUM('Fact - GL Entry'[Amount]),
    YEAR('Fact - GL Entry'[Posting Date]) = YEAR(TODAY())
)
```
Last Year
```
Amount_LY = 
CALCULATE(
    SUM('Fact - GL Entry'[Amount]),
    SAMEPERIODLASTYEAR('Calendar'[Date])
)

```
og en procentvis ændring:
```
Amount_CY_vs_LY_pct = 
DIVIDE(
    [Amount_CY] - [Amount_LY],
    [Amount_LY],
    0
)

```

