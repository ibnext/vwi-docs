# Verzoekservices werk en inkomen

*Versie: 0.10.2*

# Inhoudsopgave

1. [Generieke bouwblokken](services/generiek/README.md)
    - [Bronnen bevragen](services/generiek/bronnen-bevragen/README.md)
    - [Gegevens aanvullen](services/generiek/gegevens-aanvullen/README.md)
    - [Gegevens controleren](services/generiek/gegevens-controleren/README.md)
    - [Resultaat terugsturen](services/generiek/resultaat-terugsturen/README.md)
    - [Situatie bepalen](services/generiek/situatie-bepalen/README.md)

2. [VWI API and Portaal](README.md)
    - [Doelgroep](README.md#doelgroep)
    - [Aansluiten](README.md#aansluiten)
        - [Diginetwerk](README.md#diginetwerk)
        - [Portaal](README.md#portaal)
        - [API](README.md#api)
    - [Lees verder](README.md#lees-verder)
        - [Technische documentatie GraphQL API](./vwi-graphql-api.md)
        - [Voorbeeld API gebruik](./voorbeeld-api-gebruik.md)

Dit zijn de huidige en toekomstige beschikbare services:

| Service                     | Afkorting | Beschikbaar     | Documentatie                     |
|-----------------------------|-----------|-----------------|----------------------------------|
| Individuele Inkomenstoeslag | IIT       | Ja              | [Docs](./services/iit/readme.md) |
| Algemene Bijstand           | AB        | In ontwikkeling | [Docs](./services/ab/readme.md)  |

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

## Lees verder
* [Technische documentatie GraphQL API](./vwi-graphql-api.md)
* [Voorbeeld API gebruik](./voorbeeld-api-gebruik.md) voor uitgebreide voorbeelden van hoe de VWI API gebruikt kan worden. 
