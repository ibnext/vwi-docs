# Bronnen bevragen

`Bronnen Bevragen` stap.

## Request

| Variable     | Type     | Verplicht? | Toelichting                                                                                                                                                                                                          |
|--------------|----------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| referenceID  | `String` | Ja         | xxx                                                                                                                                                                                                                  |
| requestType  | `String` | Ja         | xxx                                                                                                                                                                                                                  |
| datBPeriode  | `String` | Ja         | peildatum - 3 jaar                                                                                                                                                                                                   | 
| datEPeriode  | `String` | Ja         | peildatum                                                                                                                                                                                                            |
| oin          | `String` | Ja         | OIN nummer van de bevragende instantie                                                                                                                                                                               |
| testMode     | `String` | Nee        | Wanneer de bkwi-mock bronbevraging gewenst is. Geldige waarden zijn <br/>"standard" voor een standaard response of "randomise" voor willekeurige data.<br/>Bij "none" en iedere andere waarde staat de testMode uit. |
| bkwi_verzoek | `String` | Ja         | Zie [BKWI verzoek](#bkwi-verzoek) voor geldige waarden.                                                                                                                                                              |                                                                                                                                                                                                              |

### BKWI verzoek

Hieronder een overzicht van de verschillende BKWI verzoeken die gedaan kunnen worden.

| BKWI verzoek               | Toelichting                                                                  |
|----------------------------|------------------------------------------------------------------------------|
| uwv_ikv                    | De inkomstenopgaven van het UWV.                                             |
| brp_domicilie              | De gegevens van de burger die op het adres van de burger staan ingeschreven. |
| brp_huwelijk               | De gegevens van de (historisch) huwelijk van de burger.                      |
| brp_partner                | De gegevens van de partner van de burger.                                    |
| brp_overige_inwonende      | De gegevens van de overige inwonende op het adres van de burger.             |
| brp_gezamenlijk_huishouden | De gegevens van het huishouden van de burger.                                |

## Response

| Variable                   | Type    | Verplicht? | Toelichting                                                     |
|----------------------------|---------|------------|-----------------------------------------------------------------|
| uwv_ikv                    | `array` | Nee        | IVK bevraging van het UWV                                       |
| brp_domicilie              | `array` | Nee        | domicilie van de burger                                         |
| brp_huwelijk               | `array` | Nee        | (Historisch-) huwelijk van de burger                            |
| brp_partner                | `array` | Nee        | gegevens van de partner van de burger                           |
| brp_overige_inwonende      | `array` | Nee        | de gegevens van de overige inwonende op het adres van de burger |
| brp_gezamenlijk_huishouden | `array` | Nee        | de gegevens van het huishouden van de burger                    |

## Errors

Ieder BKWI verzoek kan een eigen error response geven. De error komt terug in de `JSON_DATA` onder de key
`bronbevraging_error`. Hieronder een overzicht van de error responses per type BKWI verzoek.

| BKWI verzoek               | Toelichting                                                                                            | Error response                                       |
|----------------------------|--------------------------------------------------------------------------------------------------------|------------------------------------------------------|
| alle                       | Indien de bronbevraging mislukt door een technische reden                                              | `Bronbevraging mislukt`                              |
| uwv_ikv                    | De bronbevraging voor UWV gegevens is gelukt maar zonder resultaten                                    | `Geen inkomstengegevens bekend bij UWV`              |
| brp_domicilie              | De bronbevraging voor domicilie gegevens is gelukt maar zonder resultaten                              | `Geen domiciliegegevens bekend bij BRP`              |
| brp_huwelijk               | De bronbevraging voor gegevens over het huwelijk is gelukt maar zonder resultaten                      | `Geen huwelijksgegevens bekend bij BRP`              |
| brp_partner                | De bronbevraging voor gegevens over de partner van de aanvrager is gelukt maar zonder resultaten       | `Geen gegevens van partner bekend bij BRP`           |
| brp_overige_inwonende      | De bronbevraging voor overige inwonende op het adres van de aanvrager is gelukt maar zonder resultaten | `Geen gegevens van overige inwonende bekend bij BRP` |
| brp_gezamenlijk_huishouden | De bronbevraging voor gegevens over het gezamenlijk huishouden is gelukt maar zonder resultaten        | `Geen gegevens gezamenlijk huishouden gevonden`      |

Naast de error response wordt ook de `ReferenceID` bij het bericht gevoegd.

Afhankelijk van het type bronbevraging en de error, kan het proces verder gaan of stoppen. Dit is afhankelijk van
de soort verzoekservice.
