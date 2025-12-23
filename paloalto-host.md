# PALOALTO HOST

## Micro Serviço paloalto-host

### Fluxo - Host Create

```mermaid
flowchart TD
  A([Start]) --> B[for each host]
  B --> C{Singleton?}
  C -- No --> E[ERROR]
  C -- Yes --> D[build Client]
  D --> F[new client]
  F --> G{client ok?}
  G -- No --> E
  G -- Yes --> H["get host"]
  H --> I{"code ok? (0 or 5)"}
  I -- No --> E
  I -- Yes --> J[build payload]
  J --> K{host exists?}
  K -- Yes --> O[mark rollback data]
  K -- No --> M["create host (retry)"]
  M --> N{create ok?}
  N -- No --> E
  N -- Yes --> O
  O --> P[update=true]
  P --> R{more hosts?}
  R -- Yes --> B
  R -- No --> S([return COMPLETED])
``` 


### End-Point API PaloAlto - Address Object

> /config/devices/entry[@name='localhost.localdomain']/vsys/entry[@name='vsys2']/address/entry[@name='TEZAX9_CSV7UN_iaas']

### Payload API PaloAlto - Address Object

```json
{
  "Name": "HST-10.104.7.213",
  "Path": "address",
  "Ip": "10.104.7.213/32"
}
```