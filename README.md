# iWlz-adresboek

# Adressenlijst

> [!TIP]
> **Ga direct naar de Adreslijst: [iWlz-adresboek-private](https://github.com/iStandaarden/iWlz-adresboek-private)**
> Om toegang te krijgen tot deze repository is:
> 1. Account nodig op Github
> 2. Toegevoegd worden als member.
> 3. Stuur daarna een bericht met vermelding van "Aanvraag toegang iWlz Adresboek private met meegeven van organisatie en githubaccount aan Dennis de Gouw - [@dennisdegouw](http://github.com/dennisdegouw) of Remo van Rest - [@rvanrest](https://github.com/rvanrest)
> 4. Je zal daarna een uitnodiging krijgen tot de private-repository

> [!IMPORTANT]
> Tijdelijk alternatief voor adresboek tot de beschikbaarheid van de generieke functie of bruikbaarheid van Zorg-AB binnen het netwerk.

- [iWlz-adresboek](#iwlz-adresboek)
  - [Inleiding](#inleiding)
  - [Opbouw adreslijst](#opbouw-adreslijst)
  - [Schema](#schema)
  - [Opbouw `gegevensdienstId`](#opbouw-gegevensdienstid)
      - [1. OrganisatieID](#1-organisatieid)
      - [2. Functionaliteit](#2-functionaliteit)
      - [3. Omgeving](#3-omgeving)
  - [Opbouw `systeemrolcode`](#opbouw-systeemrolcode)
  - [Voorbeelden](#voorbeelden)
    - [1. Voorbeeld Testomgeving Indicatieregister.](#1-voorbeeld-testomgeving-indicatieregister)
    - [2. Voorbeeld Testomgeving Bemiddelingsregister van zorgkantoor regio 5555 en 7777](#2-voorbeeld-testomgeving-bemiddelingsregister-van-zorgkantoor-regio-5555-en-7777)
    - [3. Voorbeeeld Test notificatieadres van zorgaanbieder 12121212](#3-voorbeeeld-test-notificatieadres-van-zorgaanbieder-12121212)
- [Adressenlijst](#adressenlijst)
  - [Beheer](#beheer)
- [meer informatie:](#meer-informatie)


## Inleiding

Binnen het iWLZ-netwerk wordt informatie gedeeld via registers die te benaderen zijn met GraphQL. Om te ontdekken hoe registers en bijbehorende services in het netwerk bereikt kunnen worden is er een adresboek nodig. Het fungeert als ware een register dat informatie bijhoudt over de verschillende gegevensdiensten die worden aangeboden door verschillende netwerkdeelnemers.

De [RFC0003 - Adresboek](https://github.com/iStandaarden/iWlz-RequestForComment/blob/main/RFC/RFC0003%20-%20Adresboek.~~md~~) is hiervoor opgesteld maar intussen is voor het Adresboek de bestaande voorziening ZorgAB voorzien. ZorgAB is alleen nu nog niet geschikt voor gebruik binnen het iWlz Netwerk maar uiteindelijk aansluiting is wel het einddoel.

Tot dat moment zal hier de lijst met benodigde endpoints worden bijgehouden in een van ZorgAB afgeleid formaat.

Het formaat (schema) is opgesteld in een eerste analyse hoe Zorg-AB ingezet kan worden in het iWlz netwerk.

## Opbouw adreslijst
De opbouw van de lijst is gebaseerd op de complete OAuth-flow voor het netwerk en gaat uit van alle benodigde uri voor het benaderen van één resource doel. Zie het vereenvoudigde sequentie-diagram hieronder (meer detail in de Request for Comment [RFC0014 - Functionele uitwerking aanvragen van autorisatie](https://github.com/iStandaarden/iWlz-RequestForComment/blob/main/RFC/RFC0014%20-%20Functionele%20uitwerking%20aanvragen%20van%20autorisatie.md)).

```mermaid
---
config:
  theme: neutral
---
sequenceDiagram
  actor Client
  box lightyellow nID
  participant authz as oAuth-server
  participant PEP
  end
  box lightgreen Resource
  participant Resource
  end
  Client->>+authz: request authorisation
  activate Client
  note right of authz: Autorisatie Endpoint
  authz-->>-Client: token
  deactivate Client
  Client->>+PEP: GraphQL request + token
  

  activate Client
  activate PEP
  note right of PEP: PEP Endpoint
  PEP->>PEP: validation
  PEP->>Resource: GraphQL request forward
  
  activate PEP
  activate Resource
  note right of Resource: Resource server <br/>Endpoint
  Resource-->>PEP: GraphQL response
  deactivate Resource
  deactivate PEP
  PEP-->> Client: GraphQL response forward
  deactivate PEP
  deactivate Client
```

De flow toont dat er voor het benaderen van een register, **de Resource**, langs drie componenten moet worden gegaan. Dit zijn:
1. de **Autorisatie server**: voor het vragen van de benodigde toegang.
2. de **PEP**: voor de validatie en toegang tot de resource.
3. de **Resource**: waar het register met brondata te vinden is.

Elke van deze 3 componenten hebben een eigen **endpoint**. Er worden daarom **per doel(-server) ook drie endpoints** vastgelegd.

> [!NOTE]
> Op dit moment voorziet nID binnen het iWlz-netwerkstelsel de centrale voorziening van de Autorisatie server en PEP. Hierdoor zijn de url van deze twee voorzieningen voor elk huidig doel gelijk. In de toekomst kan dit misschien veranderen en kunnen er ook andere PEP of Autorisatieserver instanties worden toegevoegd voor een doel.


## Schema

Het [schema](./src/zab_electronicservices.json) is gebaseerd op de _ElectronicService_ entiteit uit het Zorg-AB datamodel.

| ZAB Element                | Beschrijving                       | Voorbeeld                                                       |
| :------------------------- | :--------------------------------- | :-------------------------------------------------------------- |
| description                | Omschrijving service               | TEST OMGEVING Voor het raadplegen van het Wlz Indicatieregister |
| gegevensdienstId           | Identificatie van servicecomponent | CIZ_REGISTER_TST                                               |
| weergavenaam               | Weergave naam in ZAB               | TEST OMGEVING - Wlz Indicatieregister                           |
| authorizationEndpoint      | PEP Endpoint                       |                                                                 |
| - authorizationEndpointuri | uri                                | "some.PEP.endpoint.url"                                         |
| tokenEndpoint              | Autorisatieserver endpoint         |                                                                 |
| - tokenEndpointuri         | uri                                | "some.autorisatie.server.url"                                   |
| systeemrollen              | array van systeemrol               |                                                                 |
| "systeemrol"               | <placeholder>                      |                                                                 |
| - systeemrolcode           | Type systeem                       | REGISTER                                            |
| resourceEndpoint           | <placeholder>                      |                                                                 |
| - resourceEndpointuri      | uri                                | "test.sometest.url"                                             |

## Opbouw `gegevensdienstId` 

De opbouw van de `gegevensdienstID` binnen het iWlz netwerkmodel is als volgt opgebouwd: 

> **`[OrganisatieID]-[Functionaliteit]-[Omgeving]`**

#### 1. OrganisatieID

Een waarde volgens de onderstaande tabel: 

| Organisatie   | OrganisatieID gevuld met waarde     | Referentie                                                |
| :------------ | :---------------------------------- | :-------------------------------------------------------- |
| CIZ           | *CIZ*                               |
| Zorgaanbieder | de *AGB-code* van de zorgaanbieder  | [AGB-register](https://www.vektis.nl/agb-register/zoeken) |
| Zorgkantoor   | de *uzovi-code* van het zorgkantoor | [UZOVI-register](https://www.vektis.nl/uzovi-register)    |

#### 2. Functionaliteit

Een waarde uit de volgende lijst:
| Functionaliteit | Toelichting | Resource-kenmerk | 
| :-------------- | :---------- | :--- |
| REGISTER | Het betreft een adres voor het raadplegen van een register   | Register-endpoint |
| NOTIFICATIE | Het betreft een adres voor het ontvangen van een notificatie | Notificatie-endpoint |
| MELDING | Het betreft een adres voor het ontvangen van een melding | Melding-endpoint | 

#### 3. Omgeving

Een waarde uit de volgende lijst: 

| Omgeving | Toelichting |
| :--- | :--- |
| PRD | Adres betreft de Productieomgeving |
| TST | Adres betreft een Testomgeving |
| ACC | Adres betreft een Acceptatieomgeving | 

## Opbouw `systeemrolcode`

De vulling van `systeemrolcode` is bepaald door de waarden uit het onderdeel [2. Functionaliteit](#2-functionaliteit) van de `gegevensdienstID`.

## Voorbeelden 

### 1. Voorbeeld Testomgeving Indicatieregister.

json:
```json
{
  "_comment": "iWlz service lijst - versie 0.1 - 12-11-2024",
  "electronicServices": [
    {
      "description": "TEST OMGEVING Voor het raadplegen van het Wlz Indicatieregister",
      "gegevensdienstId": "CIZ_REGISTER_TST",
      "weergavenaam": "TEST OMGEVING - Wlz Indicatieregister",
      "authorizationEndpoint": {
        "_comment": "TEST OMGEVING - PEP Endpoint",
        "authorizationEndpointuri": "https://fictief.PEP.adres/"
      },
      "tokenEndpoint": {
        "_comment": "TEST OMGEVING - Autorisatieserver",
        "tokenEndpointuri": "https://fictief.autorisatie.punt"
      },
      "systeemrollen": [
        {
          "systeemrolcode": "REGISTER",
          "resourceEndpoint": {
            "_comment": "CIZ Indicatieregister",
            "resourceEndpointuri": "https://test.graphql.end.punt.ciz"
          }
        }
      ]
    }
}
```

### 2. Voorbeeld Testomgeving Bemiddelingsregister van zorgkantoor regio 5555 en 7777

json:

```json
{
  "_comment": "iWlz service lijst - versie 0.1 - 12-11-2024",
  "electronicServices": [
    {
      "description": "TEST OMGEVING Voor het raadplegen van het Bemiddelingsregister zorgkantoor 5555",
      "gegevensdienstId": "5555_REGISTER_TST",
      "weergavenaam": "TEST OMGEVING - Wlz 5555 Bemiddelingsregister",
      "authorizationEndpoint": {
        "_comment": "TEST OMGEVING - PEP Endpoint",
        "authorizationEndpointuri": "https://fictief.PEP.adres/"
      },
      "tokenEndpoint": {
        "_comment": "TEST OMGEVING - Autorisatieserver",
        "tokenEndpointuri": "https://fictief.autorisatie.punt"
      },
      "systeemrollen": [
        {
          "systeemrolcode": "REGISTER",
          "resourceEndpoint": {
            "_comment": "5555 Bemiddelingsregister",
            "resourceEndpointuri": "https://test.5555.resource.punt"
          }
        }
      ]
    },
    {
      "description": "TEST OMGEVING Voor het raadplegen van het Bemiddelingsregister zorgkantoor 5555",
      "gegevensdienstId": "7777_REGISTER_TST",
      "weergavenaam": "TEST OMGEVING - Wlz 5555 Bemiddelingsregister",
      "authorizationEndpoint": {
        "_comment": "TEST OMGEVING - PEP Endpoint",
        "authorizationEndpointuri": "https://fictief.PEP.adres/"
      },
      "tokenEndpoint": {
        "_comment": "TEST OMGEVING - Autorisatieserver",
        "tokenEndpointuri": "https://fictief.autorisatie.punt"
      },
      "systeemrollen": [
        {
          "systeemrolcode": "REGISTER",
          "resourceEndpoint": {
            "_comment": "7777 Bemiddelingsregister",
            "resourceEndpointuri": "https://test.7777.resource.punt"
          }
        }
      ]
    }    
  ]
}
```

> [!NOTE] 
> Het voorbeeld laat ook zien dat voor de twee **resources** de endpoints voor de autorisatie en PEP gelijk zijn omdat nID voor beide resources deze functies vervult.

### 3. Voorbeeeld Test notificatieadres van zorgaanbieder 12121212

json:
```json
{
  "_comment": "iWlz service lijst - versie 0.1 - 12-11-2024",
  "electronicServices": [
    {
      "description": "TEST OMGEVING Voor het notificeren van zorgaanbieder 12121212",
      "gegevensdienstId": "12121212_NOTIFICATIE_TST",
      "weergavenaam": "TEST OMGEVING - notificaties voor 12121212",
      "authorizationEndpoint": {
        "_comment": "TEST OMGEVING - PEP Endpoint",
        "authorizationEndpointuri": "https://fictief.PEP.adres/"
      },
      "tokenEndpoint": {
        "_comment": "TEST OMGEVING - Autorisatieserver",
        "tokenEndpointuri": "https://fictief.autorisatie.punt"
      },
      "systeemrollen": [
        {
          "systeemrolcode": "NOTIFICATIE",
          "resourceEndpoint": {
            "_comment": "12121212 notificatie endpoint",
            "resourceEndpointuri": "https://test.notificatie.end.punt"
          }
        }
      ]
    }
}
```


# Adressenlijst

> [!TIP]
> **De Adreslijst is te vinden in de afgeschermde repository: [iWlz-adresboek-private](https://github.com/iStandaarden/iWlz-adresboek-private)**
> Om toegang te krijgen tot deze repository is:
> 1. Account nodig op Github
> 2. Toegevoegd worden als member.
> 3. Stuur daarna een bericht met vermelding van "Aanvraag toegang iWlz Adresboek private met meegeven van organisatie en githubaccount aan Dennis de Gouw - [@dennisdegouw](http://github.com/dennisdegouw) of Remo van Rest - [@rvanrest](https://github.com/rvanrest)
> 4. Je zal daarna een uitnodiging krijgen tot de private-repository

## Beheer

Neem voor het laten doorvoeren van wijzingen contact op met:

- servicedesk iStandaarden: [info_at_istandaarden.nl](info@istandaarden.nl)
- Dennis de Gouw - [@dennisdegouw](http://github.com/dennisdegouw)
- Remo van Rest - [@rvanrest](https://github.com/rvanrest)

# meer informatie:

- Actieprogramma iWlz: van keten naar netwerk: [het Actieprogramma iWlz](https://www.istandaarden.nl/iwlz/actieprogramma/index "Over Actieprogramma iWlz")
- Informatiemodel iStandaarden iWlz: [Informatiemodellen](https://informatiemodel.istandaarden.nl)
- Portaal voor iStandaarden in de Zorg en Ondersteuning: [homepagina iStandaarden](https://www.istandaarden.nl)
- Zorg-AB: https://www.vzvz.nl/diensten/gemeenschappelijke-diensten/zorg-ab
