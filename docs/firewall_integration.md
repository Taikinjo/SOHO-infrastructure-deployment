# Firewall Integration (ASA 5505) — SOHO Upgrade
Added a Cisco ASA 5505 to the existing SOHO topology to implement perimeter defense between the router and the ISP. Configurations include NAT, zone separation (inside/outside), ICMP allowance, and basic ACL setup. Fully reproducible using Cisco Packet Tracer.

---

## Objectives
- Insert the firewall at the perimeter without changing existing VLANs (10/20/30/40/99).
- Transfer NAT/PAT handling to the ASA to prevent double NAT on the router.
- Clearly define inside/outside zones with quick verification commands.
- Include key troubleshooting steps for rapid issue isolation.

---

## Topology Delta (What Changed)
- Added **ASA 5505** between router `G0/1` and the ISP.
- Physical connections:
  - `ASA e0/0` → **outside** → router `G0/1` (203.0.113.0/30)
  - `ASA e0/1` → **inside** → SW-Core (reaches VLAN 99 subnet)
- From this point, NAT is handled by the ASA. The router’s `ip nat inside source ... g0/1 overload` command is disabled.

---

## Addressing (Existing VLANs + New ASA IPs)
- Inside network: `192.168.99.0/24`
  - ASA inside: `192.168.99.254`
  - Router SVI: `192.168.99.1`
- Outside network: `203.0.113.0/30`
  - ASA outside: `203.0.113.1`
  - Router outside: `203.0.113.2`

---

## ASA Configuration (Packet Tracer 5505, New-NAT Syntax)
> The ASA 5505 uses an internal 8-port switch. Layer 3 IP addresses are applied to VLAN interfaces, while physical ports are assigned to VLANs.

```bash
enable
conf t

! VLAN SVIs (Layer 3)
interface Vlan1
 nameif inside
 security-level 100
 ip address 192.168.99.254 255.255.255.0
 no shut
!
interface Vlan2
 nameif outside
 security-level 0
 ip address 203.0.113.1 255.255.255.252
 no shut

! Switchport-to-VLAN mapping
interface Ethernet0/0
 switchport access vlan 2   ! to Router G0/1
 no shut
interface Ethernet0/1
 switchport access vlan 1   ! to SW-Core
 no shut

! Default route to router
route outside 0.0.0.0 0.0.0.0 203.0.113.2

! NAT/PAT (new syntax)
object network INSIDE-NET
 subnet 192.168.0.0 255.255.0.0
 nat (inside,outside) dynamic interface

! Allow ICMP for testing
icmp permit any inside
icmp permit any outside

end
wr mem
