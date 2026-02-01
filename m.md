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
