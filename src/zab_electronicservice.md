```mermaid
---
config:
  theme: default
  look: classic
---

classDiagram
    class Root {
        +List~ElectronicService~ electronicServices
    }

    class ElectronicService {
        +string type
        +string id
        +string description
        +string gegevensdienstId
        +string weergavenaam
        +AuthorizationEndpoint authorizationEndpoint
        +TokenEndpoint tokenEndpoint
        +List~Systeemrol~ systeemrollen
    }

    class AuthorizationEndpoint {
        +string authorizationEndpointuri
    }

    class TokenEndpoint {
        +string tokenEndpointuri
    }

    class Systeemrol {
        +string systeemrolcode
        +ResourceEndpoint resourceEndpoint
    }

    class ResourceEndpoint {
        +string resourceEndpointuri
    }

    Root --> ElectronicService
    ElectronicService "1"-->"1" AuthorizationEndpoint
    ElectronicService "1"-->"1" TokenEndpoint
    ElectronicService "1"-->"1..*" Systeemrol
    Systeemrol "1"-->"1" ResourceEndpoint

    ```
