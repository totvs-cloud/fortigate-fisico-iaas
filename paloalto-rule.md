# PALOALTO RULE

## Micro Serviço paloalto-rule

### Fluxo - Rule Create


```mermaid
flowchart TD
  A([Inicio]) --> C[Processa regras];
  C --> D{Config existe?};
  D -- Nao --> E[ERROR];
  D -- Sim --> F[Consulta regra];

  F --> G{Existe?};

  G -- Nao --> H[Cria regra];
  H --> I{Sucesso?};
  I -- Sim --> J[Mover];
  I -- Nao --> E;
  J --> T;

  G -- Sim --> L{Schedule?};
  L -- Sim --> M[Cria schedule];
  M --> N{Sucesso?};
  N -- Sim --> O[Mover];
  N -- Nao --> E;
  O --> T[COMPLETD];

  T --> C;

```

### End-Point API PaloAlto - Security Rule

> /config/devices/entry[@name='localhost.localdomain']/vsys/entry[@name='vsys2']/rule/entry[@name='T56593_CJP8BD_iaas']

### Payload API PaloAlto - Security Rule

```json
{
  "Name": "T56593_CJP8BD_iaas-6",
  "Path": "rule",
  "ServiceRule": {
    "Member": [
      "TCP-9001",
      "TCP-9000",
      "TCP-443"
    ]
  }
}
```

### Fluxo - Rule Delete

```mermaid
flowchart TD
  A([Start]) --> B[GetTransactionID]
  B --> C{For each rule}
  C --> D[Check Firewall]
  D -- no --> E[Return core.ERROR]
  D --> F[Build payload]
  F --> G[RuleExists]
  G -- yes --> H[DeleteRule]
  G -- no --> I[Next rule]
  H --> M{Success?}
  M -- no --> E
  M -- yes --> I[Next rule]

  I --> C
  C --> J[Rules Processed]
  J --> K[Return COMPLETED]
  E --> L([End])
  K --> L
```


### End-Point API PaloAlto

> /config/devices/entry[@name='localhost.localdomain']/vsys/entry[@name='vsys2']/rule/entry[@name='T56593_CJP8BD_iaas']

### Payload API PaloAlto

```json
{
  "Name":"T56593_CJP8BD_iaas-6"
}
```