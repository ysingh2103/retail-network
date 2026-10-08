# Retail Network Lab

An interactive 3D model of a retail company's network: one head office and a 23-store chain, with every device wired the way a real retail environment would be. Click any device to inspect it, watch traffic move hop by hop, and work through realistic outages from first symptom to root cause.

**Live demo:** https://ysingh2103.github.io/retail-network-lab/

> Built as a hands-on study of networking fundamentals and IT support for a multi-site retail environment. All addressing, hardware and scenarios are illustrative.

---

## What's modeled

**Head office**
- Fibre ONT → firewall (HA pair) → Layer 3 core switch → access switches
- Separate VLANs for staff, servers, AV and guest Wi-Fi
- Windows PCs, MacBooks on Wi-Fi, printer, DNS/print server, NAS
- Boardroom with a Teams Room, display, and camera/mic bar

**Store template** (3 shown, representing all 23)
- Two internet paths: cable (WAN1) and LTE (WAN2), with SD-WAN failover
- Firewall, PoE switch, Wi-Fi AP, IP camera
- Two Shopify POS registers with Bluetooth card readers on an isolated POS VLAN
- Encrypted site-to-site IPsec VPN back to head office

**Cloud services**
- Shopify POS and Payments, Microsoft 365 + Entra ID, public DNS, off-site backup, and a monitoring / MDM platform

## Features

| | |
|---|---|
| **Traffic flows** | Card payment, inventory sync, store-to-HQ over VPN, guest Wi-Fi, Teams call, DNS lookup, printing, MDM check-in, nightly backup. Each hop is explained. |
| **Device inspector** | IP, VLAN, hardware, connections, and what to check when it fails. |
| **Incident drills** | 9 tickets: ISP outage with a dead LTE backup, unpaired card reader, full DHCP scope, DNS service down, boardroom "No Signal", phishing + MFA fatigue, switching loop, payment-processor outage, VPN broken by a firmware upgrade. Run checks (some are risky), diagnose, then apply the fix and watch traffic recover. |
| **IT role map** | Each responsibility of a retail IT specialist role mapped to where it lives in the network. |
| **Docs** | IP and VLAN plan, hardware inventory, disaster recovery targets (RTO / RPO), and runbooks for network upgrades, new store installs and evaluating new software. |
| **Guided tour** | A 9-step walkthrough of the whole design. |

## Design decisions

- **One template per store.** Store NN uses `10.NN.0.0/16`, so a device's IP tells you where it is. Gateway is always `.1`.
- **POS is isolated.** The POS VLAN can reach only Shopify and the payment processor, which keeps PCI DSS scope small.
- **Guest reaches the internet only.** Client isolation on; no access to POS, staff devices or the VPN.
- **Two internet paths per store.** A cable outage moves payments to LTE in seconds, so the store keeps selling.
- **Reservations, not static IPs,** for registers, printers and cameras. Static IPs only for core infrastructure.
- **3-2-1 backups** with a quarterly restore test.

| VLAN | Purpose | HQ | Store NN |
|---|---|---|---|
| 10 | POS | — | 10.NN.10.0/24 |
| 20 | Staff | 10.0.20.0/24 | 10.NN.20.0/24 |
| 30 | Servers | 10.0.30.0/24 | — |
| 40 | AV | 10.0.40.0/24 | — |
| 50 | Cameras | — | 10.NN.50.0/24 |
| 60 | Guest | 10.0.60.0/24 | 10.NN.60.0/24 |
| 99 | Management | 10.0.99.0/24 | 10.NN.99.0/24 |

## Tech

A single self-contained `index.html`: vanilla JavaScript and [three.js](https://threejs.org/) r128 loaded from a CDN. No build step.

Run it locally by opening `index.html` in a browser, or:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Author

**Yuvraj Singh** · IT support and networking, Toronto
[LinkedIn](https://linkedin.com/in/y-singh-gill) · [GitHub](https://github.com/ysingh2103)

Built with the help of an AI coding assistant; the network design, scenarios and troubleshooting reflect my own study and work experience.

## License

MIT
