# iWlz-adresboek

Tijdelijk alternatief voor Zorg-AB aansluiting.

## Inleiding

Binnen het iWLZ-netwerk wordt informatie gedeeld via registers die te benaderen zijn met GraphQL. Om te ontdekken hoe registers en bijbehorende services in het netwerk bereikt kunnen worden is er een adresboek nodig. Het fungeert als ware een register dat informatie bijhoudt over de verschillende gegevensdiensten die worden aangeboden door verschillende netwerkdeelnemers.

De [RFC0003 - Adresboek](https://github.com/iStandaarden/iWlz-RequestForComment/blob/main/RFC/RFC0003%20-%20Adresboek.~~md~~) is hiervoor opgesteld maar intussen is voor het Adresboek de bestaande voorziening ZorgAB voorzien. ZorgAB is alleen nu nog niet geschikt voor gebruik binnen het iWlz Netwerk maar uiteindelijk aansluiting is wel het einddoel.

Tot dat moment zal hier de lijst met benodigde endpoints worden bijgehouden in een van ZorgAB afgeleid formaat.

Het formaat (schema) is opgesteld in een eerste analyse hoe Zorg-AB ingezet kan worden in het iWlz netwerk.

## Schema

Het [schema](./src/zab_electronicservices.json) is gebaseerd op de _ElectronicService_ entiteit uit het Zorg-AB datamodel.

| ZAB Element                | Beschrijving                       | Voorbeeld                                                       |
| :------------------------- | :--------------------------------- | :-------------------------------------------------------------- |
| description                | Omschrijving service               | TEST OMGEVING Voor het raadplegen van het Wlz Indicatieregister |
| gegevensdienstId           | Identificatie van servicecomponent | CIZ_INDICATIE_TST                                               |
| weergavenaam               | Weergave naam in ZAB               | TEST OMGEVING - Wlz Indicatieregister                           |
| authorizationEndpoint      | PEP Endpoint                       |                                                                 |
| - authorizationEndpointuri | uri                                | "some.PEP.endpoint.ur."                                         |
| tokenEndpoint              | Autorisatieserver endpoint         |                                                                 |
| - tokenEndpointuri         | uri                                | "some.autorisatie.server.url"                                   |
| systeemrollen              | array van systeemrol               |                                                                 |
| "systeemrol"               | <placeholder>                      |                                                                 |
| - systeemrolcode           | Type systeem                       | TEST-RESOURCE-SERVER                                            |
| resourceEndpoint           | <placeholder>                      |                                                                 |
| - resourceEndpointuri      | uri                                | "test.sometest.url"                                             |

## Adresgegevens

De lijst is hier te vinden: [iWlz_services.json](./iWlz_services.json)

## Beheer

Neem voor het laten doorvoeren van wijzingen contact op met:

- servicedesk iStandaarden: [info_at_istandaarden.nl](info@istandaarden.nl)
- Dennis de Gouw - [@dennisdegouw](http://github.com/dennisdegouw)
- Remo van Rest - [@rvanrest](https://github.com/rvanrest)

## meer informatie:

- Actieprogramma iWlz: van keten naar netwerk: [het Actieprogramma iWlz](https://www.istandaarden.nl/iwlz/actieprogramma/index "Over Actieprogramma iWlz")
- Informatiemodel iStandaarden iWlz: [Informatiemodellen](https://informatiemodel.istandaarden.nl)
- Portaal voor iStandaarden in de Zorg en Ondersteuning: [homepagina iStandaarden](https://www.istandaarden.nl)
- Zorg-AB: https://www.vzvz.nl/diensten/gemeenschappelijke-diensten/zorg-ab
