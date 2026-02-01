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
    Host = #"Hubspot api",
    Source = Json.Document(
    Web.Contents(
    Host,
    [RelativePath = "crm/v3/pipelines/deals", Query = queryParams, Headers=[#"Content-Type"="application/json", Authorization="Bearer xxx"]]
    )),
    LL= @Source[results],
Next = [limit="100", after = Source[#"paging"][#"next"][#"after"]],    result = try @LL & @GetPages(Next) otherwise @LL
in
    result

,

Fullset = GetPages([limit="100"]),
    #"Converted to Table" = Table.FromList(Fullset, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"label", "displayOrder", "id", "stages", "createdAt", "updatedAt", "archived"}, {"Column1.label", "Column1.displayOrder", "Column1.id", "Column1.stages", "Column1.createdAt", "Column1.updatedAt", "Column1.archived"})
in
    #"Expanded Column1"

```
