# AZ-700 Study Notes – Azure Network Engineer Associate

> Beginner-friendly notes based on the uploaded AZ-700 course resources.  
> Designed for **GitHub README use**, **revision**, and **practical work understanding**.

---

## Table of Contents

- [How to Use These Notes](#how-to-use-these-notes)
- [1. Networking Fundamentals](#1-networking-fundamentals)
- [2. From Home Networking to Enterprise and Azure](#2-from-home-networking-to-enterprise-and-azure)
- [3. IP Addressing, Subnet Masks, and CIDR](#3-ip-addressing-subnet-masks-and-cidr)
- [4. Azure Core Networking Building Blocks](#4-azure-core-networking-building-blocks)
- [5. Azure IP Addressing Concepts](#5-azure-ip-addressing-concepts)
- [6. DNS Fundamentals and Azure DNS](#6-dns-fundamentals-and-azure-dns)
- [7. Connectivity Services](#7-connectivity-services)
- [8. Application Delivery Services](#8-application-delivery-services)
- [9. Private Access to Azure Services](#9-private-access-to-azure-services)
- [10. Azure Network Security Services](#10-azure-network-security-services)
- [11. Monitoring and Troubleshooting](#11-monitoring-and-troubleshooting)
- [12. Important Comparisons for the Exam](#12-important-comparisons-for-the-exam)
- [13. Common Real-World Scenarios](#13-common-real-world-scenarios)
- [14. Exam-Focused Revision Sheet](#14-exam-focused-revision-sheet)
- [15. Quick Glossary](#15-quick-glossary)

---

## How to Use These Notes

These notes are written to help you in two ways:

1. **Pass the exam**
   - Focus on what each service does.
   - Understand when to choose one service over another.
   - Learn the differences between services that look similar.

2. **Understand real work scenarios**
   - How traffic flows.
   - How to secure resources.
   - How Azure networking services are combined in real environments.

### Best revision method

- Read the section once for understanding.
- Re-read the comparison tables.
- Practice saying out loud:
  - **What problem does this service solve?**
  - **Where does traffic enter or leave?**
  - **Is this private or public connectivity?**
  - **Is this Layer 4 or Layer 7?**
  - **Is this for inbound, outbound, or both?**

---

# 1. Networking Fundamentals

## What is a network?

A network is a collection of connected devices that exchange data.

Examples from daily life:

- Phone using Wi-Fi
- Laptop browsing the internet
- Smart TV streaming video
- Gaming console connecting online

### Beginner mental model

Think of networking like a city:

- **Devices** = houses/buildings
- **IP addresses** = house addresses
- **Router** = traffic controller
- **DNS** = phonebook / map
- **Internet** = global road system

---

## The role of a router

In a home network, the router is usually the central device.

It often combines multiple roles:

- **Router** – sends traffic to the correct destination
- **Switch** – supports wired device connectivity
- **Wi-Fi access point** – provides wireless connectivity
- **DHCP server** – automatically assigns IP addresses
- **NAT device** – translates private IPs to a public IP for internet access

### Why this matters for Azure

Azure breaks these functions into cloud services and logical resources instead of one physical home router.

---

## IP addresses

Every device needs an IP address to communicate.

### Simple idea

An IP address is like a unique address for a device on a network.

### Two broad types

#### Private IP address

Used inside a private network.

Examples:

- `10.0.0.4`
- `192.168.0.20`

Characteristics:

- Not directly reachable from the public internet
- Used for internal communication
- Safer for internal workloads

#### Public IP address

Used to communicate over the internet.

Characteristics:

- Internet-routable
- Usually attached only where needed
- Must be protected carefully

---

## DHCP

**DHCP (Dynamic Host Configuration Protocol)** automatically gives devices IP settings.

Instead of manually configuring every device, DHCP provides:

- IP address
- Subnet mask
- Default gateway
- DNS server details

### Why DHCP is useful

- Faster onboarding of devices
- Less manual work
- Fewer mistakes
- Easier network management

---

## NAT

**NAT (Network Address Translation)** lets multiple private devices share a public IP address for internet access.

### Why NAT matters

- Hides internal private addresses
- Conserves public IP addresses
- Enables outbound internet access for internal devices

### Simple flow

1. Internal device sends traffic to the internet
2. NAT replaces the private source IP with a public IP
3. Response comes back to the public IP
4. NAT maps the response back to the original private device

### Key takeaway

NAT is extremely important in both normal networking and Azure networking.

---

# 2. From Home Networking to Enterprise and Azure

## Home network vs company network

### Home network

Usually:

- Small number of devices
- Flat design
- One router
- Simple management

### Company network

Usually:

- Hundreds or thousands of devices
- Servers, databases, apps, users
- Higher security requirements
- More segmentation
- More routing and traffic control

---

## Why enterprise networks need segmentation

A single large flat network causes problems:

- Too much broadcast/traffic noise
- Harder troubleshooting
- Poor isolation
- Bigger security blast radius

### Solution: subnets and segmentation

Organizations split networks into smaller logical sections.

Common tiered design:

- **Web tier**
- **Application tier**
- **Database tier**

This improves:

- Security
- Performance
- Manageability
- Traffic control

---

## How this relates to Azure

Azure uses the same networking ideas as on-premises networks, but in cloud form:

- Physical network equipment is abstracted away
- You design logical network boundaries
- Azure provides managed services to implement connectivity, routing, security, and delivery

---

# 3. IP Addressing, Subnet Masks, and CIDR

## IPv4 basics

An IPv4 address is a 32-bit number written in dotted decimal format.

Example:

`10.0.0.4`

It has 4 octets:

- 10
- 0
- 0
- 4

Each octet is 8 bits.

So each octet can range from:

- `0` to `255`

---

## Binary and decimal

Computers work with binary, but humans use decimal notation.

### Example

An octet is 8 bits:

`11000000`

This is one 8-bit value.

You do not need deep binary math for every exam question, but you **must** understand:

- IPs are binary underneath
- Subnet masks define which bits represent network vs host
- CIDR tells you subnet size

---

## Network ID, Subnet ID, Host ID

When you look at an IP address, parts of it represent different things.

### Network ID

Identifies the larger network.

### Subnet ID

Identifies a smaller network segment inside the larger network.

### Host ID

Identifies the specific device inside the subnet.

---

## Subnet mask

A subnet mask separates the network portion from the host portion of an IP address.

Example:

- IP: `10.0.0.4`
- Mask: `255.255.255.0`

This means the subnet is commonly written as:

- `10.0.0.0/24`

---

## CIDR notation

**CIDR (Classless Inter-Domain Routing)** expresses the subnet size using a suffix like `/24`.

### Examples

- `/24` = 256 total addresses
- `/16` = larger range
- `/26` = smaller subnet than `/24`

### Why CIDR matters

CIDR helps you:

- Design subnets correctly
- Avoid overlapping address spaces
- Allocate address ranges efficiently
- Understand scalability limits

---

## Important Azure subnet reminder

Azure reserves **5 IP addresses per subnet**.

For example, in a `/24` subnet:

- Total addresses: 256
- Azure-reserved addresses: 5
- Usable addresses: 251

This is an important exam concept.

---

## Practical subnet design tips

When designing subnets:

- Leave room for growth
- Avoid overlapping ranges
- Separate workloads by function
- Keep future peering/VPN expansion in mind
- Be careful with small subnet sizes

---

# 4. Azure Core Networking Building Blocks

## Azure region

A region is a physical geographic area containing Azure datacenters.

### Why regions matter

- Resource placement
- Latency
- Data residency
- Availability options
- Pricing differences

---

## Subscription

Used for:

- Billing
- Governance boundary
- Quotas and access control context

---

## Resource group

A logical container for Azure resources.

Used for:

- Organizing resources
- Managing lifecycle
- Applying permissions and policies more easily

---

## Virtual Network (VNet)

A **Virtual Network (VNet)** is your private network in Azure.

### What a VNet gives you

- Private address space
- Subnets
- Isolation
- Connectivity between Azure resources
- Integration with other networking services

### Important idea

A VNet in Azure is conceptually similar to a network you would design on-premises, but Azure manages the underlying platform.

---

## Address space and subnets

When creating a VNet, you assign an address space such as:

- `10.0.0.0/16`

Then you divide it into subnets such as:

- `10.0.0.0/24` for web
- `10.0.1.0/24` for app
- `10.0.2.0/24` for database

### Why subnets matter

- Segmentation
- Security boundaries
- Route control
- Cleaner architecture

---

## Virtual machine, NIC, and IP configuration

A VM in Azure usually works with:

- **Virtual machine resource** – compute
- **NIC (Network Interface Card)** – networking attachment
- **IP configuration** – where the IP settings live
- **Subnet** – source of private IP address
- **Optional public IP** – for internet reachability
- **NSG** – traffic filtering

### Important exam idea

The private IP is associated through the **NIC IP configuration**, not directly “inside” the VM object itself.

---

## Public IP for a VM

You can associate a public IP so a VM can be reached from the internet.

### But be careful

Direct exposure of VMs to the internet is often not best practice.

Common safer alternatives:

- Azure Bastion for administration
- Load balancer/application gateway/front door for application traffic
- Private access patterns where possible

---

## Azure Bastion

Azure Bastion lets you securely connect to VMs over RDP/SSH using the Azure portal, without exposing those VMs directly with public IPs.

### Why it is useful

- Safer admin connectivity
- No need to expose management ports directly to the internet
- Better operational security posture

### Best use case

Use Bastion when administrators need secure browser-based or portal-based access to Azure VMs.

---

## Azure NAT Gateway

Azure NAT Gateway provides **outbound internet connectivity** for resources in a subnet.

### Core purpose

A private VM may need to reach:

- software repositories
- external APIs
- update services
- internet endpoints

NAT Gateway enables this **without assigning public IPs directly to each VM**.

### Key points

- Designed for **outbound** connectivity
- Associated to a **subnet**
- Helps provide predictable outbound connectivity
- Good for scale and centralized outbound design

### Exam trap

Do not confuse **NAT Gateway** with:
- Bastion
- Load Balancer
- Public IP assignment
- Private Endpoint

NAT Gateway is mainly about **egress/outbound internet access**.

---

# 5. Azure IP Addressing Concepts

## Private IP addresses in Azure

Private IPs are used for communication inside VNets and connected networks.

### Important characteristics

- Not internet-routable
- Assigned to NICs
- Used for internal communication between resources

---

## IP assignment methods

### Dynamic private IP

- Default behavior in many cases
- Assigned from subnet pool
- May change depending on lifecycle events and configuration

### Static private IP

- Reserved within the subnet
- Better when the address should stay constant
- Good for servers, appliances, and DNS targets

### Practical rule

If something else depends on that IP, static is often safer.

---

## Public IP addresses in Azure

Public IPs enable communication over the internet.

They may be attached to:

- NICs
- Load balancers
- Other Azure networking services

### Use public IPs carefully

Only assign them when there is a real need.

---

## Public IP prefix

A public IP prefix provides a block of contiguous public IP addresses.

### Useful for

- Large environments
- Easier firewall allow-listing
- Predictable address allocation
- Scaling public-facing services

---

## Bring Your Own IP (BYOIP)

BYOIP allows organizations to bring their own public IP ranges into Azure.

### Why organizations use it

- Retain familiar address space
- Keep compliance or ownership requirements
- Support migration or continuity strategies

---

# 6. DNS Fundamentals and Azure DNS

## What DNS does

**DNS (Domain Name System)** translates human-friendly names into IP addresses.

Example:

- Name: `www.example.com`
- Result: an IP address

Without DNS, users would need to remember raw IP addresses.

---

## DNS resolution flow

A simplified resolution process:

1. User enters a domain name
2. Query goes to a recursive resolver
3. Resolver may contact:
   - Root DNS server
   - TLD server
   - Authoritative DNS server
4. Final answer returns the IP
5. Result may be cached

### Caching matters

Caching improves speed and reduces DNS server load.

---

## DNS server types

### Recursive resolver

The “worker” that performs lookups on behalf of the client.

### Root DNS servers

Top of the DNS hierarchy.

### TLD servers

Know where domains under `.com`, `.org`, etc. are managed.

### Authoritative DNS servers

Store the final official records for the domain.

---

## Common DNS record types

### A record

Maps a name to an IPv4 address.

### AAAA record

Maps a name to an IPv6 address.

### CNAME record

Maps one name to another name.

### Also important in practice

Even if not emphasized heavily in slides, know these too:

- **MX** – mail routing
- **TXT** – verification/policies/metadata
- **PTR** – reverse lookup

---

## Azure DNS name for a VM

A public-facing VM can have a DNS label associated with its public IP, producing an Azure-managed FQDN.

### Why useful

- Easier access than remembering a public IP
- Basic testing/demo scenarios

But for production, teams often use custom domains.

---

## Azure DNS in private networks

### Custom DNS and domain controller scenario

In many enterprise setups:

- A Windows Server is deployed
- Active Directory Domain Services (AD DS) and DNS are configured
- Other VMs use that DNS server for name resolution in the VNet

This is common when extending traditional enterprise identity into Azure.

---

## Azure Private DNS

Azure Private DNS provides private name resolution inside Azure private networks.

### Why use it

- Resolve private names without public exposure
- Better for internal services
- Useful with Private Endpoints and internal application architecture

### Practical value

Great when you want internal-only resolution such as:

- `db.internal.contoso.local`
- Private endpoint names for PaaS services

---

## Azure DNS Private Resolver

Azure DNS Private Resolver helps with DNS resolution between Azure and other networks without always requiring your own custom DNS VM infrastructure.

### When it helps

- Hybrid DNS resolution
- On-premises to Azure private name resolution
- Azure to on-premises name resolution
- Centralized DNS architecture in larger environments

### Why it matters

DNS is often the hidden dependency in hybrid networking. Connectivity may exist, but if name resolution fails, applications still break.

---

# 7. Connectivity Services

## Virtual Network Peering

VNet peering connects two VNets so resources can communicate privately over the Azure backbone.

### Benefits

- Low latency
- High bandwidth
- Private communication
- No public internet required

### Common use cases

- Connect app VNet to shared services VNet
- Connect environments across teams
- Hub-and-spoke designs

### Remember

Peering is **not** a VPN tunnel. It is Azure backbone connectivity between VNets.

---

## Point-to-Site VPN (P2S)

Point-to-Site VPN connects an individual client device to Azure.

### Typical users

- Remote administrators
- Developers
- Engineers working from laptops

### Authentication examples from the course materials

- Microsoft Entra ID authentication
- RADIUS-based authentication

### Best use case

Use P2S when **individual users/devices** need secure access into Azure.

---

## Site-to-Site VPN (S2S)

Site-to-Site VPN connects an entire on-premises network to Azure.

### Best use case

Use S2S when a branch office, datacenter, or corporate site needs continuous encrypted connectivity to Azure.

### Key difference from P2S

- **P2S** = user/device to Azure
- **S2S** = network/site to Azure

---

## Site-to-Site VPN with peering

A common enterprise pattern is:

- On-premises connects to one Azure VNet
- That VNet is peered with other VNets

This can allow broader connectivity across Azure environments if routing and gateway settings are designed correctly.

### Exam caution

Understand gateway transit and connectivity design patterns conceptually. Questions often test whether traffic can actually flow, not just whether a service exists.

---

## ExpressRoute

ExpressRoute provides private connectivity between on-premises infrastructure and Microsoft cloud services.

### Compared with VPN

- Does not rely on the public internet in the same way VPN does
- Designed for more consistent private enterprise connectivity
- Often chosen for larger, more demanding, or regulated environments

### When organizations choose ExpressRoute

- Higher reliability requirements
- Predictable performance
- Compliance needs
- Large-scale hybrid connectivity

---

## Azure Virtual WAN

Azure Virtual WAN is a Microsoft-managed networking service for large-scale branch, user, and network connectivity.

### Think of it as

A managed global transit network model.

### Useful for

- Many branches
- Many remote users
- Large distributed connectivity
- Simplifying complex networking at scale

### Good exam mindset

If the environment is large, multi-region, and operational complexity matters, Virtual WAN is often a strong answer candidate.

---

# 8. Application Delivery Services

## Why this section matters

Many AZ-700 questions test whether you understand **how application traffic reaches workloads**.

This often means knowing the difference between:

- Layer 4 vs Layer 7
- regional vs global
- public vs private
- DNS-based vs proxy-based routing

---

## OSI model (high-value exam view)

You do not need to memorize every detail at deep level, but you **must** know the idea:

- **Layer 3** – IP routing
- **Layer 4** – TCP/UDP and ports
- **Layer 7** – HTTP/HTTPS and application-aware decisions

### Why this matters

Some Azure services operate mainly at Layer 4, while others inspect HTTP/HTTPS at Layer 7.

---

## Azure Load Balancer

Azure Load Balancer distributes traffic across backend resources.

### Main characteristics

- Layer 4 service
- Works with TCP/UDP
- Very good for high-performance, simple traffic distribution
- Can be public or internal

### Common variants

#### Public Load Balancer

Used for internet-facing traffic.

#### Internal Load Balancer

Used only inside private networks.

### Inbound NAT rules

Inbound NAT rules allow traffic on a specific frontend port to reach a specific backend VM/port.

### When to choose Load Balancer

Use it when you need:

- Simple Layer 4 distribution
- Internal app load balancing
- Public TCP/UDP balancing
- High availability across backend instances

---

## Virtual Machine Scale Sets (VMSS)

VM Scale Sets let you run and manage a group of identical VMs.

### Why useful

- Horizontal scaling
- Consistent deployment
- Works well behind a load balancer

### Typical pattern

- VMSS provides the compute fleet
- Load balancer or Application Gateway distributes traffic

---

## Azure Application Gateway

Application Gateway is a **Layer 7** web traffic load balancer.

### Key capabilities

- HTTP/HTTPS awareness
- URL/path-based routing
- Host-based routing
- Multi-site hosting
- SSL/TLS-related web delivery capabilities
- Can integrate with WAF

### Example use cases

- Route `/images` to one backend
- Route `/videos` to another backend
- Send `app1.contoso.com` to one site
- Send `app2.contoso.com` to another site

### When to choose it

Choose Application Gateway when you need **regional Layer 7 routing** for web applications.

---

## Azure Web Apps

Azure Web Apps host web applications without managing VMs directly.

### Why this matters in networking

Even though Web Apps are platform-managed, networking still matters for:

- inbound access patterns
- custom domains
- secure access
- private connectivity
- VNet integration
- app delivery through Front Door or Application Gateway

---

## Azure Traffic Manager

Traffic Manager is a **DNS-based** traffic distribution service.

### Important idea

Traffic Manager does **not** proxy application traffic itself.  
It answers DNS queries and directs clients toward an endpoint.

### Routing methods often associated with it

- Priority/failover
- Performance-based
- Weighted
- Geographic
- Nested profiles

### Best use cases

- Global endpoint selection
- DNS-level failover
- Multi-region endpoint steering

### Exam trap

Traffic Manager is **DNS-based**, not a reverse proxy.

---

## Azure Front Door

Azure Front Door is a **global Layer 7 application delivery** service operating at Microsoft’s edge.

### Key ideas

- Global HTTP/HTTPS routing
- Performance acceleration
- Edge-based traffic handling
- Path-based and host-based routing
- Security integration with WAF

### When to choose Front Door

Use Front Door when you need:

- Global web application delivery
- Fast user experience from many regions
- Edge entry point
- Global failover for web apps/APIs

---

## Load Balancer vs Application Gateway vs Traffic Manager vs Front Door

### Very important summary

| Service | Traffic level | Scope | Best for |
|---|---|---|---|
| Azure Load Balancer | Layer 4 | Regional | TCP/UDP balancing |
| Application Gateway | Layer 7 | Regional | Web traffic routing inside a region |
| Traffic Manager | DNS-based | Global | DNS routing across endpoints/regions |
| Front Door | Layer 7 | Global | Global web app entry, acceleration, failover |

---

# 9. Private Access to Azure Services

## Service Endpoints

Service Endpoints extend a VNet identity to certain Azure PaaS services over the Azure backbone.

### Why use them

They help secure access from a subnet to supported Azure services such as storage.

### Key idea

The service still has a public endpoint, but access can be restricted to selected VNets/subnets.

### Good for

- Simpler private-ish access control
- Securing PaaS access from specific subnets
- Cases where full Private Link is not required

---

## Private Endpoints

Private Endpoints provide a private IP address in your VNet for an Azure PaaS service using Private Link.

### Why this is powerful

The service is reached privately through your VNet.

### Benefits

- Private IP-based access
- Stronger private access model than service endpoints
- Better for zero-trust/private-only architectures
- Common with storage, SQL, and many Azure PaaS services

### Important DNS note

Private Endpoints often require correct DNS configuration so that the service name resolves to the private IP.

This is a frequent exam and real-world issue.

---

## Private Link Service

Private Link Service is used to privately expose your own service to consumers over Azure Private Link.

### Easy memory trick

- **Private Endpoint** = private consumer connection to a service
- **Private Link Service** = private provider-side service exposure

---

## App Service VNet Integration

VNet Integration allows an Azure App Service to access resources inside a VNet.

### Important point

This is mainly about **outbound connectivity from the app into the network**.

### Example

A web app needs to connect privately to:

- database
- cache
- internal API
- private service endpoint

---

## Service Endpoints vs Private Endpoints

| Feature | Service Endpoints | Private Endpoints |
|---|---|---|
| Connection type | Service still uses public endpoint | Service gets private IP in your VNet |
| Security posture | Stronger than open public access | Stronger private-only model |
| DNS dependency | Lower than Private Endpoint cases | Very important |
| Typical use | Simpler subnet-based restriction | Full private access architecture |

---

# 10. Azure Network Security Services

## Network Security Groups (NSGs)

NSGs filter traffic using rules.

They can be associated with:

- Subnets
- NICs

### Rules include

- Direction: inbound/outbound
- Priority
- Protocol
- Source
- Source port
- Destination
- Destination port
- Action: allow/deny

### Very important behavior

- Lower priority number = evaluated first
- Rules are processed based on priority
- Default rules also exist

### Use NSGs for

Basic network traffic filtering between resources.

---

## Application Security Groups (ASGs)

ASGs let you group VMs logically and use those groups in NSG rules.

### Why useful

Instead of hardcoding IP addresses in security rules, you can reference logical application groups such as:

- web-servers
- app-servers
- db-servers

### Benefits

- Cleaner rules
- Easier maintenance
- Better scalability when IPs change

---

## Web Application Firewall (WAF)

WAF protects web applications from common web-layer attacks.

### Think Layer 7

WAF is focused on HTTP/HTTPS application threats.

### Common placement

- Application Gateway WAF
- Front Door WAF

### Best for

Protecting web apps against malicious HTTP(S) traffic patterns.

---

## Azure Firewall

Azure Firewall is a managed, stateful firewall service for Azure networks.

### Core capabilities

- Centralized network security control
- Network rule filtering
- Application/FQDN filtering
- SNAT and DNAT support
- High availability as a managed service

### Typical use case

In hub-and-spoke environments, Azure Firewall often sits in the hub to inspect and control traffic for spoke networks.

### Use Azure Firewall when you need

- Centralized traffic inspection
- Network and application rule control
- Controlled outbound internet access
- DNAT for published services
- More advanced network security design than NSGs alone

---

## NSG vs WAF vs Azure Firewall

| Service | Main purpose | Traffic focus |
|---|---|---|
| NSG | Basic allow/deny filtering | Network level |
| WAF | Protect web apps from web attacks | HTTP/HTTPS |
| Azure Firewall | Centralized managed firewall | Network + application filtering |

### Easy memory rule

- **NSG** = basic traffic filtering
- **WAF** = web protection
- **Azure Firewall** = centralized managed firewall platform

---

# 11. Monitoring and Troubleshooting

## Azure Network Watcher

Network Watcher provides tools to understand and troubleshoot Azure network behavior.

This is very important both for the exam and for real operations.

---

## IP Flow Verify

Checks whether traffic is allowed or denied by NSG rules.

### Best use

Use when you want to know:

- “Why is this connection blocked?”
- “Which NSG rule is allowing or denying traffic?”

---

## Next Hop

Shows the next hop for traffic going to a destination.

### Best use

Use when you want to verify routing decisions.

Example question:

- Is traffic going to the internet?
- Is it going to a virtual appliance?
- Is it staying local?
- Is the route table working as expected?

---

## Connection Troubleshoot

Tests whether a source can reach a destination over TCP/ICMP and helps identify why not.

### Why useful

It checks multiple possible failure areas:

- NSG issue
- routing issue
- port reachability issue
- destination issue

### Best use

Use it for end-to-end connectivity testing.

---

## Connection Monitor

Continuously monitors connectivity over time.

### Useful for

- latency tracking
- packet loss tracking
- outage detection
- path change visibility
- proactive alerting

### Difference from Connection Troubleshoot

- **Connection Troubleshoot** = point-in-time diagnostic
- **Connection Monitor** = ongoing monitoring

---

## Virtual Network Flow Logs

Flow logs provide metadata about allowed/denied traffic as evaluated by NSGs.

### Useful for

- historical traffic analysis
- security investigation
- compliance review
- understanding communication patterns

---

# 12. Important Comparisons for the Exam

## Bastion vs NAT Gateway

| Service | Main purpose |
|---|---|
| Bastion | Secure admin access to VMs |
| NAT Gateway | Outbound internet access for private resources |

---

## P2S vs S2S VPN

| Service | Used for |
|---|---|
| Point-to-Site VPN | Individual client device to Azure |
| Site-to-Site VPN | Entire on-premises network to Azure |

---

## VPN vs ExpressRoute

| Service | Typical use |
|---|---|
| VPN | Encrypted connectivity over public internet path |
| ExpressRoute | Private dedicated enterprise connectivity |

---

## VNet Peering vs VPN

| Service | Key difference |
|---|---|
| VNet Peering | Private Azure backbone connectivity between VNets |
| VPN | Encrypted tunnel-based connectivity |

---

## Load Balancer vs Application Gateway

| Service | Use when |
|---|---|
| Load Balancer | Need Layer 4 TCP/UDP balancing |
| Application Gateway | Need Layer 7 HTTP/HTTPS routing |

---

## Traffic Manager vs Front Door

| Service | Key difference |
|---|---|
| Traffic Manager | DNS-based endpoint selection |
| Front Door | Global Layer 7 entry point and proxy |

---

## Service Endpoints vs Private Endpoints

| Service | Use when |
|---|---|
| Service Endpoints | Restrict subnet access to supported PaaS service over Azure backbone |
| Private Endpoints | Need private IP access to the service in your VNet |

---

## NSG vs Azure Firewall

| Service | Use when |
|---|---|
| NSG | Basic subnet/NIC traffic filtering |
| Azure Firewall | Centralized, managed firewall with broader inspection/control |

---

# 13. Common Real-World Scenarios

## Scenario 1: Admin wants RDP/SSH to a VM without exposing public IP on the VM

**Best fit:** Azure Bastion

Why:

- Secure management access
- Avoids direct public exposure of management ports

---

## Scenario 2: Private VMs need internet access for updates, but should not have individual public IPs

**Best fit:** NAT Gateway

Why:

- Outbound-only internet connectivity
- Cleaner and safer egress design

---

## Scenario 3: Two Azure VNets must communicate privately

**Best fit:** VNet Peering

Why:

- Private Azure backbone
- Low latency
- Straightforward Azure-native connectivity

---

## Scenario 4: Remote employee laptop needs secure access to Azure resources

**Best fit:** Point-to-Site VPN

---

## Scenario 5: Entire branch office/datacenter needs Azure connectivity

**Best fit:** Site-to-Site VPN  
**Or** ExpressRoute if enterprise-grade private connectivity is required

---

## Scenario 6: Need to distribute TCP traffic across multiple VMs

**Best fit:** Azure Load Balancer

---

## Scenario 7: Need path-based web routing like `/images` and `/videos`

**Best fit:** Azure Application Gateway

---

## Scenario 8: Need global web acceleration and entry point close to users

**Best fit:** Azure Front Door

---

## Scenario 9: Need DNS-based failover between endpoints in different regions

**Best fit:** Azure Traffic Manager

---

## Scenario 10: Storage account should be privately reachable inside the VNet

**Best fit:** Private Endpoint

---

## Scenario 11: App Service must reach internal database in the VNet

**Best fit:** App Service VNet Integration  
Possibly combined with Private Endpoint for the target service

---

## Scenario 12: Need centralized traffic filtering for many spoke VNets

**Best fit:** Azure Firewall in a hub-and-spoke architecture

---

## Scenario 13: Need easier NSG rules for web tier to database tier communication

**Best fit:** Application Security Groups

---

## Scenario 14: Need to know why a port is blocked between two Azure resources

**Best fit:** IP Flow Verify and Connection Troubleshoot

---

# 14. Exam-Focused Revision Sheet

## High-value facts to remember

- A **VNet** is your private network in Azure.
- A **subnet** is a segment inside a VNet.
- Azure reserves **5 IP addresses per subnet**.
- **NIC IP configuration** is where Azure IP settings live.
- **Bastion** = secure VM admin access.
- **NAT Gateway** = outbound internet for private resources.
- **VNet peering** = private Azure backbone connectivity between VNets.
- **P2S VPN** = single client to Azure.
- **S2S VPN** = site/network to Azure.
- **ExpressRoute** = private enterprise connectivity.
- **Load Balancer** = Layer 4.
- **Application Gateway** = Layer 7 regional web delivery.
- **Traffic Manager** = DNS-based global routing.
- **Front Door** = global Layer 7 edge service.
- **Service Endpoint** = secure subnet-based access to supported PaaS over Azure backbone.
- **Private Endpoint** = private IP inside your VNet for a service.
- **NSG** = basic traffic filter.
- **ASG** = logical grouping for NSG rules.
- **WAF** = web application protection.
- **Azure Firewall** = centralized managed firewall.
- **IP Flow Verify** = checks NSG allow/deny result.
- **Next Hop** = shows routing decision.
- **Connection Troubleshoot** = end-to-end reachability check.
- **Connection Monitor** = continuous monitoring.
- **Flow Logs** = historical traffic metadata.

---

## Exam habits that prevent mistakes

### 1. Ask whether traffic is inbound or outbound

A lot of wrong answers happen because the service supports the opposite direction.

Example:
- Need outbound internet from private VMs → NAT Gateway
- Need admin access into VMs → Bastion

### 2. Ask whether the traffic is private or public

Example:
- Want a private IP to a PaaS service → Private Endpoint
- Want to restrict subnet access to a public service endpoint → Service Endpoint

### 3. Ask whether the service works at Layer 4, Layer 7, or DNS

Example:
- TCP/UDP only → Load Balancer
- HTTP/HTTPS routing → Application Gateway / Front Door
- DNS steering → Traffic Manager

### 4. Ask whether the problem is connectivity, security, or name resolution

Often all three look similar in a broken application.

- Connectivity issue → routing/peering/VPN/firewall
- Security issue → NSG/WAF/Azure Firewall
- Name resolution issue → DNS/Private DNS/Private Resolver

---

# 15. Quick Glossary

| Term | Meaning |
|---|---|
| VNet | Azure private virtual network |
| Subnet | Segment inside a VNet |
| NIC | Network interface attached to a resource like a VM |
| NSG | Network Security Group for allow/deny traffic rules |
| ASG | Application Security Group for grouping resources in NSG rules |
| P2S | Point-to-Site VPN |
| S2S | Site-to-Site VPN |
| CIDR | Slash notation that defines subnet size |
| NAT | Network Address Translation |
| DNS | Domain Name System |
| WAF | Web Application Firewall |
| VMSS | Virtual Machine Scale Sets |

---

# Final Revision Advice

If you only remember one method for AZ-700, use this for every question:

1. Identify the traffic type.
2. Identify whether it is private or public.
3. Identify whether the need is connectivity, routing, delivery, or security.
4. Identify whether the scope is regional, global, single user, or full network.
5. Choose the Azure service that matches the problem exactly.

That is how both the exam and real-world design questions become much easier.

---

# Source Coverage Used for These Notes

These notes were built from the uploaded course resources, including:

- networking basics, DHCP, NAT, IP addressing, DNS, and Network Watcher topics
- Azure VNet, VM/NIC/IP configuration diagrams
- Azure Bastion and NAT Gateway
- Private DNS, AD DS/DNS, and Azure DNS Private Resolver
- VNet peering, Point-to-Site VPN, Site-to-Site VPN, ExpressRoute, and Virtual WAN
- Load Balancer, Application Gateway, Traffic Manager, Front Door, service endpoints, private endpoints, App Service VNet integration, NSGs, ASGs, WAF, and Azure Firewall

