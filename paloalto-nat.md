# PALOALTO NAT

## Micro Serviço paloalto-nat

### Fluxo - Nat Create

```mermaid
flowchart TD
  A([Start]) --> B[for each host]
  B --> C{Singleton?}
  C -- No --> E[ERROR]
  C -- Yes --> D[build Client]
  D --> F[new client]
  F --> G{client ok?}
  G -- No --> E
  G -- Yes --> H["get nat"]
  H --> I{"code ok? (0 or 5)"}
  I -- No --> E
  I -- Yes --> J[build payload]
  J --> K{nat exists?}
  K -- Yes --> O[mark rollback data]
  K -- No --> M["create nat (retry)"]
  M --> N{create ok?}
  N -- No --> E
  N -- Yes --> O
  O --> P[update=true]
  P --> R{more nats?}
  R -- Yes --> B
  R -- No --> S([return COMPLETED])
``` 


### End-Point API PaloAlto - Nat Object

> /config/devices/entry[@name='localhost.localdomain']/vsys/entry[@name='vsys2']/nat/entry[@name='TEZAX9_CSV7UN_iaas']

### Payload API PaloAlto - Nat Object

```json
{
  "Name": "TEZAX9_CSV7UN_iaas_SNAT-1",
  "Path": "nat",
  "Interface": "any",
  "ServiceNat": "any",
  "SourceTranslation": {
    "AddressDynamicAndPort": "TEZAX9_CSV7UN_iaas_HST-189.126.138.52"
  },
  "To": {
    "Member": "Externo"
  },
  "From": {
    "Member": [
      "Floating-NSX"
    ]
  },
  "Source": {
    "Member": [
      "HST-10.104.7.213"
    ]
  },
  "Destination": {
    "Member": [
      "any"
    ]
  }
}
```

### Fluxo - Nat Edit

```mermaid
flowchart TD
  A([Start]) --> B[for each host]
  B --> C{Singleton?}
  C -- No --> E[ERROR]
  C -- Yes --> D[build Client]
  D --> F[new client]
  F --> G{client ok?}
  G -- No --> E
  G -- Yes --> H["get nat"]
  H --> I{"code ok? (0 or 5)"}
  I -- No --> E
  I -- Yes --> J[build payload]
  J --> K{nat exists?}
  K -- Yes --> O[mark rollback data]
  K -- No --> M["edit nat (retry)"]
  M --> N{create ok?}
  N -- No --> E
  N -- Yes --> O
  O --> P[update=true]
  P --> R{more nats?}
  R -- Yes --> B
  R -- No --> S([return COMPLETED])
``` 


### End-Point API PaloAlto - Nat Object

> /config/devices/entry[@name='localhost.localdomain']/vsys/entry[@name='vsys2']/nat/entry[@name='TEZAX9_CSV7UN_iaas']

### Payload API PaloAlto - Nat Object

```json
{
  "Name": "TEZAX9_CSV7UN_iaas_SNAT-1",
  "Path": "nat",
  "Interface": "any",
  "ServiceNat": "any",
  "SourceTranslation": {
    "AddressDynamicAndPort": "TEZAX9_CSV7UN_iaas_HST-189.126.138.52"
  },
  "To": {
    "Member": "Externo"
  },
  "From": {
    "Member": [
      "Floating-NSX"
    ]
  },
  "Source": {
    "Member": [
      "HST-10.104.7.213"
    ]
  },
  "Destination": {
    "Member": [
      "any"
    ]
  }
}
```