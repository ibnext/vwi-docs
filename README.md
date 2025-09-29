# Verzoekservices werk en inkomen

*Versie: 0.10.2*

# Inhoudsopgave

1. [VWI API and Portaal](README.md)
    - [Doelgroep](README.md#doelgroep)
    - [Aansluiten](README.md#aansluiten)
        - [Diginetwerk](README.md#diginetwerk)
        - [Portaal](README.md#portaal)
        - [API](README.md#api)
        - [Service start, vervolg en einde](README.md#service-start-vervolg-en-einde)
            - [Beschikbare services opvragen](README.md#beschikbare-services-opvragen)
            - [Service starten](README.md#service-starten)
            - [Service vervolgen](README.md#service-vervolgen)
            - [Response object](README.md#response-object)
            - [Service einde](README.md#service-einde)
    - [Services](README.md#services)
    - [Lees verder](README.md#lees-verder)
        - [Technische documentatie GraphQL API](./vwi-graphql-api.md)
        - [Voorbeeld API gebruik](./voorbeeld-api-gebruik.md)

Dit zijn de huidige en toekomstige beschikbare services:

| Service                     | Afkorting | Beschikbaar     | Documentatie                     |
|-----------------------------|-----------|-----------------|----------------------------------|
| Individuele Inkomenstoeslag | IIT       | Ja              | [Docs](./services/IIT/README.md) |
| Algemene Bijstand           | AB        | In ontwikkeling | [Docs](./services/AB/README.md)  |

## Doelgroep

Gemeentes en softwareleveranciers die werken in opdracht van een gemeente.

## Aansluiten

Indien u of uw softwareleverancier van de dienst gebruik wil maken, kunt u bij de Servicedesk van BIDN
een aanvraagformulier verzoeken. Geef hierbij aan of u gebruik wilt maken van de API of het Portaal of beide en ook
welke services binnen VWI u wilt afnemen.

### Diginetwerk

Om aan te sluiten of gebruik te maken van de VWI services is er
een [Diginetwerk](https://www.logius.nl/domeinen/infrastructuur/diginetwerk) verbinding nodig.

```
(o_o)        [             ]  ----> [ Portaal ]
  |     ---> | Diginetwerk |
 / \         [             ]  ----> [ API ]
```

Nadat de diginetwerk verbinding tot stand is gebracht moet de juiste authenticate methode gebruikt worden afhankelijk
van de gekozen route.

### Portaal

Voor het webportaal van VWI dient u te beschikken over een e-herkenning account. Nadat uw organisatie is geregistreerd
bij VWI kunt u met behulp van uw e-herkenning account inloggen.

### API

Endpoint: <API URL>/vwi\
Type: GraphQL\
Methode: POST

Om toegang to de VWI API te krijgen, is het volgende nodig:
1. Een `PKI-overheids` certificaat. Deze dient bij elk verzoek meegestuurd te worden. 
Verzoeken zonder geldig certificaat worden geblokkeerd.
1. Een gebruiker met toegang API-toegang.
1. Een geldige API-key die wordt meegegeven in de `subscription-key` header.

> Zowel de API-gebruiker als API-key worden door BIDN geleverd.

De API van VWI maakt gebruik van GraphQL, de URL van de API inclusief het schema worden ter beschikking gesteld na het
aanmelden van uw organisatie.

Ook zal het GraphQL schema geleverd worden voor de definitie van de API.

#### Service start, vervolg en einde

Alle services bestaan uit 1 of meerdere stappen. 

### Beschikbare services opvragen
Gebruik de `query Services` om een lijst van beschikbare services op te vragen voordat u een service start.

### Service starten
Elke service moet worden gestart met de `mutation ExecuteService` query.

### Service vervolgen
Alle opeenvolgende taken worden aangeroepen met behulp van de `mutation ExecuteTask` query.

### Response object
Beide mutations retourneren het `ExecuteServicePayload` object dat de volgende velden bevat:
- `referenceID`: Dit ID blijft gedurende het hele proces hetzelfde
- `results`: Een object van type `ServiceTask` dat bevat:
  - `variables`: Bevat alle data voor de huidige stap
  - `ID`: Representeert het ID voor de volgende stap en moet worden gebruikt als de `taskID` input voor de volgende `ExecuteTask` aanroep

### Service einde
De laatste stap wordt aangegeven wanneer de `variables` lijst een object bevat met de key `is_final_step` en waarde `true`. Dit betekent dat het proces/service is voltooid en er geen stappen meer over zijn. 

## Lees verder
* [Technische documentatie GraphQL API](./vwi-graphql-api.md)
* [Voorbeeld API gebruik](./voorbeeld-api-gebruik.md) voor uitgebreide voorbeelden van hoe de VWI API gebruikt kan worden. 

## Services

Op dit moment biedt BIDN de volgende service(s) aan:

* [Individuele inkomenstoeslag (IIT)](./services/IIT/README.md) voor meer informatie over de IIT service.
