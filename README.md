# buy dedicated server hosting: what actually matters before you pay, with a closer look at DMIT's bare metal

When you type "buy dedicated server hosting" into a search box, what you're really trying to figure out is rarely the brand name. It's usually one of these: "Do I actually need a whole physical box to myself, or will a beefy VPS do?", "How much is this going to cost me every month, and what am I really paying for?", and "Which provider won't quietly oversell the CPU or strand me with a network that collapses at 8pm Beijing time?"

This guide walks through the questions that decide whether a dedicated server is the right call, what to compare between providers, and where a provider like DMIT fits in — particularly if your traffic has any China or Asia-Pacific component. The pricing, plan structure, and limitations referenced below come from DMIT's public pricing and bare metal pages at the time of writing.

## Do you actually need a dedicated server, or is a VPS enough?

A dedicated server (often called bare metal) is a single physical machine reserved for you. No hypervisor slicing it up, no neighbors bursting your CPU, no shared disk IOPS. You get 100% of the cores, RAM, and storage.

That sounds great in principle, but it only justifies the price premium when your workload genuinely can't tolerate sharing. The honest list of cases where bare metal wins is shorter than marketing pages suggest:

- **CPU-bound workloads that run flat-out**: busy databases, in-memory analytics, video encoding, rendering, virtualization hosts where you're slicing the box into your own VMs.
- **Strict isolation or compliance**: regulated data, single-tenant requirements, security audits that don't accept multi-tenant clouds.
- **Predictable, sustained performance**: workloads where you can't afford a noisy neighbor spiking disk latency at random.
- **Custom hardware needs**: large GPU rigs, exotic storage arrays, specific NICs, or IP/BGP configurations that cloud platforms don't expose.

If your workload is a web app doing a few thousand requests per second with bursty traffic, a well-sized VPS on a non-oversold host is usually cheaper and easier to scale. The jump to bare metal makes sense when you've already maxed out what a VPS can give you, or when the compliance/isolation requirement forces it.

## What to compare when you buy dedicated server hosting

The specs vendors put front and center (cores, RAM, GB of storage) are only part of the picture. The things that actually bite you later are usually hidden in the fine print.

**CPU platform and generation.** An "8-core server" means nothing without the model. A current AMD EPYC 9005 (Zen 5) core outperforms an older Zen 3 core by a wide margin on single-threaded work. Ask which generation, not just how many cores.

**Storage type and RAID.** NVMe vs SATA SSD vs HDD is a 10–50x difference in IOPS. Whether RAID is hardware or software, and whether you can pick the layout, matters for both performance and redundancy.

**Network quality, not just speed.** A 10Gbps port is useless if the upstream routes are congested. This is where providers differ enormously, especially for traffic heading into China or crossing the Pacific.

**IP resources and BGP.** Need extra IPv4 blocks, large IPv6 allocations, or the ability to announce your own IPs (BYOIP)? Not every provider supports BGP sessions. This is a real differentiator for anyone building CDN nodes or multi-homed setups.

**Managed vs unmanaged.** Most bare metal is unmanaged — you get root/IPMI and you're on your own. Confirm the actual support SLA. "Unmanaged" at one provider might mean 24-hour ticket responses; at another it might be 72 hours.

**Datacenter tier and redundancy.** Tier III+ with N+1 power and 24/7 remote hands is a different animal from a single-rack closet. For production workloads, this is not the place to save $20/month.

**SLA and what happens when it's breached.** Read the actual SLA percentage and the compensation structure. A 99.9% SLA with no meaningful remedy is worse than a 99% SLA with clear service credits.

**Refund and abuse policies.** Dedicated servers are often non-refundable once deployed, or refundable only within a very short window. DDoS-targeted IPs, abuse takedowns, and "network not good enough" complaints can void refunds entirely at some providers. Read these before buying, not after.

## Where DMIT fits: a provider built around China-optimized routing

DMIT (dmit.io) is a hosting provider whose entire product line is organized around one premise: getting traffic reliably between the Asia-Pacific region and the rest of the world, with particular attention to mainland China. They operate out of Los Angeles, Hong Kong, and Tokyo, and they run their own capacity rather than reselling someone else's racks.

What sets DMIT apart from the generic dedicated-server crowd is the network layer. Instead of selling one "kind" of bandwidth, they split their network into three tiers, and every plan sits in one of them:

- **Premium Network** — built on China Telecom CN2 GIA plus DMIT's own backbone and direct peering with China Telecom (AS4809), China Unicom (AS9929), and China Mobile International (AS58807). This is the lowest-latency, lowest-packet-loss path into mainland China. It's also the most expensive per GB.
- **Eyeball Network** — Tier 1 transit combined with "reasonable-effort" China routing via CMIN2/CMI and other Chinese eyeball ISPs. A middle ground: better China access than plain Tier 1, cheaper than Premium.
- **Tier 1 Network** — clean global transit over a multi-Tbps backbone (Cogent, NTT, GTT, Arelion, Lumen, Tata, plus IX peering). No China-specific optimization. Cheapest, best for bandwidth-heavy or non-China workloads.

If your users are in mainland China and you care about peak-hour latency and packet loss, the Premium tier is the entire reason to look at DMIT. If your audience is global with some China traffic, Eyeball is the practical pick. If China isn't a factor at all, you're probably paying for routing you don't need, and a cheaper Tier 1 provider may serve you just as well.

## DMIT bare metal: what you actually get

DMIT's true dedicated server product is their **Bare Metal Instance** — a single-tenant physical machine with no virtualization overhead. The bare metal page describes the building blocks, and unlike their cloud VPS plans, bare metal configurations are custom-quoted rather than listed at fixed prices.

**Hardware**
- AMD EPYC platforms, up to 128 cores / 256 threads
- DDR4 or DDR5 ECC memory, up to multi-TB
- NVMe / SSD / HDD options, with hardware and software RAID
- GPU and accelerator options on request
- IPMI / out-of-band management included
- 10Gbps uplinks with custom port speeds available

**Hardware platforms by generation**

DMIT groups their compute into three platform tiers, which matters when you're comparing price-per-core:

| Platform | CPU | Position |
| --- | --- | --- |
| AN5 | AMD EPYC 9005 (Zen 5) + DDR5 + NVMe Gen5 | Flagship, highest single-core and multi-core performance |
| AN4 | AMD EPYC 9004 (Zen 4) | Balanced, field-tested workhorse |
| AS3 | AMD EPYC 7003 (Zen 3) | Best price-per-core, entry-level |

> **Heads up on AS3:** DMIT's own pricing page notes that the LAX AS3 series is still being built out and optimized, and during this period you may experience reduced disk performance and a lower SLA than their mature platforms. If you're buying for production, factor that in — the cheaper AS3 tier isn't yet at parity with AN4/AN5.

**Workload categories DMIT positions bare metal for**

- **Compute Optimized** — high-frequency, high-core-count builds for databases, app servers, virtualization hosts.
- **Storage Optimized** — all-NVMe or large HDD arrays, tunable for IOPS or raw capacity.
- **Enterprise & Custom** — GPU, large-memory, and cluster configurations sourced and assembled to spec.

**IP and network customization**

This is an area where DMIT is more flexible than most: additional IPv4 blocks, large IPv6 allocations, BGP sessions, BYOIP announcements, multi-subnet and private network options are all on the table. For anyone building CDN edges or multi-homed infrastructure, that's not a checkbox feature — it's a real capability.

**Datacenter and operations**
- Tier III+ facilities with concurrently maintainable design
- N+1 or better UPS and generator backup
- Redundant precision cooling
- 24/7 on-site staff, multi-factor access control, CCTV
- 24/7 remote hands for reboots, hardware swaps, emergencies

**SLA.** DMIT currently offers a 99% SLA. The compensation structure: below 99% uptime you get half a month credited; below 95% a full month; below 90% two months. Notification must follow the SLA procedure within three days of the incident or the credit is waived.

## DMIT plans and pricing

Here's where it's important to be precise, because DMIT's product line has two layers and they're easy to confuse.

**True bare metal (dedicated physical servers) is custom-quoted.** There is no public price list for bare metal configurations — you describe your CPU, RAM, storage, bandwidth, and IP requirements, and DMIT's team returns a tailored quote. This is normal for serious dedicated hardware; the cost varies too much with configuration to publish a flat menu.

**Fixed-price plans on the pricing page are cloud VPS instances**, not bare metal. They're listed under location/network/platform selectors (e.g. Los Angeles / Premium Network / AS3) and give you vCores, RAM, SSD, transfer, and port speed at a monthly price. These are the entry point if you want DMIT's network quality without committing to a full physical machine.

The table below reflects the fixed-price plans DMIT currently shows on their Los Angeles / Premium Network / AS3 pricing page. These are cloud instances with shared vCores — useful as a reference for DMIT's pricing structure and as a starting point if you're scaling up toward bare metal.

| Plan | vCores | RAM | Storage | Monthly Transfer | Port Speed | Price (USD/month) | Purchase |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TINY | 1 | 2GB | 20GB SSD | 1000GB | 1Gbps | $10.90 | [View plan](https://bit.ly/DmiT) |
| Pocket | 2 | 2GB | 40GB SSD | 1500GB | 4Gbps | $16.90 | [View plan](https://bit.ly/DmiT) |
| Starter | 2 | 2GB | 80GB SSD | 3000GB | 10Gbps | $34.90 | [View plan](https://bit.ly/DmiT) |
| Mini | 4 | 4GB | 80GB SSD | 5000GB | 10Gbps | $62.90 | [View plan](https://bit.ly/DmiT) |
| Micro | 4 | 4GB | 160GB SSD | 7000GB | 10Gbps | $87.90 | [View plan](https://bit.ly/DmiT) |
| Medium | 6 | 8GB | 160GB SSD | 15000GB | 10Gbps | $199.90 | [View plan](https://bit.ly/DmiT) |
| Bare Metal (custom) | Up to 128c/256t | Multi-TB | NVMe/SSD/HDD + RAID | Custom | 10Gbps+ | Custom quote | [Request a bare metal quote](https://bit.ly/DmiT) |

> DMIT notes on the pricing page that products and prices may not be updated in real time due to adjustment, and the figures are for reference only. For Tier 1 network products, assigned IP addresses are not guaranteed to be reachable in all countries or regions — relevant if you're serving users in places with national network filtering.

For current AN5 (Zen 5) Premium configurations in Los Angeles, DMIT's cloud instance page also lists curated plans starting around $79.90/month for a 4 vCore / 4GB / 80GB / 5000GB / 10Gbps tier and scaling up to $289.90/month for 6 vCore / 8GB / 160GB / 15000GB / 10Gbps — these are the newer-platform equivalents of the AS3 plans above, and the natural comparison point if you want current-generation hardware.

If you're ready to spec a real dedicated box, the cleanest path is to 👉 [request a bare metal quote](https://bit.ly/DmiT) directly with your workload details — DMIT's team will return a configuration and price tailored to what you actually need rather than what fits a fixed menu.

## Discounts and promo codes

DMIT releases discount codes from time to time. A few things worth knowing from their published terms:

- **Discount codes apply to new customers only.** Existing-customer codes are issued separately for things like service compensation, and DMIT explicitly states that using someone else's customer-specific code will result in service suspension and refused refund until the full order is paid.
- **Non-monthly billing cycles usually unlock better deals.** Recurring discounts and launch promos historically require quarterly or longer prepayment. Annual prepayment is also where the biggest savings show up.
- **Prices are locked for the term you sign up for**, but DMIT reserves the right to change listed prices and resource allocations at any time for new orders or renewals.

Because promo codes rotate and some are series-specific or time-limited, the reliable approach is to check what's currently active on the DMIT site at the point of purchase rather than relying on a code copied from a third-party coupon page. When a code is live, it's applied at checkout.

## The buying process: what to expect

For DMIT's cloud VPS plans, the flow is self-service: pick location, network series, and platform; choose a plan; deploy in minutes with free instant setup. You get full root access, automated backups, instant snapshots, and SSH key authentication. Supported operating systems include Ubuntu, Debian, CentOS, CentOS Stream, AlmaLinux, Rocky Linux, Fedora, openSUSE Leap, Arch Linux, and Alpine Linux.

For bare metal, the process is quote-based: you submit requirements, DMIT returns a configuration and price, and deployment happens after agreement and payment. Custom builds take longer than VPS instantiation — expect days, not minutes, especially for GPU or large-memory configurations that require sourcing.

A few account and policy details worth flagging before you commit:

- **Unmanaged by default.** DMIT's terms state that most services are unmanaged and they can only guarantee support ticket replies within 72 hours. If you need hands-on management or faster response, factor that into your decision or look for a managed add-on.
- **OFAC restrictions.** DMIT does not accept orders from Cuba, Iran, Lebanon, Libya, Myanmar, North Korea, Somalia, Sudan, or Syria.
- **Refund window is tight.** Full refunds (minus gateway fees) are available only within 3 days of purchase and only if you've used no more than 30GB of transfer. Partial refunds extend to 30 days, calculated against either remaining transfer or remaining service time, whichever is lower. Refunds are not issued if the IP has been DDoS-targeted, if the complaint is about network quality or IP geolocation, or if you've already had three refunds on the same product series. For bare metal specifically, confirm the refund terms in your quote — they may be stricter than the VPS policy.
- **No account transfers.** DMIT does not allow account transfers between users and reserves the right to terminate accounts immediately without refund in such cases.
- **IP replacement policies vary by network tier.** On Premium and Eyeball, free replacement is available every 7 days with the IP Care+ add-on, or every 15 days without it; immediate replacement costs $5. On Tier 1, replacement is $5 per occurrence with a 7-day cooldown, and without the IP Guarantee+ add-on DMIT doesn't guarantee global reachability for new IPs (notably in China, Russia, and other countries with national censorship).

## When DMIT is the right call — and when it isn't

DMIT earns its premium when one or more of these is true:

- Your users are in mainland China or the broader Asia-Pacific region and you need routing that survives peak hours without falling apart.
- You need direct peering with all three major Chinese carriers and CN2 GIA quality, but you don't want to host inside China itself.
- You want a single provider spanning Los Angeles, Hong Kong, and Tokyo with consistent network quality across all three.
- You need real bare metal with custom hardware, BGP/BYOIP, or GPU configurations — not just a large VPS.

DMIT is harder to justify when:

- Your traffic has no China or APAC component and you're purely price-shopping on raw specs. Generic Tier 1 providers will undercut them.
- You need a fully managed server with 24/7 hands-on admin support. DMIT is unmanaged with a 72-hour ticket SLA.
- You want a self-serve bare metal catalog with published prices and instant deployment. DMIT's bare metal is quote-based and takes longer to stand up.
- You're shopping for the absolute cheapest dedicated box regardless of network. That's not what DMIT is built for.

## A practical checklist before you buy

Regardless of provider, run through this before you hand over payment:

1. **Confirm the CPU model and generation**, not just core count.
2. **Get the storage type and RAID options in writing** — NVMe vs SSD vs HDD changes everything.
3. **Ask which network tier your plan sits in and what the China/APAC routing actually looks like** at peak hours, not just on a test run at 2am.
4. **Check the real SLA and the compensation structure**, not the marketing "99.9% uptime" claim.
5. **Read the refund and abuse policies** — especially the DDoS and IP-reachability exclusions.
6. **Confirm managed vs unmanaged and the actual ticket response SLA.**
7. **Verify IP resources**: how many IPv4 addresses, IPv6 allocation, and whether BGP/BYOIP is supported if you need it.
8. **Check datacenter location and tier**, including redundant power and remote-hands availability.
9. **Test the network from your actual user locations** if possible — ideally from inside mainland China during evening hours, since that's where China-optimized routing either earns its keep or doesn't.
10. **Ask for a custom bare metal quote** rather than assuming the published VPS plans represent the ceiling of what's available.

If your workload genuinely needs a whole machine and your audience sits behind the Great Firewall, DMIT's combination of CN2 GIA routing, direct carrier peering, and custom bare metal builds is a serious option — the Premium tier pricing reflects real cost, but the routing quality is equally real. For purely global, non-China workloads, the math may point elsewhere, and that's worth admitting before you buy rather than after.
