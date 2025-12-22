# SDN-VPN

**Objetivo:** Criar e gerenciar uma rede privada virtual (VPN) utilizando Software-Defined Networking (SDN) para melhorar a segurança, flexibilidade e gerenciamento da rede.

## Fluxo - SDN-VPN Create

```mermaid
flowchart TB
  v2_1_nsxt_vpn_service_create["v2.1.nsxt.vpn.service.create"] -->|next| v2_2_nsxt_vpn_endpoint_create["v2.2.nsxt.vpn.endpoint.create"]
  v2_1_nsxt_vpn_service_create["v2.1.nsxt.vpn.service.create"] -->|error| v2_5_nsxt_vpn_service_create_error_delete["v2.5.nsxt.vpn.service.create.error.delete"]
  v2_2_nsxt_vpn_endpoint_create["v2.2.nsxt.vpn.endpoint.create"] -->|next| v2_3_nsxt_vpn_session_create["v2.3.nsxt.vpn.session.create"]
  v2_2_nsxt_vpn_endpoint_create["v2.2.nsxt.vpn.endpoint.create"] -->|error| v2_4_nsxt_vpn_endpoint_create_error_delete["v2.4.nsxt.vpn.endpoint.create.error.delete"]
  v2_3_nsxt_vpn_session_create["v2.3.nsxt.vpn.session.create"] -->|next| v2_4_paloalto_host_create["v2.4.paloalto.host.create"]
  v2_3_nsxt_vpn_session_create["v2.3.nsxt.vpn.session.create"] -->|error| v2_3_nsxt_vpn_session_create_error_delete["v2.3.nsxt.vpn.session.create.error.delete"]
  v2_4_paloalto_host_create["v2.4.paloalto.host.create"] -->|next| v2_5_paloalto_rule_create["v2.5.paloalto.rule.create"]
  v2_4_paloalto_host_create["v2.4.paloalto.host.create"] -->|error| v2_2_paloalto_host_create_error_delete["v2.2.paloalto.host.create.error.delete"]
  v2_5_paloalto_rule_create["v2.5.paloalto.rule.create"] -->|next| v2_6_paloalto_rule_edit["v2.6.paloalto.rule.edit"]
  v2_5_paloalto_rule_create["v2.5.paloalto.rule.create"] -->|error| v2_1_paloalto_rule_create_error_delete["v2.1.paloalto.rule.create.error.delete"]
  v2_6_paloalto_rule_edit["v2.6.paloalto.rule.edit"] -->|next| v2_7_paloalto_router_edit["v2.7.paloalto.router.edit"]
  v2_6_paloalto_rule_edit["v2.6.paloalto.rule.edit"] -->|error| v2_1_paloalto_rule_create_error_delete["v2.1.paloalto.rule.create.error.delete"]
  v2_7_paloalto_router_edit["v2.7.paloalto.router.edit"] -->|next| v2_vpn_create["v2.vpn.create"]
  v2_7_paloalto_router_edit["v2.7.paloalto.router.edit"] -->|error| v2_1_paloalto_rule_create_error_delete["v2.1.paloalto.rule.create.error.delete"]
  v2_1_paloalto_rule_create_error_delete["v2.1.paloalto.rule.create.error.delete"] -->|next| v2_2_paloalto_host_create_error_delete["v2.2.paloalto.host.create.error.delete"]
  v2_1_paloalto_rule_create_error_delete["v2.1.paloalto.rule.create.error.delete"] -->|error| v2_vpn_create["v2.vpn.create"]
  v2_2_paloalto_host_create_error_delete["v2.2.paloalto.host.create.error.delete"] -->|next| v2_3_nsxt_vpn_session_create_error_delete["v2.3.nsxt.vpn.session.create.error.delete"]
  v2_2_paloalto_host_create_error_delete["v2.2.paloalto.host.create.error.delete"] -->|error| v2_vpn_create["v2.vpn.create"]
  v2_3_nsxt_vpn_session_create_error_delete["v2.3.nsxt.vpn.session.create.error.delete"] -->|next| v2_4_nsxt_vpn_endpoint_create_error_delete["v2.4.nsxt.vpn.endpoint.create.error.delete"]
  v2_3_nsxt_vpn_session_create_error_delete["v2.3.nsxt.vpn.session.create.error.delete"] -->|error| v2_vpn_create["v2.vpn.create"]
  v2_4_nsxt_vpn_endpoint_create_error_delete["v2.4.nsxt.vpn.endpoint.create.error.delete"] -->|next| v2_5_nsxt_vpn_service_create_error_delete["v2.5.nsxt.vpn.service.create.error.delete"]
  v2_4_nsxt_vpn_endpoint_create_error_delete["v2.4.nsxt.vpn.endpoint.create.error.delete"] -->|error| v2_vpn_create["v2.vpn.create"]
  v2_5_nsxt_vpn_service_create_error_delete["v2.5.nsxt.vpn.service.create.error.delete"] -->|next| v2_vpn_create["v2.vpn.create"]
  v2_5_nsxt_vpn_service_create_error_delete["v2.5.nsxt.vpn.service.create.error.delete"] -->|error| v2_vpn_create["v2.vpn.create"]
```

### Serviços envolvidos

- [v2.4.paloalto.host.create](paloalto-host.md#fluxo---host-create)
- [v2.5.paloalto.rule.create](paloalto-rule.md#fluxo---rule-create)
- [v2.6.paloalto.rule.edit](paloalto-rule.md#fluxo---rule-edit)
- [v2.7.paloalto.router.edit](paloalto-router.md#fluxo---router-edit)

## Fluxo - SDN-VPN Delete

```mermaid
flowchart TB
  v2_1_nsxt_vpn_session_delete["v2.1.nsxt.vpn.session.delete"] -->|next| v2_2_nsxt_vpn_endpoint_delete["v2.2.nsxt.vpn.endpoint.delete"]
  v2_1_nsxt_vpn_session_delete["v2.1.nsxt.vpn.session.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_2_nsxt_vpn_endpoint_delete["v2.2.nsxt.vpn.endpoint.delete"] -->|next| v2_3_nsxt_vpn_service_delete["v2.3.nsxt.vpn.service.delete"]
  v2_2_nsxt_vpn_endpoint_delete["v2.2.nsxt.vpn.endpoint.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_3_nsxt_vpn_service_delete["v2.3.nsxt.vpn.service.delete"] -->|next| v2_4_paloalto_rule_edit["v2.4.paloalto.rule.edit"]
  v2_3_nsxt_vpn_service_delete["v2.3.nsxt.vpn.service.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_4_paloalto_rule_edit["v2.4.paloalto.rule.edit"] -->|next| v2_5_paloalto_rule_delete["v2.5.paloalto.rule.delete"]
  v2_4_paloalto_rule_edit["v2.4.paloalto.rule.edit"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_5_paloalto_rule_delete["v2.5.paloalto.rule.delete"] -->|next| v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"]
  v2_5_paloalto_rule_delete["v2.5.paloalto.rule.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"] -->|next| v2_7_paloalto_router_edit["v2.7.paloalto.router.edit"]
  v2_6_paloalto_host_delete["v2.6.paloalto.host.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_7_paloalto_router_edit["v2.7.paloalto.router.edit"] -->|next| v2_8_fortinet_vip_delete["v2.8.fortinet.vip.delete"]
  v2_7_paloalto_router_edit["v2.7.paloalto.router.edit"] -->|error| v2_1_paloalto_rule_create_error_delete["v2.1.paloalto.rule.create.error.delete"]
  v2_8_fortinet_vip_delete["v2.8.fortinet.vip.delete"] -->|next| v2_9_fortinet_snat_edit["v2.9.fortinet.snat.edit"]
  v2_8_fortinet_vip_delete["v2.8.fortinet.vip.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_9_fortinet_snat_edit["v2.9.fortinet.snat.edit"] -->|next| v2_10_fortinet_snat_delete["v2.10.fortinet.snat.delete"]
  v2_9_fortinet_snat_edit["v2.9.fortinet.snat.edit"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_10_fortinet_snat_delete["v2.10.fortinet.snat.delete"] -->|next| v2_11_fortinet_ippool_delete["v2.11.fortinet.ippool.delete"]
  v2_10_fortinet_snat_delete["v2.10.fortinet.snat.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
  v2_11_fortinet_ippool_delete["v2.11.fortinet.ippool.delete"] -->|next| v2_vpn_delete["v2.vpn.delete"]
  v2_11_fortinet_ippool_delete["v2.11.fortinet.ippool.delete"] -->|error| v2_vpn_delete["v2.vpn.delete"]
```
