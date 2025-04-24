# Bronnen bevragen

`Bronnen Bevragen` stap.

**Request**

| Variable    | Type     | Verplicht? | Toelichting                            |
|-------------|----------|------------|----------------------------------------|
| referenceID | `String` | Ja         | xxx                                    |
| requestType | `String` | Ja         | xxx                                    |
| datBPeriode | `String` | Ja         | peildatum - 3 jaar                     | 
| datEPeriode | `String` | Ja         | peildatum                              |
| oin         | `String` | Ja         | OIN nummer van de bevragende instantie |

**Response**

| Variable                  | Type    | Verplicht? | Toelichting                                                     |
|---------------------------|---------|------------|-----------------------------------------------------------------|
| uwv_ikv                   | `array` | Nee        | IVK bevraging van het UWV                                       |
| brp_domicilie             | `array` | Nee        | domicilie van de burger                                         |
| brp_huwelijk              | `array` | Nee        | (Historisch-) huwelijk van de burger                            |
| brp_partner               | `array` | Nee        | gegevens van de partner van de burger                           |
| brp_overige_inwonende     | `array` | Nee        | de gegevens van de overige inwonende op het adres van de burger |
| brp_gezamelijk_huishouden | `array` | Nee        | de gegevens van het huishouden van de burger                    |