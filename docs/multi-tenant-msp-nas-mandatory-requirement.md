# Mandatory Requirement: Multi-Tenant Enterprise NAS in Managed Service Provider (MSP) Mode

The proposed Enterprise Scale-out NAS solution must operate as a **multi-tenant platform in Managed Service Provider (MSP) mode**: a single shared scale-out storage system that concurrently delivers file services to multiple tenants, where each tenant and its associated file services are subject to **full isolation** across identity, network, security, namespace and administrative boundaries.

Within SNB’s multi-domain Active Directory environment, the solution must natively provide file services to multiple Active Directory (AD) domains (and, where applicable, SNB-approved trusted domains and trusted forests) from that shared platform, without collapsing tenants into a single network or security context.

## The solution must

1. **Operate as a multi-tenant MSP-mode NAS**  
   Concurrently host and serve multiple tenants from one shared scale-out platform, with each tenant treated as an independent consumer of file services (SMB and/or NFS), while preserving capacity consolidation across the platform.

2. **Provide full tenant and tenant-service isolation**  
   Isolate each tenant—and each tenant service instance associated with that tenant—across identity, authentication/authorization, networking, namespaces and administration, such that one tenant’s users, traffic or configuration cannot access or affect another tenant’s context.

3. **Isolate tenant services by VLAN**  
   Bind each tenant service (or designated service instance) to one or more SNB-assigned VLAN-tagged network contexts, including dedicated network interfaces (logical or physical as applicable), IP subnets and designated gateways appropriate to that tenant’s security boundary.

4. **Provide independent routing per service-to-VLAN relationship (VRF-based / source-aware routing)**  
   For each tenant service–to–VLAN relationship, maintain an **independent routing table** (VRF or equivalent) so that ingress and egress traffic for that service is forwarded only within its assigned network context. Routing must be **deterministic and symmetric**: return traffic must use the same tenant network path, and traffic belonging to one tenant must not be forwarded through another tenant’s gateway, VLAN or routing table. This behaviour must be native to the NAS platform’s multi-tenant networking model (or clearly identified equivalent), and must not rely on ambiguous or vendor-proprietary naming alone.

5. **Support multi-domain and trusted-domain identity**  
   Concurrently integrate with multiple AD domains within SNB’s multi-domain Active Directory environment; authenticate and authorize users and groups from SNB-approved trusted domains (and trusted forests where required) for cross-domain or cross-forest access, without breaking tenant isolation.

6. **Preserve stable tenant namespaces and centralized MSP administration**  
   Provide stable, tenant-scoped namespaces and allow centralized administration of the shared platform while retaining per-tenant isolation of identity, network and security controls.

7. **Consolidate capacity without per-tenant hardware silos**  
   Share storage capacity across tenants without requiring SNB to permanently deploy and separately size a physical storage platform for each AD domain or tenant.

8. **Survive lifecycle and DR operations without losing isolation**  
   Preserve the required multi-tenant identity, VLAN, VRF/independent-routing, namespace and security characteristics during normal operations, maintenance, capacity expansion, software upgrades and supported DR/failover operations.

9. **Disclose any non-NAS components**  
   Standard configuration of SNB network infrastructure (VLAN trunks, switches, firewalls and routing/VRF) is permitted. Any additional gateway, proxy, appliance, virtual machine, service, licence or third-party component required to satisfy this mandatory requirement must be clearly identified by the bidder and included in the proposed architecture, pricing, support model and lifecycle plan.

## Non-compliance

A proposed solution will be considered non-compliant if it:

- Does not operate as a true multi-tenant MSP-mode platform with full isolation of tenants and tenant services;
- Cannot isolate tenant services via VLAN tags with a defined service-to-VLAN binding;
- Cannot provide an **independent routing table per service-to-VLAN relationship** (VRF or equivalent) with deterministic, symmetric, tenant-scoped forwarding (i.e. cannot prevent traffic from one tenant being routed via another tenant’s network context);
- Requires a separate physical storage platform for each AD domain or tenant;
- Cannot maintain the required tenant-specific identity and network isolation;
- Cannot support required cross-domain authentication and authorization without disruption to isolation or service; or
- Requires SNB to independently integrate and lifecycle-manage additional general-purpose compute infrastructure in order to provide the core multi-tenant NAS service.

## Technical Rationale / Existing SNB Design Characteristics

SNB operates as a government-wide service provider delivering file-storage services across multiple organizational, security and network boundaries—functionally an **MSP multi-tenant** delivery model. SNB’s current Active Directory environment is a single forest containing multiple domains, with departmental networks that have distinct network, authentication and security requirements. Trusted-forest integration may also be required for specific approved use cases.

SNB’s existing scale-out NAS deployment consolidates file services onto a shared platform while maintaining separate identity and network contexts per organization. In that model, tenant services are presented on distinct VLAN-tagged interfaces, and each service’s traffic is associated with an independent routing context so that forwarding remains tenant-specific and does not cross another tenant’s network/security boundary. The mandatory requirement above is intended to capture that **multi-tenant MSP isolation model**—including VLAN isolation and per-service independent routing (VRF / source-aware routing)—in vendor-neutral terms, so that equivalent architectures can be evaluated without relying on any single product’s terminology.

## Design intent checklist

| Theme | How it is expressed |
|---|---|
| MSP multi-tenant NAS | Shared platform, concurrent tenants, MSP-mode operation |
| Full isolation | Identity, network, security, namespace, admin |
| VLAN isolation | Explicit service ↔ VLAN binding |
| Independent routing | Per service–VLAN relationship: VRF (or equivalent) + deterministic/symmetric forwarding |
| Avoid product jargon | “Source-based routing” → independent routing table / VRF / source-aware, tenant-scoped routing |
