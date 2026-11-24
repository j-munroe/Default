# Enterprise NAS Systems — MSP Multi-Tenant Requirement Shortlist

Market research shortlist of Enterprise NAS platforms evaluated against the mandatory requirement in [`multi-tenant-msp-nas-mandatory-requirement.md`](./multi-tenant-msp-nas-mandatory-requirement.md).

**Scope of the requirement (summary):** MSP-mode multi-tenant scale-out NAS with full tenant isolation, VLAN-tagged tenant services, independent routing per service-to-VLAN relationship (VRF or equivalent / source-aware routing), and multi-domain Active Directory integration on a shared platform.

> This is a shortlist for RFP validation—not a compliance certification. Bidder claims must still be proven against SNB’s environment and the mandatory wording.

## Strongest apparent fit

| Platform | Tenant / isolation model | VLAN | Independent routing (VRF or equivalent) | Multi-domain AD |
|---|---|---|---|---|
| **Dell PowerScale (OneFS)** | Access Zones + groupnets/IP pools | Yes | **Source-based routing (SBR)** — not VRF; OneFS docs state VRF/VXLAN are not supported; SBR keeps replies on the matching tenant path | One AD provider per Access Zone; multiple zones/domains on one cluster |
| **NetApp ONTAP** | Storage VMs (SVMs) + IPspaces | Yes | **Per-SVM routing tables**; IPspaces for SSP/MSP overlapping networks — closest native “VRF” analogue | One AD domain per data SVM; different SVMs can join different domains |
| **Hitachi HNAS / VSP One File** | Enterprise Virtual Servers (EVS) + multi-tenancy mode | Yes (VLAN interfaces) | **Per-EVS routing** when multi-tenancy is enabled | Per-EVS AD with Virtual Server Security license |

## Plausible / needs deep diligence

| Platform | Why it may qualify | Main risk vs the mandatory wording |
|---|---|---|
| **Pure Storage FlashBlade** | “Servers” + Realms: per-tenant interfaces, VLANs, exports, AD accounts | Independent **routing table / VRF** on the array is less clearly documented; AD egress often via management path |
| **VAST Data** | Tenants with dedicated VIP pools, VLAN tagging, per-tenant identity providers | VRF is often described with the **network fabric**, not always as a native NAS VRF |
| **Dell Unity XT** | Explicit **IP multi-tenancy**: per-tenant VLAN domain, routing table, firewall, DNS | Midrange dual-controller; may struggle as a true **enterprise scale-out** MSP platform |
| **Dell PowerStore (File)** | NAS servers for logical multi-tenancy; port/bond isolation | Weaker public evidence of per-tenant independent routing tables |

## Likely weak / non-compliant as commonly sold

| Platform | Why |
|---|---|
| **Qumulo Core** | Network tenants/VLANs exist, but **all tenants share one AD/identity provider** — fails multi-domain isolation |
| **Qumulo Stratus** | Stronger per-tenant AD isolation, but uses **dedicated per-tenant compute instances** — may hit the “no extra general-purpose compute” non-compliance clause |
| **IBM Storage Scale** | Scale-out file + CES VLAN/node groups possible; **native MSP-style per-tenant VRF + multi-AD** is less clearly productized than PowerScale/ONTAP/HNAS |

## Practical RFP bidder set

Platforms that can realistically answer “yes” to MSP isolation + VLAN + independent routing + multi-AD without stretching definitions:

1. **Dell PowerScale** (incumbent; SBR = source-aware routing equivalent)
2. **NetApp ONTAP** (SVM + IPspace; strongest VRF-equivalent language)
3. **Hitachi HNAS / VSP One File** (EVS multi-tenancy + per-EVS routes)
4. **Pure FlashBlade** and **VAST Data** as stretch/challenger options — force explicit proof of per-service independent routing tables, not just VLAN tagging

## Key diligence question for every bidder

Does the NAS itself maintain an **independent routing table per tenant service ↔ VLAN**, or does isolation stop at L2 VLAN tagging and rely on SNB’s network VRF?

## Selected public references (research basis)

- Dell PowerScale Access Zones / multi-tenant networking and source-based routing (OneFS network design materials)
- NetApp ONTAP IPspaces and per-SVM routing documentation
- Hitachi HNAS multi-tenancy / per-EVS routing documentation
- Pure FlashBlade Servers / Realms multi-tenancy materials
- VAST Data tenant / VIP pool / VLAN multi-tenancy documentation
- Qumulo Core network multitenancy known limits (shared identity providers)
