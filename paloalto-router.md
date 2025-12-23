# PALOALTO ROUTER

## Micro Serviço paloalto-router

### Fluxo - Service Create

```mermaid
flowchart TD
  A([Start]) --> B[GetTransactionID]
  B --> C{Para cada router}
  C --> D[Checar Firewall]
  D -- não --> E[Retornar core.ERROR]
  D --> F[Montar payload]
  F --> G[client.Exists.payload]
  G -- não --> H[client.Create.payload]
  H -- erro --> E
  H --> I[Próximo router]

  G -- existe --> I
  I --> C
  C --> J[Portas Processadas]
  J --> K[Retornar COMPLETED]
  E --> L([End])
  K --> L
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