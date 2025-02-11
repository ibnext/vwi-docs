# Verzoekservices werk en inkomen
*versie: 0.6.0*

...


Dit zijn de huidige en toekomstige beschikbare services:
| Service   | Beschikbaar         | Documentatie
| --------  | -------             | ---------
| IIT       | Ja                  | [docs](./services/IIT/README.md)
| ALO       | In ontwikkeling     | [docs](./services/ALO/README.md)
| ...       | ..                  |

**Generieke bouwblokken**

De beschikbare services zijn opgebouwd uit verschillende bouwblokken, sommige van deze blokken zijn ook beschikbaar als losse service. Welke dit zijn en hoe deze gebruikt kunnen worden staat [hier](./services/generiek/README.md).

## Doelgroep
Gemeentes en softwareleveranciers die werken in opdracht van een gemeente. 

## Aansluiten
Indien u of uw softwareleverancier van de dienst gebruik wil maken, kunt u bij de Servicedesk van het Inlichtingenbureau een aanvraagformulier verzoeken. Geef hierbij aan of u gebruik wilt maken van de API of het Portaal of beide en ook welke services binnen VWI u wilt afnemen. 

### Diginetwerk
Om Aan te sluiten of gebruik te maken van de VWI services is er een [Diginetwerk](https://www.logius.nl/domeinen/infrastructuur/diginetwerk) verbinding nodig.
```
(o_o)        [             ]  ----> [ Portaal ]
  |     ---> | Diginetwerk |
 / \         [             ]  ----> [ API ]
```

Nadat de diginetwerk verbinding tot stand is gebracht moet de juiste authenticate methode gebruikt worden afhankelijk van de gekozen route. 

### Portaal
Voor het webportaal van VWI dient u te beschikken over een e-herkenning account. Nadat uw organisatie is geregistreerd bij VWI kunt u met behulp van uw e-herkenning account inloggen.


### API
Endpoint: <API URL>/vwi\
Type: GraphQL\
Methode: POST

Voor het gebruik van de API is een `PKI-overheids` certificaat nodig, deze dient bij elk verzoek meegestuurd te worden. Verzoeken zonder geldig certificaat worden geblokkeerd. 

Naast een geldig certifcaat moet er ook altijd een geldige API-key worden meegegeven in de `subscription-key` header. Deze API-Key wordt aangeleverd door het Inlichtingenbureau na het aanmelden van uw organisatie.  

De API van VWI maakt gebruik van GraphQL, de URL van de API inclusief het schema worden ter beschikking gesteld na het aanmelden van uw organisatie.

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

Vervolgens kan u een service uitvoeren, variables die nodig zijn verschillen per service en zijn terug te vinden in de documentatie van de specifieke service. 

Hieronder nemen we een voorbeeld service (PING PONG), de documentatie van deze service geeft aan dat de variable `name` verplicht is. 

**Request**
| Variable  | Type         | Verplicht?  | Toelichting         |
| --------  | -------      | --------- | -------               |
| name      | `String`     | Ja        | Naam van de aanvrager |

**Response**
| Variable  | Type                   | Verplicht?  | Toelichting                       |
| --------  | -------                | ---------  | -------                            |
| results   | `PingPongResponse`     | Nee        | Resultaat van het Ping Pong proces |

**PingPongResponse**
| Variable  | Type                   | Verplicht?  | Toelichting                       |
| --------  | -------                | ---------  | -------                            |
| name      | `String`     | Nee        | Naam van Pong |
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
Een service kan bestaan uit één of meerdere stappen, dit is terug te vinden in de documentatie van de service. Om de volgende stap uit te voeren kan de onderstaande voorbeeld mutatie gebruikt worden. 

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


Indien de response van `executeService` of van `executeTask` de variabele `is_last_step` bevat dan is het proces klaar en is het resultaat het eind resultaat. 

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



    

