## UNAT

**Objetivo:** O UNAT, também conhecido como U-Turn NAT, é um tipo especial de NAT utilizado quando usuários internos precisam acessar um servidor interno usando o endereço de IP público do servidor. Nestes casos, a origem e destino compartilham do mesmo firewall e ambiente de rede.

### Fluxo - Criação de UNAT

```mermaid
flowchart TB
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|next| v2_2_paloalto_host_create["v2.2.paloalto.host.create"]
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|error| v2_unat_create["v2.unat.create"]
  v2_2_paloalto_host_create["v2.2.paloalto.host.create"] -->|next| v2_3_paloalto_unat_create["v2.3.paloalto.unat.create"]
  v2_2_paloalto_host_create["v2.2.paloalto.host.create"] -->|error| v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"]
  v2_3_paloalto_unat_create["v2.3.paloalto.unat.create"] -->|next| v2_4_paloalto_unat_edit["v2.4.paloalto.unat.edit"]
  v2_3_paloalto_unat_create["v2.3.paloalto.unat.create"] -->|error| v2_2_paloalto_unat_create_error_delete["v2.2.paloalto.unat.create.error.delete"]
  v2_4_paloalto_unat_edit["v2.4.paloalto.unat.edit"] -->|next| v2_5_fortinet_address_create["v2.5.fortinet.address.create"]
  v2_4_paloalto_unat_edit["v2.4.paloalto.unat.edit"] -->|error| v2_1_paloalto_unat_edit_error_edit["v2.1.paloalto.unat.edit.error.edit"]
  v2_5_fortinet_address_create["v2.5.fortinet.address.create"] -->|next| v2_6_fortinet_service_create["v2.6.fortinet.service.create"]
  v2_5_fortinet_address_create["v2.5.fortinet.address.create"] -->|error| v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"]
  v2_6_fortinet_service_create["v2.6.fortinet.service.create"] -->|next| v2_7_fortinet_vip_create["v2.7.fortinet.vip.create"]
  v2_6_fortinet_service_create["v2.6.fortinet.service.create"] -->|error| v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"]
  v2_7_fortinet_vip_create["v2.7.fortinet.vip.create"] -->|next| v2_8_fortinet_policy_create["v2.8.fortinet.policy.create"]
  v2_7_fortinet_vip_create["v2.7.fortinet.vip.create"] -->|error| v2_2_fortinet_vip_create_error_delete["v2.2.fortinet.vip.create.error.delete"]
  v2_8_fortinet_policy_create["v2.8.fortinet.policy.create"] -->|next| v2_unat_create["v2.unat.create"]
  v2_8_fortinet_policy_create["v2.8.fortinet.policy.create"] -->|error| v2_3_fortinet_policy_create_error_delete["v2.3.fortinet.policy.create.error.delete"]
  v2_1_paloalto_unat_edit_error_edit["v2.1.paloalto.unat.edit.error.edit"] -->|next| v2_2_paloalto_unat_create_error_delete["v2.2.paloalto.unat.create.error.delete"]
  v2_1_paloalto_unat_edit_error_edit["v2.1.paloalto.unat.edit.error.edit"] -->|error| v2_unat_create["v2.unat.create"]
  v2_2_fortinet_vip_create_error_delete["v2.2.fortinet.vip.create.error.delete"] -->|next| v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"]
  v2_2_fortinet_vip_create_error_delete["v2.2.fortinet.vip.create.error.delete"] -->|error| v2_unat_create["v2.unat.create"]
  v2_2_paloalto_unat_create_error_delete["v2.2.paloalto.unat.create.error.delete"] -->|next| v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"]
  v2_2_paloalto_unat_create_error_delete["v2.2.paloalto.unat.create.error.delete"] -->|error| v2_unat_create["v2.unat.create"]
  v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"] -->|next| v2_2_paloalto_unat_create_error_delete["v2.2.paloalto.unat.create.error.delete"]
  v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"] -->|error| v2_unat_create["v2.unat.create"]
  v2_3_fortinet_policy_create_error_delete["v2.3.fortinet.policy.create.error.delete"] -->|next| v2_unat_create["v2.unat.create"]
  v2_3_fortinet_policy_create_error_delete["v2.3.fortinet.policy.create.error.delete"] -->|error| v2_unat_create["v2.unat.create"]
  v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"] -->|next| v2_unat_create["v2.unat.create"]
  v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"] -->|error| v2_unat_create["v2.unat.create"]
```

## Serviços envolvidos

- [v2.1.paloalto.service.create](paloalto-service.md#fluxo---service-create)
- [v2.2.paloalto.host.create](paloalto-host.md#fluxo---host-create)
- [v2.3.paloalto.unat.create](paloalto-nat.md#fluxo---unat-create)
- [v2.4.paloalto.unat.edit](paloalto-nat.md#fluxo---unat-edit)
---

### Fluxo - Edição de UNAT

```mermaid
flowchart TB
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|next| v2_2_paloalto_host_create["v2.2.paloalto.host.create"]
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_2_paloalto_host_create["v2.2.paloalto.host.create"] -->|next| v2_3_paloalto_unat_delete["v2.3.paloalto.unat.delete"]
  v2_2_paloalto_host_create["v2.2.paloalto.host.create"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_3_paloalto_unat_delete["v2.3.paloalto.unat.delete"] -->|next| v2_4_paloalto_unat_create["v2.4.paloalto.unat.create"]
  v2_3_paloalto_unat_delete["v2.3.paloalto.unat.delete"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_4_paloalto_unat_create["v2.4.paloalto.unat.create"] -->|next| v2_5_paloalto_unat_edit["v2.5.paloalto.unat.edit"]
  v2_4_paloalto_unat_create["v2.4.paloalto.unat.create"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_5_paloalto_unat_edit["v2.5.paloalto.unat.edit"] -->|next| v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"]
  v2_5_paloalto_unat_edit["v2.5.paloalto.unat.edit"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"] -->|next| v2_7_fortinet_address_create["v2.7.fortinet.address.create"]
  v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_7_fortinet_address_create["v2.7.fortinet.address.create"] -->|next| v2_8_fortinet_address_delete["v2.8.fortinet.address.delete"]
  v2_7_fortinet_address_create["v2.7.fortinet.address.create"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_8_fortinet_address_delete["v2.8.fortinet.address.delete"] -->|next| v2_9_fortinet_service_create["v2.9.fortinet.service.create"]
  v2_8_fortinet_address_delete["v2.8.fortinet.address.delete"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_9_fortinet_service_create["v2.9.fortinet.service.create"] -->|next| v2_10_fortinet_service_delete["v2.10.fortinet.service.delete"]
  v2_9_fortinet_service_create["v2.9.fortinet.service.create"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_10_fortinet_service_delete["v2.10.fortinet.service.delete"] -->|next| v2_11_fortinet_vip_create["v2.11.fortinet.vip.create"]
  v2_10_fortinet_service_delete["v2.10.fortinet.service.delete"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_11_fortinet_vip_create["v2.11.fortinet.vip.create"] -->|next| v2_12_fortinet_vip_delete["v2.12.fortinet.vip.delete"]
  v2_11_fortinet_vip_create["v2.11.fortinet.vip.create"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_12_fortinet_vip_delete["v2.12.fortinet.vip.delete"] -->|next| v2_13_fortinet_policy_create["v2.13.fortinet.policy.create"]
  v2_12_fortinet_vip_delete["v2.12.fortinet.vip.delete"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_13_fortinet_policy_create["v2.13.fortinet.policy.create"] -->|next| v2_14_fortinet_policy_delete["v2.14.fortinet.policy.delete"]
  v2_13_fortinet_policy_create["v2.13.fortinet.policy.create"] -->|error| v2_unat_edit["v2.unat.edit"]
  v2_14_fortinet_policy_delete["v2.14.fortinet.policy.delete"] -->|next| v2_unat_edit["v2.unat.edit"]
  v2_14_fortinet_policy_delete["v2.14.fortinet.policy.delete"] -->|error| v2_unat_edit["v2.unat.edit"]
```

### Fluxo - Remoção de UNAT

```mermaid
flowchart TB
  v2_1_paloalto_unat_delete["v2.1.paloalto.unat.delete"] -->|next| v2_2_paloalto_service_delete["v2.2.paloalto.service.delete"]
  v2_1_paloalto_unat_delete["v2.1.paloalto.unat.delete"] -->|error| v2_unat_delete["v2.unat.delete"]
  v2_2_paloalto_service_delete["v2.2.paloalto.service.delete"] -->|next| v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"]
  v2_2_paloalto_service_delete["v2.2.paloalto.service.delete"] -->|error| v2_unat_delete["v2.unat.delete"]
  v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"] -->|next| v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"]
  v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"] -->|error| v2_unat_delete["v2.unat.delete"]
  v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"] -->|next| v2_5_fortinet_service_delete["v2.5.fortinet.service.delete"]
  v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"] -->|error| v2_unat_delete["v2.unat.delete"]
  v2_5_fortinet_service_delete["v2.5.fortinet.service.delete"] -->|next| v2_6_fortinet_vip_delete["v2.6.fortinet.vip.delete"]
  v2_5_fortinet_service_delete["v2.5.fortinet.service.delete"] -->|error| v2_unat_delete["v2.unat.delete"]
  v2_6_fortinet_vip_delete["v2.6.fortinet.vip.delete"] -->|next| v2_7_fortinet_policy_delete["v2.7.fortinet.policy.delete"]
  v2_6_fortinet_vip_delete["v2.6.fortinet.vip.delete"] -->|error| v2_unat_delete["v2.unat.delete"]
  v2_7_fortinet_policy_delete["v2.7.fortinet.policy.delete"] -->|next| v2_unat_delete["v2.unat.delete"]
  v2_7_fortinet_policy_delete["v2.7.fortinet.policy.delete"] -->|error| v2_unat_delete["v2.unat.delete"]
```