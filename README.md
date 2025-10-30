# EVPN-VXLAN Interworking Lab

![Network Diagram](diagram.png)

This lab demonstrates **EVPN-VXLAN interworking** between **SR Linux–based 7720 IXR** nodes (leaf and spine) and an **SROS-based 7750 SR-1** acting as the **Data Center Gateway (DCGW)**.

A **Layer 2 EVPN (MAC-VRF)** spans across the leaf switches up to the DCGWs. The **spines** are not service-aware—they simply route VXLAN tunnels.

On the DCGWs, a **Layer 3 VRF (VPRN)** includes an **IRB interface** attached to the L2 EVPN (MAC-VRF on SR Linux / VPLS on SROS).

Both DCGWs share the same **gateway IP** using **passive VRRP** (VRRP without signaling).

- **Anycast GW IP:** `100.99.98.1`  
- **Client 1 IP:** `100.99.98.2`  
- **Client 2 IP:** `100.99.98.3`  
- **Rogue Client:** Spoofs the GW IP and MAC

Because **proxy-ARP** is enabled and the **GW MAC/IP** is advertised as **EVPN-static**, spoofing attempts fail. The network consistently prefers the legitimate GW entry, ignoring rogue traffic. The rogue client’s MAC is not learned, and its IP is excluded from the EVPN domain due to overlap with the GW.  
This mechanism secures the EVPN domain against both **spoofing** and **misconfiguration**.
