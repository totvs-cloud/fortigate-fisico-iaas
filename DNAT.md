## DNAT

**Objetivo:** A regra DNAT (Destination Network Address Translation) é utilizada para publicar uma aplicação que esteja em sua máquina virtual IaaS para a internet. É um acesso de entrada onde a origem é internet ( sendo uma origem restrita ou não ) e o destino é o IP Público e Porta externa IaaS, onde a regra DNAT faz a tradução para o IP Privado de sua máquina virtual, na Porta interna e Protocolo de sua aplicação.

### Fluxo - Criação de DNAT

```mermaid
flowchart TB
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|next| v2_2_paloalto_host_create["v2.2.paloalto.host.create"]
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|error| v2_dnat_create["v2.dnat.create"]
  v2_2_paloalto_host_create["v2.2.paloalto.host.create"] -->|next| v2_3_paloalto_rule_create["v2.3.paloalto.rule.create"]
  v2_2_paloalto_host_create["v2.2.paloalto.host.create"] -->|error| v2_5_paloalto_host_create_error_delete["v2.5.paloalto.host.create.error.delete"]
  v2_3_paloalto_rule_create["v2.3.paloalto.rule.create"] -->|next| v2_4_paloalto_rule_edit["v2.4.paloalto.rule.edit"]
  v2_3_paloalto_rule_create["v2.3.paloalto.rule.create"] -->|error| v2_4_paloalto_rule_create_error_delete["v2.4.paloalto.rule.create.error.delete"]
  v2_4_paloalto_rule_edit["v2.4.paloalto.rule.edit"] -->|next| v2_5_paloalto_dnat_create["v2.5.paloalto.dnat.create"]
  v2_4_paloalto_rule_edit["v2.4.paloalto.rule.edit"] -->|error| v2_3_paloalto_rule_edit_error_edit["v2.3.paloalto.rule.edit.error.edit"]
  v2_5_paloalto_dnat_create["v2.5.paloalto.dnat.create"] -->|next| v2_6_paloalto_dnat_edit["v2.6.paloalto.dnat.edit"]
  v2_5_paloalto_dnat_create["v2.5.paloalto.dnat.create"] -->|error| v2_2_paloalto_dnat_create_error_delete["v2.2.paloalto.dnat.create.error.delete"]
  v2_6_paloalto_dnat_edit["v2.6.paloalto.dnat.edit"] -->|next| v2_7_fortinet_address_create["v2.7.fortinet.address.create"]
  v2_6_paloalto_dnat_edit["v2.6.paloalto.dnat.edit"] -->|error| v2_1_paloalto_dnat_edit_error_edit["v2.1.paloalto.dnat.edit.error.edit"]
  v2_7_fortinet_address_create["v2.7.fortinet.address.create"] -->|next| v2_8_fortinet_vip_create["v2.8.fortinet.vip.create"]
  v2_7_fortinet_address_create["v2.7.fortinet.address.create"] -->|error| v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"]
  v2_8_fortinet_vip_create["v2.8.fortinet.vip.create"] -->|next| v2_dnat_create["v2.dnat.create"]
  v2_8_fortinet_vip_create["v2.8.fortinet.vip.create"] -->|error| v2_2_fortinet_vip_create_error_delete["v2.2.fortinet.vip.create.error.delete"]
  v2_1_paloalto_dnat_edit_error_edit["v2.1.paloalto.dnat.edit.error.edit"] -->|next| v2_2_paloalto_dnat_create_error_delete["v2.2.paloalto.dnat.create.error.delete"]
  v2_1_paloalto_dnat_edit_error_edit["v2.1.paloalto.dnat.edit.error.edit"] -->|error| v2_dnat_create["v2.dnat.create"]
  v2_2_fortinet_vip_create_error_delete["v2.2.fortinet.vip.create.error.delete"] -->|next| v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"]
  v2_2_fortinet_vip_create_error_delete["v2.2.fortinet.vip.create.error.delete"] -->|error| v2_dnat_create["v2.dnat.create"]
  v2_2_paloalto_dnat_create_error_delete["v2.2.paloalto.dnat.create.error.delete"] -->|next| v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"]
  v2_2_paloalto_dnat_create_error_delete["v2.2.paloalto.dnat.create.error.delete"] -->|error| v2_dnat_create["v2.dnat.create"]
  v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"] -->|next| v2_2_paloalto_dnat_create_error_delete["v2.2.paloalto.dnat.create.error.delete"]
  v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"] -->|error| v2_dnat_create["v2.dnat.create"]
  v2_3_paloalto_rule_edit_error_edit["v2.3.paloalto.rule.edit.error.edit"] -->|next| v2_4_paloalto_rule_create_error_delete["v2.4.paloalto.rule.create.error.delete"]
  v2_3_paloalto_rule_edit_error_edit["v2.3.paloalto.rule.edit.error.edit"] -->|error| v2_dnat_create["v2.dnat.create"]
  v2_4_paloalto_rule_create_error_delete["v2.4.paloalto.rule.create.error.delete"] -->|next| v2_5_paloalto_host_create_error_delete["v2.5.paloalto.host.create.error.delete"]
  v2_4_paloalto_rule_create_error_delete["v2.4.paloalto.rule.create.error.delete"] -->|error| v2_dnat_create["v2.dnat.create"]
  v2_5_paloalto_host_create_error_delete["v2.5.paloalto.host.create.error.delete"] -->|next| v2_dnat_create["v2.dnat.create"]
  v2_5_paloalto_host_create_error_delete["v2.5.paloalto.host.create.error.delete"] -->|error| v2_dnat_create["v2.dnat.create"]
```

## Serviços envolvidos

- [v2.1.paloalto.service.create](paloalto-service.md#fluxo---service-create)
- [v2.2.paloalto.host.create](paloalto-host.md#fluxo---host-create)
- [v2.3.paloalto.rule.create](paloalto-rule.md#fluxo---rule-create)
- [v2.4.paloalto.rule.edit](paloalto-rule.md#fluxo---rule-edit)
- [v2.5.paloalto.dnat.create](paloalto-nat.md#fluxo---dnat-create)
- [v2.6.paloalto.dnat.edit](paloalto-nat.md#fluxo---dnat-edit)
---

### Fluxo - Edição de DNAT

```mermaid
flowchart TB
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|next| v2_2_paloalto_host_delete["v2.2.paloalto.host.delete"]
  v2_1_paloalto_service_create["v2.1.paloalto.service.create"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_2_paloalto_host_delete["v2.2.paloalto.host.delete"] -->|next| v2_3_paloalto_host_create["v2.3.paloalto.host.create"]
  v2_2_paloalto_host_delete["v2.2.paloalto.host.delete"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_3_paloalto_host_create["v2.3.paloalto.host.create"] -->|next| v2_4_paloalto_rule_delete["v2.4.paloalto.rule.delete"]
  v2_3_paloalto_host_create["v2.3.paloalto.host.create"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_4_paloalto_rule_delete["v2.4.paloalto.rule.delete"] -->|next| v2_5_paloalto_rule_create["v2.5.paloalto.rule.create"]
  v2_4_paloalto_rule_delete["v2.4.paloalto.rule.delete"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_5_paloalto_rule_create["v2.5.paloalto.rule.create"] -->|next| v2_6_paloalto_rule_edit["v2.6.paloalto.rule.edit"]
  v2_5_paloalto_rule_create["v2.5.paloalto.rule.create"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_6_paloalto_rule_edit["v2.6.paloalto.rule.edit"] -->|next| v2_7_paloalto_dnat_delete["v2.7.paloalto.dnat.delete"]
  v2_6_paloalto_rule_edit["v2.6.paloalto.rule.edit"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_7_paloalto_dnat_delete["v2.7.paloalto.dnat.delete"] -->|next| v2_8_paloalto_dnat_create["v2.8.paloalto.dnat.create"]
  v2_7_paloalto_dnat_delete["v2.7.paloalto.dnat.delete"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_8_paloalto_dnat_create["v2.8.paloalto.dnat.create"] -->|next| v2_9_paloalto_dnat_edit["v2.9.paloalto.dnat.edit"]
  v2_8_paloalto_dnat_create["v2.8.paloalto.dnat.create"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_9_paloalto_dnat_edit["v2.9.paloalto.dnat.edit"] -->|next| v2_10_fortinet_address_delete["v2.10.fortinet.address.delete"]
  v2_9_paloalto_dnat_edit["v2.9.paloalto.dnat.edit"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_10_fortinet_address_delete["v2.10.fortinet.address.delete"] -->|next| v2_11_fortinet_address_create["v2.11.fortinet.address.create"]
  v2_10_fortinet_address_delete["v2.10.fortinet.address.delete"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_11_fortinet_address_create["v2.11.fortinet.address.create"] -->|next| v2_12_fortinet_vip_delete["v2.12.fortinet.vip.delete"]
  v2_11_fortinet_address_create["v2.11.fortinet.address.create"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_12_fortinet_vip_delete["v2.12.fortinet.vip.delete"] -->|next| v2_13_fortinet_vip_create["v2.13.fortinet.vip.create"]
  v2_12_fortinet_vip_delete["v2.12.fortinet.vip.delete"] -->|error| v2_dnat_edit["v2.dnat.edit"]
  v2_13_fortinet_vip_create["v2.13.fortinet.vip.create"] -->|next| v2_dnat_edit["v2.dnat.edit"]
  v2_13_fortinet_vip_create["v2.13.fortinet.vip.create"] -->|error| v2_dnat_edit["v2.dnat.edit"]
```

### Fluxo - Remoção de DNAT

```mermaid
flowchart TB
  v2_1_paloalto_rule_delete["v2.1.paloalto.rule.delete"] -->|next| v2_2_paloalto_rule_edit["v2.2.paloalto.rule.edit"]
  v2_1_paloalto_rule_delete["v2.1.paloalto.rule.delete"] -->|error| v2_dnat_delete["v2.dnat.delete"]
  v2_2_paloalto_rule_edit["v2.2.paloalto.rule.edit"] -->|next| v2_3_paloalto_dnat_delete["v2.3.paloalto.dnat.delete"]
  v2_2_paloalto_rule_edit["v2.2.paloalto.rule.edit"] -->|error| v2_dnat_delete["v2.dnat.delete"]
  v2_3_paloalto_dnat_delete["v2.3.paloalto.dnat.delete"] -->|next| v2_4_paloalto_dnat_edit["v2.4.paloalto.dnat.edit"]
  v2_3_paloalto_dnat_delete["v2.3.paloalto.dnat.delete"] -->|error| v2_dnat_delete["v2.dnat.delete"]
  v2_4_paloalto_dnat_edit["v2.4.paloalto.dnat.edit"] -->|next| v2_5_paloalto_service_delete["v2.5.paloalto.service.delete"]
  v2_4_paloalto_dnat_edit["v2.4.paloalto.dnat.edit"] -->|error| v2_dnat_delete["v2.dnat.delete"]
  v2_5_paloalto_service_delete["v2.5.paloalto.service.delete"] -->|next| v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"]
  v2_5_paloalto_service_delete["v2.5.paloalto.service.delete"] -->|error| v2_dnat_delete["v2.dnat.delete"]
  v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"] -->|next| v2_7_fortinet_address_delete["v2.7.fortinet.address.delete"]
  v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"] -->|error| v2_dnat_delete["v2.dnat.delete"]
  v2_7_fortinet_address_delete["v2.7.fortinet.address.delete"] -->|next| v2_8_fortinet_vip_delete["v2.8.fortinet.vip.delete"]
  v2_7_fortinet_address_delete["v2.7.fortinet.address.delete"] -->|error| v2_dnat_delete["v2.dnat.delete"]
  v2_8_fortinet_vip_delete["v2.8.fortinet.vip.delete"] -->|next| v2_dnat_delete["v2.dnat.delete"]
  v2_8_fortinet_vip_delete["v2.8.fortinet.vip.delete"] -->|error| v2_dnat_delete["v2.dnat.delete"]
```