## TAG

**Objetivo:** Criação e remoção de Tag no Paloalto. Esse recurso está dentro do fluxo de criação de Organization. v2.4.paloalto.tag.create

### Fluxo

```mermaid
flowchart TB
  v2_1_nsxt_tier1_create["v2.1.nsxt.tier1.create"] -->|next| v2_2_nsxt_locale_service_create["v2.2.nsxt.locale.service.create"]
  v2_1_nsxt_tier1_create["v2.1.nsxt.tier1.create"] -->|error| v2_5_nsxt_tier1_create_error_delete["v2.5.nsxt.tier1.create.error.delete"]
  v2_2_nsxt_locale_service_create["v2.2.nsxt.locale.service.create"] -->|next| v2_3_nsxt_policy_create["v2.3.nsxt.policy.create"]
  v2_2_nsxt_locale_service_create["v2.2.nsxt.locale.service.create"] -->|error| v2_4_nsxt_locale_service_create_error_delete["v2.4.nsxt.locale.service.create.error.delete"]
  v2_3_nsxt_policy_create["v2.3.nsxt.policy.create"] -->|next| v2_4_paloalto_tag_create["v2.4.paloalto.tag.create"]
  v2_3_nsxt_policy_create["v2.3.nsxt.policy.create"] -->|error| v2_3_nsxt_poliy_create_error_delete["v2.3.nsxt.poliy.create.error.delete"]
  v2_4_nsxt_rule_create["v2.4.nsxt.rule.create"] -->|next| v2_5_paloalto_tag_create["v2.5.paloalto.tag.create"]
  v2_4_nsxt_rule_create["v2.4.nsxt.rule.create"] -->|error| v2_3_nsxt_poliy_create_error_delete["v2.3.nsxt.poliy.create.error.delete"]
  v2_4_paloalto_tag_create["v2.4.paloalto.tag.create"] -->|next| v2_5_vsphere_create_folder["v2.5.vsphere.create.folder"]
  v2_4_paloalto_tag_create["v2.4.paloalto.tag.create"] -->|error| v2_2_paloalto_tag_create_error_delete["v2.2.paloalto.tag.create.error.delete"]
  v2_5_vsphere_create_folder["v2.5.vsphere.create.folder"] -->|next| v2_organization_create["v2.organization.create"]
  v2_5_vsphere_create_folder["v2.5.vsphere.create.folder"] -->|error| v2_1_vsphere_create_folder_error_delete["v2.1.vsphere.create.folder.error.delete"]
  v2_1_vsphere_create_folder_error_delete["v2.1.vsphere.create.folder.error.delete"] -->|next| v2_2_paloalto_tag_create_error_delete["v2.2.paloalto.tag.create.error.delete"]
  v2_1_vsphere_create_folder_error_delete["v2.1.vsphere.create.folder.error.delete"] -->|error| v2_organization_create["v2.organization.create"]
  v2_2_paloalto_tag_create_error_delete["v2.2.paloalto.tag.create.error.delete"] -->|next| v2_3_nsxt_policy_create_error_delete["v2.3.nsxt.policy.create.error.delete"]
  v2_2_paloalto_tag_create_error_delete["v2.2.paloalto.tag.create.error.delete"] -->|error| v2_organization_create["v2.organization.create"]
  v2_3_nsxt_policy_create_error_delete["v2.3.nsxt.policy.create.error.delete"] -->|next| v2_4_nsxt_locale_service_create_error_delete["v2.4.nsxt.locale.service.create.error.delete"]
  v2_3_nsxt_policy_create_error_delete["v2.3.nsxt.policy.create.error.delete"] -->|error| v2_organization_create["v2.organization.create"]
  v2_4_nsxt_locale_service_create_error_delete["v2.4.nsxt.locale.service.create.error.delete"] -->|next| v2_5_nsxt_tier1_create_error_delete["v2.5.nsxt.tier1.create.error.delete"]
  v2_4_nsxt_locale_service_create_error_delete["v2.4.nsxt.locale.service.create.error.delete"] -->|error| v2_organization_create["v2.organization.create"]
  v2_5_nsxt_tier1_create_error_delete["v2.5.nsxt.tier1.create.error.delete"] -->|next| v2_organization_create["v2.organization.create"]
  v2_5_nsxt_tier1_create_error_delete["v2.5.nsxt.tier1.create.error.delete"] -->|error| v2_organization_create["v2.organization.create"]

```

## Micro Serviço paloalto-tag - create

### Fluxo

```mermaid
flowchart LR
  Start([Start])
  ForEach["Loop: para cada tag"]
  Check{Existe Paloalto?}
  CreateClient["Iniciar client PaloAlto"]
  GetTag["Get tag por nome"]
  Exists{Tag existe?}
  CreateTag["Criar tag"]
  SetUpdate["update = true"]
  Next["Próximo tag"]
  ReturnOK["Retorna COMPLETED (update true/false)"]
  Error["Retorna ERROR"]

  Start --> ForEach --> Check
  Check -- Não --> Error
  Check -- Sim --> CreateClient --> GetTag
  GetTag --> Exists
  Exists -- Sim --> Next
  Exists -- Não --> CreateTag --> SetUpdate --> Next
  Next --> ForEach
  ForEach -->|todos processados| ReturnOK
```

### Payload no Micro Serviço

```json
{
  "PaloaltoTag": [
    {
      "ID": 6781,
      "CreatedAt": "2025-12-16T14:55:59Z",
      "UpdatedAt": "2025-12-16T14:55:59Z",
      "DeletedAt": null,
      "OrganizationID": 3949,
      "Name": "T13726_C1B7HY_iaas",
      "Identifier": "fisico"
    }
  ]
}
```

### End-Point API PaloAlto

> /config/devices/entry[@name='localhost.localdomain']/vsys/entry[@name='vsys2']/tag/entry[@name='T13726_C1B7HY_iaas']

### Payload API PaloAlto

```json
{
  "Name": "TFDFA0_CP2RY7_iaas",
  "Path": "tag",
  "Color": "color9",
  "Comments": "TFDFA0_CP2RY7_iaas"
}

```



 