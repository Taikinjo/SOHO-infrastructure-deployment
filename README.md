## Quickstart
1. Open `soho_deployment.pka` in Cisco Packet Tracer 8.x.
2. Wait for links to go green.
3. On the router:
   - `show ip interface brief` → confirm subinterfaces are up/up
   - `show ip dhcp binding` → see leases after clients request IPs
   - `show ip route` → confirm connected VLANs + default route
4. From a PC (Admin VLAN): ping 192.168.20.1 and 8.8.8.8.


## Topology & Screens
- Topology: `assets/topology.png`
- Core switch status: `assets/sw-core-interface-status.png`
- Router interfaces: `assets/router-interface-brief.png`
- DHCP bindings: `assets/dhcp_bindings.png`
- Routing table: `assets/ip_route.png`

## Config Exports
- Show_ip_interface_brief.txt: `configs/show_ip_interface_brief.txt`
- Show_ip_route.txt: `configs/show_ip_route.txt`
- Sw-core_show_run.txt: `configs/sw-core_show_run.txt`
