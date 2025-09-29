# Individuele inkomenstoeslag (IIT)

Voor nu ondersteunt de service alleen de IIT voor alleenstaanden. 
De service wordt in de toekomst uitgebreid met de IIT voor samenwonenden.

De IIT voor alleenstaanden bestaat uit twee stappen:

1. Het starten van de service. 

   Bij deze stap moeten drie velden worden ingevuld:
   - een geldig BSN (burgerservicenummer)
   - een clientnummer dat een vrij tekstveld is en als referentie voor de consulent is bedoeld.
   - een peildatum die bepaalt tot welke dag het onderzoek wordt toegepast.

   ## Request

   | Variable     | Type     | Verplicht? | Toelichting                                                                   |
   |--------------|----------|------------|-------------------------------------------------------------------------------|
   | bsn          | `String` | Ja         | BSN van de aanvrager                                                          |
   | peildatum    | `Date`   | Ja         | De datum waar vanuit teruggekeken wordt op de inkomstengegevens van de burger |
   | clientnummer | `String` | Ja         | Het nummer waarmee de client of casus achterhaald kan worden                  |

   Aan de hand van deze gegevens wordt een bevraging gedaan die de inkomensgegevens opvraagt die bekend zijn bij het UWV.

   **Note:** Zorg dat het clientnummer vrij is van privacy-gevoelige informatie.
   
   **Note:** De peildatum wordt voor het onderzoek altijd teruggezet naar de eerste dag van de maand van de peildatum zodat alleen hele maanden worden meegenomen.

   De response van deze stap bevat de variabelen "verschillende_verhoudingen" en "uwv_ikv_bruto_aanvrager". De "verschillende_verhoudingen" zijn bedoeld ter informatie. De data uit "uwv_ikv_bruto_aanvrager" wordt gebruikt in de volgende stap en kan eventueel aangepast worden.

   ## Response

   | Variable                    | Type    | Verplicht? | Toelichting                         |
   |-----------------------------|---------|------------|-------------------------------------|
   | verschillende_verhoudingen  | `array` | Nee        | Unieke werkgevers (afgeleid uit IKV) |
   | uwv_ikv_bruto_aanvrager     | `array` | Nee        | De inkomstenopgaven van het UWV     |

   ### verschillende_verhoudingen (object in array):

   Array van objecten die elke unieke werkgever vertegenwoordigen die is aangetroffen in `uwv_ikv_bruto_aanvrager`.
   De sleutel `organisatie` wordt gevuld met de waarde van `naam_administratieveEenheid`.
   De sleutels `datum_aanvang_inkomstenverhouding` en `datum_einde_inkomstenverhouding` komen 1-op-1 overeen met dezelfde velden in `uwv_ikv_bruto_aanvrager`.

   | Variable                          | Type     | Verplicht? | Toelichting                                                                          |
   |-----------------------------------|----------|------------|--------------------------------------------------------------------------------------|
   | organisatie                       | `string` | Nee        | Naam van de administratieve eenheid (uit `naam_administratieveEenheid`)             |
   | datum_aanvang_inkomstenverhouding | `date`   | Nee        | Eerste dag waarop de inkomstenverhouding geldig is                                   |
   | datum_einde_inkomstenverhouding   | `date`   | Nee        | Laatste dag van de inkomstenverhouding                                               |


   ### uwv_ikv_bruto_aanvrager (object in array):

   | Variable                                    | Type       | Verplicht? | Toelichting                                                                                                                                                             |
   |---------------------------------------------|------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | datum_begin_inkomstenperiode                | `date`     | Nee        | Begindatum van de datum_begin_inkomstenperiode                                                                                                                          |
   | datum_einde_inkomstenperiode                | `date`     | Nee        | De einddatum van de datum_begin_inkomstenperiode                                                                                                                        | 
   | code_soort_inkomstenverhouding              | `string`   | Nee        | De code ter aanduiding van het soort inkomstenverhouding                                                                                                                | 
   | omschrijving_code_soort_inkomstenverhouding | `string`   | Nee        | Omschrijving van de code ter aanduiding van het soort inkomstenverhouding                                                                                               | 
   | naam_administratieveEenheid                 | `string`   | Nee        | Een administratieve eenheid is een door de belastingdienst en UWV erkende organisatorische eenheid, die door een inhoudingsplichtige wordt uitgever                     |
   | datum_aanvang_inkomstenverhouding           | `date`     | Nee        | De datum van de eerste dag waarop de inkomstenverhouding geldig is                                                                                                      |
   | datum_einde_inkomstenverhouding             | `date`     | Nee        | Datum laatste dag van de inkomstenverhouding                                                                                                                            |
   | datum_aanvang_inkomstenopgave               | `date`     | Nee        | De datum van de eerste dag van de inkomstenopgave                                                                                                                       |
   | datum_einde_inkomstenopgave                 | `date`     | Nee        | De datum van de laatste dag van de inkomstenopgave                                                                                                                      | 
   | loon_sv                                     | `currency` | Nee        | Het bedrag dat in totaal in het aangiftetijdvak door de administratieve eenheid is vastgesteld aan loon voor de werknemersverzekeringen                                 |
   | inhouding_lbph_totaal                       | `currency` | Nee        | Het bedrag dat in totaal in het aangiftetijdvlak aan loonbelasting en premie volksverzekeringen is ingehouden                                                           |
   | inhoudingen_lbph_bijzonder                  | `currency` | Nee        | Dat deel van het loon LB/PH dat is belast onder toepassing van de tabel bijzondere beloningen                                                                           |
   | loon_lbph_totaal                            | `currency` | Nee        | Het bedrag dat in totaal in het aangiftetijdvak door de administratieve eenheid is vastgesteld aan loon dat onderworpen is aan loonbelasting/premie volksverzekeringen. |
   | inhouding_zvw_premie                        | `currency` | Nee        | Het bedrag dat in het aangiftetijdvak door de administratieve eenheid als werkgeversheffing voor de Zvw wordt afgedragen.                                               |
   | waarde_gebruik_auto                         | `currency` | Nee        | Het bedrag van de forfaitaire waarde van het privégebruik van een aan de werknemer ter beschikking gestelde auto vóór aftrek van de eigen bijdrage van de werknemer.    |
   | code_loontijdvak                            | `integer`  | Nee        | Een code die aangeeft in welk tijdvak het loon uitbetaald is                                                                                                            |
   | code_loonbelastingtabel                     | `string`   | Nee        | Een code waarmee aangegeven wordt welke tabel voor de inhouding van LB/PH is toegepast                                                                                  |
   | omschrijving_code_loonbelastingtabel        | `string`   | Nee        | Omschrijving van een code waarmee aangegeven wordt welke tabel voor de inhouding van LB/PH is toegepast                                                                 |
   | opgb_recht_vakantietoeslag                  | `currency` | Nee        | Het bedrag van de vakantietoeslag die tot dan toe is opgebouwd.                                                                                                         |
   | vakantietoeslag                             | `currency` | Nee        | Het bedrag van de daadwerkelijke uitbetaling van de vakantietoeslag.                                                                                                    |
   | loonheffingskorting_toegepast               | `string`   | Nee        | Een indicatie of er op dit inkomen loonheffingskorting is toegepast                                                                                                     |

2. Het vervolgen van de service (stap 2).

   In deze stap moet de variabele `uwv_ikv_bruto_aanvrager` verplicht worden meegegeven. Dit is een array met de inkomensgegevens die in stap 1 zijn opgehaald en eventueel aangepast kunnen zijn.

   ## Request
   | Variable                  | Type      | Verplicht? | Toelichting                                                                 |
   |---------------------------|-----------|------------|-----------------------------------------------------------------------------|
   | uwv_ikv_bruto_aanvrager   | `array`   | Ja         | Inkomensgegevens van de aanvrager, zoals opgehaald in stap 1                |

   Zie voor de structuur van `uwv_ikv_bruto_aanvrager` [hierboven](#uwv_ikv_bruto_aanvrager-object-in-array).

   ## Response
   De response van deze stap is de uitkomst van de toetsing en bevat de volgende variabelen:

   | Variable                                    | Type      | Verplicht? | Toelichting                                                                 |
   |---------------------------------------------|-----------|------------|-----------------------------------------------------------------------------|
   | algemene_gegevens_referteperiode            | `object`  | Nee        | Algemene gegevens over de referteperiode van de toetsing                     |
   | uwv_ikv_netto_jaarlijks_aanvrager_1         | `array`   | Nee        | Jaarlijkse netto-inkomensgegevens van de aanvrager over de referteperiode  |
   | uwv_ikv_netto_maandelijks_aanvrager_1       | `array`   | Nee        | Maandelijkse netto-inkomensgegevens van de aanvrager over de referteperiode |
   | uwv_ikv_ongeschikte_inkomstenopgaven_aanvrager_1 | `array` | Nee        | Inkomensopgaven die niet geschikt zijn voor de toetsing                      |

   ### Toelichting op de response-velden:
   - `algemene_gegevens_referteperiode`: bevat informatie over de periode waarover de toetsing is uitgevoerd.
   - `uwv_ikv_netto_jaarlijks_aanvrager_1`: bevat per jaar het totaal en het gemiddelde netto inkomen.
   - `uwv_ikv_netto_maandelijks_aanvrager_1`: bevat per maand het netto inkomen.
   - `uwv_ikv_ongeschikte_inkomstenopgaven_aanvrager_1`: bevat inkomensopgaven die buiten de referteperiode vallen of niet bruikbaar zijn voor de toetsing.

   ### algemene_gegevens_referteperiode (object)

   | Variable                | Type       | Verplicht? | Toelichting                                                      |
   |-------------------------|------------|------------|------------------------------------------------------------------|
   | aantal_lege_maanden     | `integer`  | Nee        | Aantal maanden zonder inkomensgegevens in de referteperiode      |
   | gem_maandinkomen        | `currency` | Nee        | Het gemiddelde maandinkomen over de referteperiode               |

   ### uwv_ikv_netto_jaarlijks_aanvrager_1 (array)

   | Variable                          | Type       | Verplicht? | Toelichting                                                      |
   |-----------------------------------|------------|------------|------------------------------------------------------------------|
   | maandelijks_gemiddelde_netto_inkomen | `currency` | Nee        | Het gemiddelde netto inkomen per maand over het jaar             |
   | gemiddelde_bijstandsnorm          | `currency` | Nee        | De gemiddelde bijstandsnorm voor het betreffende jaar            |
   | aantal_geldige_maanden            | `integer`  | Nee        | Aantal maanden met geldige inkomensgegevens in het jaar          |
   | datum_begin_jaar                  | `date`     | Nee        | De eerste dag van het jaar waarover de berekening is uitgevoerd  |
   | datum_einde_jaar                  | `date`     | Nee        | De laatste dag van het jaar waarover de berekening is uitgevoerd |
   | jaarlijks_totaal_netto_inkomen    | `currency` | Nee        | Het totale netto inkomen over het hele jaar                      |
   | berekening_volledig               | `string`   | Nee        | Indicatie of de berekening volledig is uitgevoerd                |
   | beschrijving_volledigheid         | `string`   | Nee        | Beschrijving van de volledigheid van de berekening               |

   ### uwv_ikv_netto_maandelijks_aanvrager_1 (array)

   | Variable                          | Type       | Verplicht? | Toelichting                                                      |
   |-----------------------------------|------------|------------|------------------------------------------------------------------|
   | netto_inkomen_bijzonder           | `currency` | Nee        | Netto inkomen uit bijzondere beloningen                          |
   | loon_lbph_totaal                  | `currency` | Nee        | Totaal loon onderworpen aan loonbelasting/premie volksverzekeringen |
   | aantal_geldige_maanden            | `integer`  | Nee        | Aantal geldige maanden voor deze berekening                      |
   | nettoloon_totaal                  | `currency` | Nee        | Totaal nettoloon na aftrek van alle inhoudingen                  |
   | inhouding_zvw_premie_totaal       | `currency` | Nee        | Totaal ingehouden zorgverzekeringswet premie                     |
   | netto_inkomen_normaal             | `currency` | Nee        | Netto inkomen uit regulier tijdvakloon                           |
   | jgk                               | `currency` | Nee        | Optionele netto tegemoetkoming voor Wajong uitkeringen           |
   | loon_lbph_normaal                 | `currency` | Nee        | Loon onderworpen aan normaal tarief LB/PH                        |
   | inhouding_lbph_normaal            | `currency` | Nee        | Inhouding loonbelasting/premie volksverzekeringen normaal tarief |
   | nettoloon_bijzonder               | `currency` | Nee        | Nettoloon uit bijzondere beloningen                              |
   | woonleefsituatie                  | `string`   | Nee        | Woon- en leefsituatie van de aanvrager                           |
   | toetsingsnorm_procentueel         | `percentage` | Nee        | Toetsingsnorm als percentage van de bijstandsnorm                |
   | inhouding_lbph_bijzonder          | `currency` | Nee        | Inhouding loonbelasting/premie volksverzekeringen bijzonder tarief |
   | inhouding_wgawhk_premie_normaal   | `currency` | Nee        | Inhouding WGA/WHK premie normaal tarief                          |
   | anw_tegemoetkoming                | `currency` | Nee        | Optionele netto tegemoetkoming voor ANW uitkeringen              |
   | waarde_gebruik_auto               | `currency` | Nee        | Waarde van het privégebruik van een bedrijfsauto                 |
   | inhouding_wgawhk_premie_totaal    | `currency` | Nee        | Totaal ingehouden WGA/WHK premie                                 |
   | loon_lbph_bijzonder               | `currency` | Nee        | Loon onderworpen aan bijzonder tarief LB/PH                      |
   | datum_aanvang_inkomstenopgave     | `date`     | Nee        | Begindatum van de inkomstenopgave                                |
   | netto_inkomen_procentueel         | `percentage` | Nee        | Netto inkomen als percentage van de bijstandsnorm                |
   | inhouding_lbph_totaal             | `currency` | Nee        | Totaal ingehouden loonbelasting/premie volksverzekeringen        |
   | datum_einde_inkomstenopgave       | `date`     | Nee        | Einddatum van de inkomstenopgave                                 |
   | netto_inkomen_totaal              | `currency` | Nee        | Totaal netto inkomen                                             |
   | inhouding_wgawhk_premie_bijzonder | `currency` | Nee        | Inhouding WGA/WHK premie bijzonder tarief                        |
   | berekening_volledig               | `string`   | Nee        | Indicatie of de berekening volledig is uitgevoerd                |
   | nettoloon_normaal                 | `currency` | Nee        | Nettoloon uit regulier tijdvakloon                               |
   | toetsingsinkomen                  | `currency` | Nee        | Het inkomen dat wordt gebruikt voor de toetsing                  |
   | inhouding_zvw_premie_bijzonder    | `currency` | Nee        | Inhouding zorgverzekeringswet premie bijzonder tarief            |
   | inhouding_zvw_premie_normaal      | `currency` | Nee        | Inhouding zorgverzekeringswet premie normaal tarief              |

   ### uwv_ikv_ongeschikte_inkomstenopgaven_aanvrager_1 (array)

   | Variable                          | Type       | Verplicht? | Toelichting                                                      |
   |-----------------------------------|------------|------------|------------------------------------------------------------------|
   | code_loonbelastingtabel           | `string`   | Nee        | Code waarmee aangegeven wordt welke tabel voor de inhouding van LB/PH is toegepast |
   | code_loontijdvak                  | `integer`  | Nee        | Code die aangeeft in welk tijdvak het loon uitbetaald is         |
   | code_soort_inkomstenverhouding    | `string`   | Nee        | Code ter aanduiding van het soort inkomstenverhouding            |
   | datum_aanvang_inkomstenopgave     | `date`     | Nee        | De datum van de eerste dag van de inkomstenopgave                |
   | datum_aanvang_inkomstenverhouding | `date`     | Nee        | De datum van de eerste dag waarop de inkomstenverhouding geldig is |
   | datum_begin_inkomstenperiode      | `date`     | Nee        | Begindatum van de inkomstenperiode                               |
   | datum_einde_inkomstenopgave       | `date`     | Nee        | De datum van de laatste dag van de inkomstenopgave               |
   | datum_einde_inkomstenperiode      | `date`     | Nee        | De einddatum van de inkomstenperiode                             |
   | datum_einde_inkomstenverhouding   | `date`     | Nee        | Datum laatste dag van de inkomstenverhouding                     |
   | inhouding_lbph_totaal             | `currency` | Nee        | Het bedrag dat in totaal in het aangiftetijdvlak aan loonbelasting en premie volksverzekeringen is ingehouden |
   | inhouding_zvw_premie              | `currency` | Nee        | Het bedrag dat in het aangiftetijdvak door de administratieve eenheid als werkgeversheffing voor de Zvw wordt afgedragen |
   | loon_lbph_bijzonder               | `currency` | Nee        | Het bedrag waarover, door inhoudingsplichtige in het aangiftetijdvak voor de inkomstenverhouding, de loonbelasting/premie is berekend conform bijzonder tarief |
   | loon_lbph_totaal                  | `currency` | Nee        | Het bedrag dat in totaal in het aangiftetijdvak door de administratieve eenheid is vastgesteld aan loon dat onderworpen is aan loonbelasting/premie volksverzekeringen |
   | loon_sv                           | `currency` | Nee        | Het bedrag dat in totaal in het aangiftetijdvak door de administratieve eenheid is vastgesteld aan loon voor de werknemersverzekeringen |
   | loonheffingskorting_toegepast     | `string`   | Nee        | Een indicatie of er op dit inkomen loonheffingskorting is toegepast |
   | naam_administratieveEenheid       | `string`   | Nee        | Een administratieve eenheid is een door de belastingdienst en UWV erkende organisatorische eenheid, die door een inhoudingsplichtige wordt uitgegeven |
   | opgb_recht_vakantietoeslag        | `currency` | Nee        | Het bedrag van de vakantietoeslag die tot dan toe is opgebouwd   |
   | vakantietoeslag                   | `currency` | Nee        | Het bedrag van de daadwerkelijke uitbetaling van de vakantietoeslag |
   | waarde_gebruik_auto               | `currency` | Nee        | Het bedrag van de forfaitaire waarde van het privégebruik van een aan de werknemer ter beschikking gestelde auto vóór aftrek van de eigen bijdrage van de werknemer |
   | woonleefsituatie                  | `string`   | Nee        | Woon- en leefsituatie van de aanvrager                           |
