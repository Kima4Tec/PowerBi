## M
### M: get data from api with token
```M
= (Query as text) => let
        EndPoint = "statistik",
        Url = "https://api.navn.dk/api/v1/",
        Token = "Indsæt API-nøgle her",
        auth_key = "Bearer "&Token,
        header= [
            #"Authorization" = auth_key,
            #"Content-Type" = "application/json"
        ],
        body = Text.ToBinary(Query),
        rawdata = Web.Contents(Url&EndPoint, [Headers=header, Content=body]),
        jsonData = Json.Document(rawdata),
        toTable = Table.FromList(jsonData, Splitter.SplitByNothing(), null, null, ExtraValues.Error)
    in
        toTable
```
Nyere fra Anders
```
let
    GetPages = (queryParams)=>
let
    Host = "https://localhost:7279",
    Source = Json.Document(
    Web.Contents(
    Host,
    [RelativePath = "api/auth/users", Query = queryParams, Headers=[#"Content-Type"="application/json", Authorization="Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoiYWRtaW4iLCJ1c2VyTmFtZSI6ImFkbWluIiwiZXhwIjoxNzY5OTYxODIwLCJpc3MiOiJNeUF3ZXNvbWVBcHAiLCJhdWQiOiJNeUF3ZVNvbWVBdWRpZW5jZSJ9.3DXSKyuT-dvK_EY5rXaUCyZvAXYxZOUmrLtmOQytJV2URW7IU5iYP7s5XZmHVpcrScPuIRhhEo1BS3Z4GmulfQ"]]
    )),
    LL= @Source[results],
Next = [limit="100", after = Source[#"paging"][#"next"][#"after"]],    result = try @LL & @GetPages(Next) otherwise @LL
in
    result

,

Fullset = GetPages([limit="100"]),
    #"Converted to Table" = Table.FromList(Fullset, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"id", "userName", "password"})
in
    #"Expanded Column1"

```


I denne opretter man en parameter i Power Bi. 
Parameter:
Opret en parameter til token
I Power BI Desktop → Transformer data → Administrer parametre → Ny parameter

Navn: JwtToken
Type: Tekst
Vilkårlig Værdi: sæt dit token fra Postman fx "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."

Klik OK

Forspørgsel til at hente userdata
```
let
    Host = "https://localhost:7279",
    QueryParams = [ page = "1", pageSize = "20" ],
    UsersResponse = Json.Document(
        Web.Contents(
            Host,
            [
                RelativePath = "api/auth/users",
                Query = QueryParams,
                Headers = [
                    Authorization = "Bearer " & JwtToken,
                    Accept = "application/json"
                ]
            ]
        )
    ),

    // Konverter listen til tabel
    UsersTable = Table.FromList(UsersResponse, Splitter.SplitByNothing(), null, null, ExtraValues.Error),

    // Expand records til kolonner
    ExpandedUsers = Table.ExpandRecordColumn(UsersTable, "Column1", {"id","userName","password"}, {"id","userName","password"})
in
    ExpandedUsers
```
