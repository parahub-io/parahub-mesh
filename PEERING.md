# Parahub Mesh — Peering Agreement

**Status: v0.10 — DRAFT (pending legal review). Not yet effective.**

Sections 1–4 and the *Definition of terms* below are the **Pico Peering Agreement
v1.0**, maintained by volunteers at <http://picopeer.net> and reproduced here
verbatim. Section 5 (*Local Amendments*) is completed with Parahub's local terms,
exactly as the PPA template intends. The PPA is designed to be reused as a template
for small-scale peering documents and licences.

Canonical source:
<https://github.com/freifunk/PicoPeeringAgreement/blob/master/PPAv1.0/PPA1.0-en.md>

---

# Pico Peering Agreement v1.0

## Preamble

There are now many community networks, but they are seperated geographically and
socially and do not form a coherent network. This document is an attempt to connect
those network islands by providing the minimum baseline template for a peering
agreement between owners of individual network nodes - the Pico Peering Agreement.

The PPA is a way of formalizing the interaction between two peers. Owners of network
nodes assert their right of ownership by declaring their willingness to donate the
free exchange of data across their networks.

The PPA is maintained at http://picopeer.net by a group of volunteers from around the
world. It is intended to be used as a template for other small-scale peering documents
and licenses.

## Agreement

### 1. Free Transit:

* The owner agrees to provide free transit accross their free network.
* The owner agrees not to modify or interfere with data as it passes through their
  free network.

### 2. Open Communication:

* The owner agrees to publish the information necessary for peering to take place
* This information shall be published under a free licence
* The owner agrees to be contactable and will provide at least an email adress

### 3. No Warranty:

* There is no guaranteed level of service
* The service is provided "as is", with no warranty or liability of whatsoever kind
* The service can be scaled back or withdrawn at any time with no notice

### 4. Terms of Use:

* The owner is entitled to formulate an 'acceptable use policy'
* This may or may not contain information about additional services provided (apart
  from basic access)
* The owner is free to formulate this policy as long as it does not contradict points
  1 to 3 of this agreement (see point 5)

### 5. Local Amendments:

*For Parahub mesh nodes, this section is completed as follows. It does not alter
points 1–4: every service or limit below is introduced under point 4 (Terms of Use /
Additional Services) and, where it restricts the Service, under point 3 (the owner's
right to scale back or withdraw it).*

* **5.1 Additional Services.** Beyond Free Transit, Parahub *Bumblebee* nodes may
  provide guests with DHCP, encrypted DNS (DoH — resolved locally, with Cloudflare /
  Google as an upstream fallback only), guest isolation, best-effort traffic
  management, and a VPN-tunnelled internet exit. Each is an "Additional Service" in the
  sense of point 4 and the Definition of terms — offered over and above, and without
  displacing, the Free Transit of point 1.
* **5.2 Traffic management (net neutrality).** Any bandwidth shaping is best-effort,
  content-neutral, and applied solely for network stability and fair resource sharing,
  consistent with Regulation (EU) 2015/2120 on open-internet access. It is a management
  measure under point 3, not interference with the content of data under point 1.
* **5.3 Paid capacity.** The base tier of guest access is provided free of charge and
  is Free Transit within the meaning of point 1. Throughput above that free tier may be
  offered as an optional, separately purchased Additional Service (for example, via
  Lightning); charging for this optional capacity is not a charge for Free Transit.
* **5.4 Acceptable use.** Guests must not use the network for unlawful activity, network
  attacks, or abuse. Per point 3, access may be scaled back or withdrawn at any time
  without notice.
* **5.5 Egress / mere-conduit.** Guest traffic egresses to the open internet from the IP
  address of a VPN gateway — by default a Parahub gateway whose own exit is a commercial VPN
  provider located in Portugal, or a VPN account the node owner has configured directly on
  the node — and never from a node owner's own public IP address. The encrypted tunnel traverses the owner's access line, but
  the owner does not originate that traffic, choose its recipient, or modify it, and so acts
  solely as an intermediary conduit ("simples transporte") under Article 14 of Decreto-Lei
  7/2004 (transposing Directive 2000/31/EC) and Article 4 of Regulation (EU) 2022/2065
  (Digital Services Act). If a node's own tunnel is unavailable, its guest traffic may cross
  the mesh and egress through another node's tunnel; if neither a tunnel nor a mesh gateway
  can be reached, guest traffic is isolated (no internet).
* **5.6 The Association.** Parahub — Associação (NIPC 519 190 904) publishes and
  maintains these amendments and operates the shared VPN egress. In respect of that
  egress it likewise acts as a mere conduit under the instruments cited in 5.5, and will
  respond to lawful requests and abuse notices addressed to the egress operator in
  accordance with Articles 12–13 of Decreto-Lei 7/2004. To the maximum extent permitted
  by applicable mandatory law, the Association is not a party to, and assumes no
  liability for, the transit traffic of guests or nodes.
* **5.7 Data protection.** Nodes send a periodic heartbeat carrying technical and network
  data: hardware and MAC identifiers, node keys, firmware version, connection status and
  throughput counters, and — from the node that terminates the guest network — the current
  guest DHCP leases (WiFi address (MAC), local IP, and the hostname the device announces
  itself under). Guest leases are what allows a full-speed pass to be applied to the right
  device; they are held as current state only, each heartbeat replacing the previous
  snapshot, and are not accumulated into a history. Guest traffic itself is neither
  inspected nor logged. A node's location may be shown on the public coverage map only with
  the owner's consent, and the owner is named there only on explicit opt-in. The
  Association, as operator of the egress, processes the minimum connection metadata needed
  to run the service. Processing follows Regulation (EU) 2016/679 (GDPR); the controller
  roles, retention, and logging position are set out in the Parahub Privacy Policy
  (<https://parahub.io/about/privacy>), which forms part of these terms.
* **5.8 Non-profit operation.** The mesh is run by a non-profit association on a community
  basis, to donate and sustain Free Transit. The Additional Services exist to secure and
  maintain the network. The base tier of guest access is free of charge, and full-speed
  access can also be earned without paying for it; where optional capacity above the free
  tier is paid for (5.3), the payment is received by the Association and goes to the shared
  infrastructure. The Association takes no share of transit and no percentage of anything
  that passes over the mesh.
* **5.9 Governing law and scope.** This Agreement is governed by the law of Portugal,
  and the competent Portuguese courts have jurisdiction — without prejudice to any
  mandatory protection or forum a consumer may have under their local law. Point 5.5
  describes the network's architecture and the legal position of nodes operated in
  Portugal; it is not a warranty of any particular liability regime for nodes operated
  elsewhere, whose owners remain subject to their local law.
* **5.10 Limitation and severability.** The exclusions of warranty and liability in
  point 3 apply only to the extent permitted by applicable mandatory law — including
  consumer-protection rules, the regime on standard contract terms (Decreto-Lei 446/85),
  and Article 809 of the Civil Code. If any provision of this section is held invalid,
  the remainder stays in force.
* **5.11 Acceptance and versioning.** This document is versioned; material changes are
  published before they take effect. Flashing Parahub mesh firmware and operating a node
  on the mesh constitutes the owner's acceptance of the version in force. While this
  document is marked "not yet effective", no acceptance is sought and nothing in it binds
  a node owner.

## Definition of terms

* Owner: The owner of the node has the right to operate their network equipment and to
  donate any part of its functionality to the FreeNetwork
* Transit: Transit is the exchange of data into, out of or across a network
* Free Transit: Free transit means that the owner will neither charge for the transit
  of data nor modify the data
* Free Network: The Free Network is the sum of interconnected hardware and software
  resources, whose FreeTransit has been donated by the owners of those resources
* The Service: The Service is made up of Free transit and Additional Services
* Additional Services: In terms of the PPA, an additional service is anything over and
  above Free Transit. For example, provision of a DHCP server, a web server or a mail
  server

## The PPA in practice

The PPA shall be implemented in data readable form following agreed standards in
community network node data bases to facilitate automatic interconnection of nodes.

---

*Sections 1–4 and the Definition of terms are the Pico Peering Agreement v1.0, released
under CC0 (public domain) and reproduced verbatim (original spelling preserved).
Section 5 (Local Amendments) is © Parahub — Associação (NIPC 519 190 904).*
