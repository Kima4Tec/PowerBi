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
   3. 
   
