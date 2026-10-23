# buy japan vps: Tokyo return routing built for mainland China peak hours, from $29, and how to test it in 24 hours

If your users sit in mainland China, a Japan VPS gets judged on one thing: how traffic returns to China at 9pm. Core count and NVMe throughput matter, but the return path decides whether your site feels fast or falls apart during the evening rush.

GoMami's Tokyo product is called JPN Pulse, and it is built around that single question. Every plan, including the $29 entry tier, ships with China Telecom CN2, Unicom AS9929 and China Mobile CMI/CMIN2 as the return routing, plus source-IP-based route selection. The officially stated typical latency from Tokyo to mainland China is under 50ms.

GoMami's own documentation names Japan as a recommended node for **targeting northern China users**, alongside Hong Kong, and credits Unicom route optimization as the reason. That is a more specific selection rule than most provider docs bother to give, and it is worth knowing before you compare Tokyo against Hong Kong or Singapore.

Below: what the five plans cost, where the ladder stops making sense, what the DDoS layer actually covers, and how to use the 24-hour refund window to test the route from your own network.

> **Quick pick:** most first-time buyers of a China-facing Japan VPS should start on **JPN.Pulse.Nano at $29/month** (2 vCPU / 2GB / 40GB NVMe / 500GB outbound / 1Gbps). It carries the same premium routing as the $338 plan. If you expect more than roughly 15GB of outbound per day, move up to **Mini at $49** instead, since that $20 step includes more than a standalone traffic pack would. 👉 [Compare the five JPN Pulse plans](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse)

## The full JPN Pulse lineup

Japan and Singapore tie as the cheapest entry points into GoMami's China-optimized network, both starting at $29 for the Nano tier. Hong Kong's entry plan is priced higher; third-party stock trackers list the equivalent HKG.Pulse.Nano at $49. The trade-off is latency, with official typical figures of under 30ms to Hong Kong versus under 50ms to Tokyo. For plenty of workloads that gap is invisible, and $20 a month is real money.

| Plan | vCPU / RAM / NVMe | Traffic (outbound) | Port | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| JPN.Pulse.Nano | 2 / 2GB / 40GB | 500GB | 1Gbps | $29 | [Order Nano](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| JPN.Pulse.Mini | 2 / 4GB / 60GB | 1000GB | 1.5Gbps | $49 | [Order Mini](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| JPN.Pulse.Air | 4 / 8GB / 80GB | 2000GB | 1Gbps | $89 | [Order Air](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| JPN.Pulse.Pro | 8 / 16GB / 100GB | 5000GB | 3Gbps | $169 | [Order Pro](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| JPN.Pulse.Ultra | 12 / 32GB / 300GB | 10000GB | 3Gbps | $338 | [Order Ultra](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |

A few details the table doesn't show. Traffic is metered on outbound only, so inbound doesn't count against the quota, and the allowance resets at the start of each billing cycle. Pro and Ultra carry a "Windows-ready" marker on the store page; Nano, Mini and Air do not. On the CPU, GoMami's documentation doesn't name the part used in Tokyo, and third-party write-ups contradict each other (one says EPYC 7763, another says 7773X / 7K83). Treat any clock-speed claim for this specific node as unconfirmed until it appears on GoMami's own pages.

Watch the port column as well. Mini at 1.5Gbps is the fastest port below Pro, and Air steps back down to 1Gbps while costing $40 more. If port speed matters more to you than core count, Air is the odd one out here.

## Where the plan ladder actually breaks

The Nano to Mini jump is the best-value step in the whole Japan lineup. For $20 more per month you get double the RAM (2GB to 4GB), 20GB more disk, double the traffic (500GB to 1000GB), and a faster port (1Gbps to 1.5Gbps). Buying 500GB of extra traffic on its own costs $30, so the upgrade comes in cheaper than the bandwidth it bundles, with the RAM and disk thrown in.

Above Mini, the math gets less exciting unless you need cores. Air costs $40 more than Mini and adds 2 vCPU, 4GB RAM, 20GB disk and another 1TB of traffic. That is a fair deal for a multi-site setup and overkill for one small site. Pro is where you land when the workload has real concurrency: 8 vCPU, 16GB, 5TB, 3Gbps. Ultra at $338 doubles most of that again.

Two practical notes on sizing. GoMami sells traffic packs at a declining per-GB rate, starting at $6 for 100GB and running to $1,200 for 20TB, but purchased traffic is valid only for the current billing cycle and does not roll over. And if you exceed the monthly quota, the official documentation only says bandwidth "may be throttled," with specifics depending on your product spec. Third-party write-ups claim it drops to 20 kb/s until the next cycle with no overage fee; GoMami has not published that number, so don't build a budget around it.

When you're torn between two tiers, size up rather than planning to buy traffic packs later. 👉 [Check current JPN Pulse pricing](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse)

## What CN2, AS9929 and CMIN2 buy you

These are the three premium Chinese backbones behind the price. CN2 (AS4809) is China Telecom's next-generation carrier network, and the GIA tier gets priority handling during congestion. AS9929 is Unicom's premium network, separate from the ordinary 4837 backbone and generally less congested at international exits. CMIN2 and CMI are China Mobile's international premium paths.

GoMami's documentation describes the setup as "Triple-Network Direct" with Smart Routing that selects the return path based on source IP. A third-party technical write-up adds a distinction the official docs skip: Hong Kong, Japan and Singapore use premium return paths with mainline direct forward paths, while Los Angeles runs premium in both directions. For website and application hosting, the return path is what users actually feel, which is why the Asia nodes get positioned as the hosting-friendly choice.

Now the part that matters more than any of the above: verifying it. GoMami's docs recommend testing latency and routing per node before purchase. It does not publish a public test IP for Tokyo, so that recommendation can't be followed before you buy. You need a server first. The next section covers what to do once you have one.

## DDoS protection: what the 600 Gbps figure covers

Every GoMami instance includes best-effort DDoS protection by default, with 600 Gbps advanced scrubbing available as a paid add-on. The homepage advertises "up to 600 Gbps mitigation capacity," the docs describe the default layer as automatic detection and scrubbing with no manual intervention, and the 600 Gbps tier as the upgrade path for stronger protection. Third-party write-ups describe that capacity as shared infrastructure across all GoMami locations; the docs don't break it down per node, and there is no separate confirmation for Tokyo specifically.

The behavior you should actually plan around is the blackhole rule. When an attack affects other customers, the targeted IP gets blackholed for 30 to 60 minutes and released automatically, with no manual override available. A normal visitor sees nothing during that window except a dead server.

That matters most for two workloads that attract attention: game servers and e-commerce checkout flows. If either describes you, factor the paid scrubbing tier into the budget rather than assuming the default layer will absorb everything. Note also that an IP blackholed by DDoS is explicitly excluded from the refund policy, so a heavily targeted box is a one-way purchase.

## The 24-hour refund is the real testing tool

GoMami offers a full refund within 24 hours of service activation: new instances only, once per user, returned to the original payment method. Renewal orders, services terminated for Terms of Service violations, used account credit and DDoS-blackholed IPs are all excluded.

Deployment completes automatically within 1 to 5 minutes of payment, and the IP plus login credentials arrive by email. Combined with the refund window, that gives you a working test bench. Three steps:

1. **Order the $29 Nano** and wait for provisioning. The confirmation email carries the IP and credentials.
2. **Test from the networks your users are on.** Run `ping`, `traceroute` and a download test against the Tokyo IP. Confirm the path enters China via AS4809 for Telecom, AS9929 for Unicom, or CMIN2 for Mobile. Seeing AS4837 or generic NTT transit means the premium routing isn't applying, and that is worth a ticket rather than a shrug.
3. **Back up, then decide.** If the numbers don't hold up, open a Billing ticket inside the 24-hour window. Once a refund is approved, the instance and all its data are deleted immediately and permanently, so export anything you created first.

The once-per-user limit is real. You cannot cycle through buy-test-refund repeatedly to sample nodes for free, so make the first test count and run it during the hours your users are actually active.

Refund processing time depends on the payment method: Alipay 1 to 3 business days, credit card 5 to 10, cryptocurrency 1 to 3.

👉 [Start with the $29 Nano and test the route yourself](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse)

## Costs that aren't on the plan card

The sticker price is monthly. Four things change it.

**Annual billing saves 15% to 20%** versus monthly, with the exact discount varying by product line. On the Nano, that works out to roughly $278 to $296 for a year instead of $348. Those figures are arithmetic on GoMami's published range, so treat them as an estimate rather than a quoted price. Quarterly and semi-annual cycles are also available.

**There is no grace period on renewal.** The service is suspended on the due date itself, terminated at five days overdue with data possibly unrecoverable, and the unpaid invoice is auto-cancelled on day six. The renewal invoice is generated seven days before the due date and emailed, and a card on file gets charged automatically. Suspended services restore automatically once payment lands, with no ticket needed. If a live site depends on this box, set your own reminder a week before expiry.

**Add-ons are à la carte.** An IP change costs $5 and takes effect immediately, with the old IP released permanently, so update DNS first. Plan changes currently require a support ticket rather than a self-service button. OS templates cover Ubuntu 22.04 and 24.04, Debian 11 and 12, CentOS Stream 9, and AlmaLinux 8 and 9.

**Payment methods** are credit/debit card via Stripe, Stripe Alipay, and cryptocurrency. Account credit can be applied at checkout but cannot be withdrawn directly.

**Promo codes go in the cart.** The field sits on the Review & Checkout page, which comes before the payment step, and people routinely blow past it. Third-party sources have cited `GOMAMI365` as a 20% recurring annual discount across the product line. That code does not appear on GoMami's own pages, and whether it stacks with the built-in annual discount is unconfirmed, so apply it in the cart and watch the total before paying.

## Who shouldn't buy this

If your traffic doesn't originate in mainland China, you're paying for routing you won't use. General-purpose clouds charge less per unit of spec because they sell plain transit rather than a Chinese premium return path. Nothing in their catalog is engineered around Unicom's evening congestion, and for a European or North American audience that difference never surfaces.

GoMami sits in the mid-to-high tier for exactly that reason. The $29 Nano buys 2 vCPU and 2GB, and the same money spent on a commodity cloud gets you a bigger machine with worse latency to China. That's the trade, stated plainly.

Japan also isn't short of sub-$10 entry plans from Tokyo providers routing over ordinary transit. Those are fine until evening peak, when the gap becomes obvious to anyone in Shanghai or Beijing.

One honest caveat on evidence. The detailed third-party speed tests published for GoMami's network cover the Hong Kong node from December 2025, where the tester reported strong international throughput and single-thread speeds in the 500Mbps-plus range, but also severe mobile uplink QoS causing intermittent drops while downlink stayed normal. That tells you something about how the operator runs its network. It is not a measurement of the Tokyo node, and no independent evening-peak benchmark for Japan turned up in the sources reviewed here.

## Picking by situation

**Solo site, blog, or API with modest daily traffic** → Nano at $29. Same routing as the top tier, and the 24-hour window lets you confirm the path before you're committed.

**Production site with moderate traffic, or anything running a database** → Mini at $49. Doubling RAM and traffic for $20 is the best step in the lineup, and 4GB is where PHP or Node stacks stop swapping.

**Multiple sites or a mid-traffic app** → Air at $89, but only if you need cores more than port speed. Otherwise skip to Pro.

**Real concurrency, e-commerce checkout, or a game server** → Pro at $169, and budget for the 600 Gbps scrubbing add-on if the box is likely to draw attacks. Ultra at $338 if 5TB of outbound isn't enough.

Before you check out: pick the billing cycle you actually want, enter any promo code on the cart page rather than at payment, and set a renewal reminder for a week before expiry since there is no grace period.

👉 [Pick your JPN Pulse plan and check today's pricing](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse)
