# VWI API voor ontwikkelaars


## GraphQL-toegang voor organisaties met ROLE_API

Dit document vat samen welke GraphQL-requests beschikbaar zijn voor organisaties waarvan gebruikers de rol `ROLE_API` hebben in het vwi-apps platform.

## Aannames

- De feitelijke permissies worden afgedwongen door privacyregels en security-checks in de platform servicelaag.
- Gebruikers met `ROLE_API` hebben voor procesuitvoering/-opvraging vergelijkbare rechten als `ROLE_USER`, maar zijn uitgesloten van administratieve CRUD-operaties op entiteiten zoals Organisations, Users, ServiceCategories, Services, Parameters en JSON Schemas.
- Alle queries en resultaten zijn door privacyfilters beperkt tot de organisatie van de gebruiker.

## Queries beschikbaar voor ROLE_API

- `users`: Toegestaan, beperkt tot gebruikers binnen dezelfde organisatie.
- `organisations`: Toegestaan, beperkt tot de eigen organisatie.
- `services`: Toegestaan, gefilterd op services die zijn toegewezen aan de organisatie van de gebruiker en/of categorieën die zijn toegewezen aan de organisatie.
- `serviceCategories`: Toegestaan, gefilterd op categorieën die zijn toegewezen aan de organisatie van de gebruiker.
- `parameters`: Toegestaan, gescopeerd door organisatie-/service-/categoriefilters.
- `jsonSchemas` en `jsonSchemaVersions`: Lezen toegestaan (geen create/update/delete) wanneer gekoppeld aan toegankelijke services/categorieën; onderhevig aan organisatie-/categoriefilters.
- `node` / `nodes`: Volgt dezelfde privacyfilters als hierboven.

Verwijzingen

- Query-definities: `svc/platform/transport/ent/schema/ent.graphql`
- Privacyregels: `svc/platform/internal/security/rules.go`
- Security-checks: `svc/platform/internal/security/security.go`

## Mutaties beschikbaar voor ROLE_API

Administratieve mutaties zijn niet toegestaan voor `ROLE_API` (create/update/delete voor Organisations, Users, ServiceCategories, Services, Parameters, JSON Schemas/Versions worden door privacyregels geblokkeerd).

De volgende uitvoeringsmutaties zijn toegestaan (mits de organisatie toegang heeft tot de service en/of het proces):
- `startCamundaProcess(input: { serviceID })`: Toegestaan als de organisatie toegang heeft tot de `Service` en de bijbehorende `ServiceCategory`.
- `completeServiceTask(input: { referenceID, taskID, variables })`: Toegestaan als de gebruiker bij dezelfde organisatie hoort als de `Process`.
- `executeService(input: { serviceID, variables })`: Toegestaan met dezelfde controles als `startCamundaProcess`.
- `executeTask(input: { referenceID, taskID, variables })`: Toegestaan met dezelfde controles als `completeServiceTask`.

## Opmerkingen

- Organisatiescoping en categorie-/service-toegang bepalen de zichtbaarheid; `ROLE_API`-gebruikers kunnen services en categorieën zien die zijn toegewezen aan hun organisatie.
- `ROLE_API`-gebruikers kunnen geen admin-CRUD uitvoeren, ook niet binnen de eigen organisatie.
