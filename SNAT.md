## SNAT

**Objetivo:** A regra SNAT (Source Network Address Translation) é utilizada no sentido de Saída, ou seja, uma máquina virtual IaaS acessando a Internet. A máquina virtual IaaS não possui um IP válido para a Internet, sendo necessário uma tradução do IP Privado da máquina virtual para um IP Público válido na Internet.

### Fluxo - Criação de SAT

```mermaid
flowchart TB
  v2_1_paloalto_host_create["v2.1.paloalto.host.create"] -->|next| v2_2_paloalto_snat_create["v2.2.paloalto.snat.create"]
  v2_1_paloalto_host_create["v2.1.paloalto.host.create"] -->|error| v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"]
  v2_2_paloalto_snat_create["v2.2.paloalto.snat.create"] -->|next| v2_3_paloalto_snat_edit["v2.3.paloalto.snat.edit"]
  v2_2_paloalto_snat_create["v2.2.paloalto.snat.create"] -->|error| v2_2_paloalto_snat_create_error_delete["v2.2.paloalto.snat.create.error.delete"]
  v2_3_paloalto_snat_edit["v2.3.paloalto.snat.edit"] -->|next| v2_4_fortinet_address_create["v2.4.fortinet.address.create"]
  v2_3_paloalto_snat_edit["v2.3.paloalto.snat.edit"] -->|error| v2_1_paloalto_snat_edit_error_edit["v2.1.paloalto.snat.edit.error.edit"]
  v2_4_fortinet_address_create["v2.4.fortinet.address.create"] -->|next| v2_5_fortinet_snat_create["v2.5.fortinet.snat.create"]
  v2_4_fortinet_address_create["v2.4.fortinet.address.create"] -->|error| v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"]
  v2_5_fortinet_snat_create["v2.5.fortinet.snat.create"] -->|next| v2_snat_create["v2.snat.create"]
  v2_5_fortinet_snat_create["v2.5.fortinet.snat.create"] -->|error| v2_1_fortinet_snat_create_error_delete["v2.1.fortinet.snat.create.error.delete"]
  v2_1_fortinet_snat_create_error_delete["v2.1.fortinet.snat.create.error.delete"] -->|next| v2_2_fortinet_vip_create_error_delete["v2.2.fortinet.vip.create.error.delete"]
  v2_1_fortinet_snat_create_error_delete["v2.1.fortinet.snat.create.error.delete"] -->|error| v2_snat_create["v2.snat.create"]
  v2_1_paloalto_snat_edit_error_edit["v2.1.paloalto.snat.edit.error.edit"] -->|next| v2_2_paloalto_snat_create_error_delete["v2.2.paloalto.snat.create.error.delete"]
  v2_1_paloalto_snat_edit_error_edit["v2.1.paloalto.snat.edit.error.edit"] -->|error| v2_snat_create["v2.snat.create"]
  v2_2_paloalto_snat_create_error_delete["v2.2.paloalto.snat.create.error.delete"] -->|next| v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"]
  v2_2_paloalto_snat_create_error_delete["v2.2.paloalto.snat.create.error.delete"] -->|error| v2_snat_create["v2.snat.create"]
  v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"] -->|next| v2_2_paloalto_snat_create_error_delete["v2.2.paloalto.snat.create.error.delete"]
  v2_3_fortinet_address_create_error_delete["v2.3.fortinet.address.create.error.delete"] -->|error| v2_snat_create["v2.snat.create"]
  v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"] -->|next| v2_snat_create["v2.snat.create"]
  v2_3_paloalto_host_create_error_delete["v2.3.paloalto.host.create.error.delete"] -->|error| v2_snat_create["v2.snat.create"]
```

## Serviços envolvidos

- [v2.1.paloalto.host.create](paloalto-host.md#fluxo---host-create)
- [v2.2.paloalto.snat.create](paloalto-nat.md#fluxo---snat-create)
- [v2.3.paloalto.snat.edit](paloalto-nat.md#fluxo---snat-edit)


---

### Fluxo - Edição de NAT

```mermaid
flowchart TB
  v2_1_paloalto_host_create["v2.1.paloalto.host.create"] -->|next| v2_2_paloalto_snat_edit["v2.2.paloalto.snat.edit"]
  v2_1_paloalto_host_create["v2.1.paloalto.host.create"] -->|error| v2_snat_edit["v2.snat.edit"]
  v2_2_fortinet_address_delete["v2.2.fortinet.address.delete"] -->|next| v2_6_fortinet_snat_edit["v2.6.fortinet.snat.edit"]
  v2_2_fortinet_address_delete["v2.2.fortinet.address.delete"] -->|error| v2_snat_edit["v2.snat.edit"]
  v2_2_paloalto_snat_edit["v2.2.paloalto.snat.edit"] -->|next| v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"]
  v2_2_paloalto_snat_edit["v2.2.paloalto.snat.edit"] -->|error| v2_snat_edit["v2.snat.edit"]
  v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"] -->|next| v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"]
  v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"] -->|error| v2_snat_edit["v2.snat.edit"]
  v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"] -->|next| v2_5_fortinet_address_create["v2.5.fortinet.address.create"]
  v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"] -->|error| v2_2_fortinet_address_delete["v2.2.fortinet.address.delete"]
  v2_5_fortinet_address_create["v2.5.fortinet.address.create"] -->|next| v2_6_fortinet_snat_edit["v2.6.fortinet.snat.edit"]
  v2_5_fortinet_address_create["v2.5.fortinet.address.create"] -->|error| v2_snat_edit["v2.snat.edit"]
  v2_6_fortinet_snat_edit["v2.6.fortinet.snat.edit"] -->|next| v2_snat_edit["v2.snat.edit"]
  v2_6_fortinet_snat_edit["v2.6.fortinet.snat.edit"] -->|error| v2_snat_edit["v2.snat.edit"]

```

### Fluxo - Remoção de SNAT

```mermaid
flowchart TB
  v2_1_paloalto_snat_delete["v2.1.paloalto.snat.delete"] -->|next| v2_2_paloalto_snat_edit["v2.2.paloalto.snat.edit"]
  v2_1_paloalto_snat_delete["v2.1.paloalto.snat.delete"] -->|error| v2_snat_delete["v2.snat.delete"]
  v2_2_paloalto_snat_edit["v2.2.paloalto.snat.edit"] -->|next| v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"]
  v2_2_paloalto_snat_edit["v2.2.paloalto.snat.edit"] -->|error| v2_snat_delete["v2.snat.delete"]
  v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"] -->|next| v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"]
  v2_3_paloalto_host_delete["v2.3.paloalto.host.delete"] -->|error| v2_snat_delete["v2.snat.delete"]
  v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"] -->|next| v2_5_fortinet_snat_delete["v2.5.fortinet.snat.delete"]
  v2_4_fortinet_address_delete["v2.4.fortinet.address.delete"] -->|error| v2_snat_delete["v2.snat.delete"]
  v2_5_fortinet_snat_delete["v2.5.fortinet.snat.delete"] -->|next| v2_snat_delete["v2.snat.delete"]
  v2_5_fortinet_snat_delete["v2.5.fortinet.snat.delete"] -->|error| v2_snat_delete["v2.snat.delete"]
```