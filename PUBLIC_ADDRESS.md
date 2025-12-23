# PUBLIC_ADDRESS

**Objetivo:** Criar e gerenciar os endereço de IP Público para o ambiente da organização .

## Fluxo - Public-Address Create

**Obs:** A criação de Public-Address não utiliza serviços como é feito no delete, pois é criado diretamente no api-nemesis que é o microservico principal do gateway, e segue o fluxo normal de criação de host

### Serviços envolvidos na criação

- [fluxo de host](paloalto-host.md#fluxo---host-create)

### Serviços envolvidos no delete

- [v2.1.paloalto.rule.delete](paloalto-rule.md#fluxo---rule-delete)
- [v2.2.paloalto.rule.edit](paloalto-rule.md#fluxo---rule-edit)
- [v2.3.paloalto.nat.delete](paloalto-nat.md#fluxo---nat-delete)
- [v2.4.paloalto.nat.edit](paloalto-nat.md#fluxo---nat-edit)
- [v2.5.paloalto.host.delete](paloalto-host.md#fluxo---host-delete)

## Fluxo - Public-Address Delete

```mermaid
flowchart TB
  v2_1_paloalto_rule_delete["v2.1.paloalto.rule.delete"] -->|next| v2_2_paloalto_rule_edit["v2.2.paloalto.rule.edit"]
  v2_1_paloalto_rule_delete["v2.1.paloalto.rule.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_2_paloalto_rule_edit["v2.2.paloalto.rule.edit"] -->|next| v2_3_paloalto_nat_delete["v2.3.paloalto.nat.delete"]
  v2_2_paloalto_rule_edit["v2.2.paloalto.rule.edit"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_3_paloalto_nat_delete["v2.3.paloalto.nat.delete"] -->|next| v2_4_paloalto_nat_edit["v2.4.paloalto.nat.edit"]
  v2_3_paloalto_nat_delete["v2.3.paloalto.nat.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_4_paloalto_nat_edit["v2.4.paloalto.nat.edit"] -->|next| v2_5_paloalto_host_delete["v2.5.paloalto.host.delete"]
  v2_4_paloalto_nat_edit["v2.4.paloalto.nat.edit"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_5_paloalto_host_delete["v2.5.paloalto.host.delete"] -->|next| v2_6_nsxt_lb_edit["v2.6.nsxt.lb.edit"]
  v2_5_paloalto_host_delete["v2.5.paloalto.host.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_6_nsxt_lb_edit["v2.6.nsxt.lb.edit"] -->|next| v2_7_nsxt_vs_delete["v2.7.nsxt.vs.delete"]
  v2_6_nsxt_lb_edit["v2.6.nsxt.lb.edit"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_7_nsxt_vs_delete["v2.7.nsxt.vs.delete"] -->|next| v2_8_nsxt_pool_delete["v2.8.nsxt.pool.delete"]
  v2_7_nsxt_vs_delete["v2.7.nsxt.vs.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_8_nsxt_pool_delete["v2.8.nsxt.pool.delete"] -->|next| v2_9_fortinet_vip_delete["v2.9.fortinet.vip.delete"]
  v2_8_nsxt_pool_delete["v2.8.nsxt.pool.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_9_fortinet_vip_delete["v2.9.fortinet.vip.delete"] -->|next| v2_10_fortinet_snat_delete["v2.10.fortinet.snat.delete"]
  v2_9_fortinet_vip_delete["v2.9.fortinet.vip.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_10_fortinet_snat_delete["v2.10.fortinet.snat.delete"] -->|next| v2_11_fortinet_ippool_delete["v2.11.fortinet.ippool.delete"]
  v2_10_fortinet_snat_delete["v2.10.fortinet.snat.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_11_fortinet_ippool_delete["v2.11.fortinet.ippool.delete"] -->|next| v2_12_fortinet_address_delete["v2.12.fortinet.address.delete"]
  v2_11_fortinet_ippool_delete["v2.11.fortinet.ippool.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
  v2_12_fortinet_address_delete["v2.12.fortinet.address.delete"] -->|next| v2_public_address_delete["v2.public.address.delete"]
  v2_12_fortinet_address_delete["v2.12.fortinet.address.delete"] -->|error| v2_public_address_delete["v2.public.address.delete"]
```
