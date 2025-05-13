# Vermogensgegevens aanvullen

`Vermogensgegevens aanvullen` stap.

## Request

| Variable            | Type      | Verplicht? | Toelichting                                        |
|---------------------|-----------|------------|----------------------------------------------------|
| woz_waarde          | `integer` | Nee        | WOZ waarde van de OZ van de burger                 |
| waarde_voertuig     | `integer` | Nee        | De waarde van het voertuig van de burger           |
| liquide_middelen    | `integer` | Nee        | de waarde van de liquide middelen van de burger    |
| overige_bezittingen | `integer` | Nee        | de waarde van de overige bezittingen van de burger |
| hypotheekschuld     | `integer` | Nee        | De waarde van de hypotheekschuld van de burger     |
| studieschuld        | `integer` | Nee        | de waarde van de studieschuld van de burger        |
| overige_schulden    | `integer` | Nee        | de waarde van de overige schulden van de burger    |

Het resultaat van de vermogenstoets wordt toegevoegd aan de resultaten. Deze komen terug in de variabele 
`uitkomst_toets` in de stap [resultaat terugsturen](../resultaat-terugsturen/README.md).
