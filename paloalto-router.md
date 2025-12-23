# PALOALTO ROUTER

## Micro Serviço paloalto-router

### Fluxo - Get Router

```mermaid
flowchart TD
  A([Start]) --> B[GetTransactionID]
  B --> C[Para cada router]
  C --> D[Checar Firewall]
  D -- não --> E[Retornar core.ERROR]
  D --> F[Montar payload]
  F --> G[client.Exists.payload]
  G -- não --> E[Retornar core.ERROR]
  G --> I[Utilizar router]

  G -- existe --> I
  I --> L([End])
  E --> L([End])
  
```

### End-Point API PaloAlto

> /config/devices/entry[@name='localhost.localdomain']/vsys/entry[@name='vsys2']/router/entry[@name='TEZII4_CKGMN8_iaas']

### Payload API PaloAlto

```json
{
  "Name": "default",
  "Path": "router"
}

```