# Voorbeeld API gebruik

Om te testen of de connectie werkt kan de volgende query gebruikt worden:

```
headers:
Content-Type: application/json
subscription-key: <api key>

query Me {
    me {
        user {
            id
            name
            email
            organisation{
                id
                name
                kvk
            }
        }
    }
}
```

Hierna kunnen alle beschikbare services voor de ingelogde organisatie opgehaald worden als volgt:

```
query Services {
    services {
        pageInfo{
            hasNextPage
            hasPreviousPage
        }
        totalCount
        edges{
            cursor
            node{
                id,
                name,
                description
                camundaid
                category {           
                    id            
                    name
                }
            }
        }
        
    }
}
```

Vervolgens kan u een service uitvoeren, variables die nodig zijn verschillen per service en zijn terug te vinden in de
documentatie van de specifieke service.

Hieronder nemen we een voorbeeld service (PING PONG), de documentatie van deze service geeft aan dat de variable `name`
verplicht is.

**Request**

| Variable | Type     | Verplicht? | Toelichting           |
|----------|----------|------------|-----------------------|
| name     | `String` | Ja         | Naam van de aanvrager |

**Response**

| Variable | Type               | Verplicht? | Toelichting                        |
|----------|--------------------|------------|------------------------------------|
| results  | `PingPongResponse` | Nee        | Resultaat van het Ping Pong proces |

**PingPongResponse**

| Variable | Type     | Verplicht? | Toelichting   |
|----------|----------|------------|---------------|
| name     | `String` | Nee        | Naam van Pong |

```
mutation ExecuteService {
    executeService(input: {serviceID: "12884901888", variables: {
        name: "name", value: "value"
    }}) {        
        referenceID
        results{
            id
            variables{
                name
                value
            }           
        }
    }
}
```

Het resultaat ziet er dan uit als volgt:

```
{
    "data": {
        "executeService": {
            "referenceID": "b50de104-5367-41a3-ba27-5ccbdff24174",
            "results": {
                id: "123",
                variables: [
                    {
                        "name": "results",
                        "value": "{'name': 'pong'}"
                    }
                ]
            }
        }
    }
}
```

De `referenceID` blijgt voor de rest van de service en vervolgstappen hetzelfde.  
De `results.ID` daarentegen, wordt gebruikt om de volgende stap uit te kunnen voeren.
Een service kan bestaan uit één of meerdere stappen en dit is terug te vinden in de documentatie van de service. Om de
volgende stap uit te voeren kan de onderstaande voorbeeld mutatie gebruikt worden.

```
mutation ExecuteTask {
    executeTask(input: {
        referenceID: "b50de104-5367-41a3-ba27-5ccbdff24174", 
        taskID: "123"
        variables: {
            name: "key2", value: "value2"
        }
    }) {        
        results{
            id
            variables{
                name
                value
            }           
        }
    }
}
```

Indien de response van `executeService` of van `executeTask` de variabele `is_last_step` bevat dan is het proces klaar
en is het resultaat het eind resultaat.

```
{
    "data": {
        "executeService": {
            "referenceID": "b50de104-5367-41a3-ba27-5ccbdff24174",
            "results": {
                id: "123",
                variables: [
                    {
                        "name": "results",
                        "value": "{}"
                    },
                    {
                        "name": "is_last_step",
                        "value": "true"
                    }
                ]
            }
        }
    }
}
```

Met deze stappen kunnen alle services in VWI, waar de organisatie toegang tot heeft, uitegevoerd worden.
