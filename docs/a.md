# L2/L3 Network Plan
I am going to follow general good practice and use VLANs to enforce application seperation and enhance security.

- I will configure IPv4 only to begin, but I will keep the same design pattern for IPv6.
- I will not use VLAN1, security best practice.
- I will group VLANs in groups of 16 for now as a best guess, the first group of VLANs will be reserved for infrastructure.
- Each VLAN will be /24.
- I will have at least 3 routers to each VLAN using router redundancy (HSRP/VRRP). .1, .2, .3
- The shared gateway address for these routers will be .20 in every VLAN.
- I will use .21-.99 in each VLAN for statics.
- I will use .110-.199 in each VLAN for DHCP. In practice, this implies 100 nodes per VLAN as a design specification.
- I will reserve addresses over .200 in each VLAN. This will give me some headroom if I run out of addresses!

The table blow summarizes the VLANs I will create.

| VLAN  | Purpose                |
| ------| ---------------------- |
|  1    | Unused                 |
|  2    | Hosts                  |
|  5    | Clients (technicians)  |
|  6   | Services                |

Each host and server will have a fixed IP address, and it will use the same last octet in any VLAN.

- Hosts will be in the range .21-30
- Domain Controllers in the range .31-.40
- Storage in the range .41-.50
- Database servers in the range .51-.60
- Application servers in the range .61-.70
