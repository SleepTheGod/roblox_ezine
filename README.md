# ═══════════════════════════════════════════════════════════
#              R O B L O X   R E C O N   Z I N E
#                       ISSUE #001
#                   U L T R A   E D I T I O N
# ═══════════════════════════════════════════════════════════

**SUBDOMAIN MAPPING · ATTACK SURFACE · ENDPOINT ATLAS**

**Made by SleepTheGod · 2026**

> "68 hosts in the dump. 70+ API domains in the wild. The dump caught a fraction."
>
> — The Recon Desk, 2026

---

# CONTENTS

```text
[01] THE HEADLINE .......... Two IPs run the show
[02] THE MAP ............... Host-to-IP inventory
[03] THE CLUSTERS .......... Shared infrastructure
[04] THE OUTLIERS .......... Records that do not fit
[05] THE GAPS .............. Missing and unenriched assets
[06] THE TAG LIE ........... N/A does not mean no CDN
[07] THE 128.116 QUESTION .. Network ownership and edge attribution
[08] THE MISSING GATEWAY ... apis.roblox.com
[09] THE ENDPOINT ATLAS .... Roblox API domain inventory
[10] TIER LIST ERRORS ...... Logical function vs physical hosting
[11] DEEP DIVES ............ vpn2, explore, upload
[12] WHAT IS NOT HERE ...... CT, MX, ASN, TLS, HTTP
[13] THE ARCHITECTURE ...... What the evidence actually supports
[14] RECON NEXT STEPS ...... Reproducible collection procedures
[15] EVIDENCE & LIMITATIONS . Confidence and attribution
[16] THE TAKEAWAY .......... Final analyst assessment
[17] APPENDIX .............. Commands, schemas, terminology
```

---

# ░▒▓ [01] THE HEADLINE ▓▒░

The supplied DNS inventory contains approximately 67 resolved host records, alongside a narrative-only reference to `apis.roblox.com`.

The dataset presents an interesting pattern: several unrelated-looking hostnames resolve to a relatively small number of shared IP addresses.

```text
  11 hosts  -->  128.116.5.3
  10 hosts  -->  128.116.31.3
  --------------------------------
  21 hosts  -->  2 IP addresses
```

That represents approximately 31% of the stated 67-host inventory.

The immediate observation is straightforward:

**Multiple Roblox subdomains share public network destinations.**

What that means internally is a separate question.

Shared addresses can represent reverse proxies, load balancers, virtual hosting, edge infrastructure, or other shared hosting arrangements. DNS alone cannot distinguish these possibilities.

Meanwhile, the original dataset identifies two Cloudflare-associated records:

```text
auth.roblox.com    104.18.2.63
pay.roblox.com     104.18.35.156
```

And one CloudFront-associated record:

```text
upload.roblox.com  3.173.161.96
```

These observations suggest multiple infrastructure providers or delivery layers within the visible hostname inventory.

They do not establish that authentication and payment are the only protected services, nor do they establish why those particular hostnames use Cloudflare.

### The central finding

The inventory is not a one-hostname-to-one-server map.

It is a collection of DNS observations showing shared addresses, cloud-associated addresses, and several records requiring further attribution.

> **The IP is the destination observed by DNS. It is not necessarily the application server, backend, or origin.**

---

# ░▒▓ [02] THE MAP ▓▒░

The following table preserves the hostname and address information supplied in the original inventory.

Provider classifications are retained as reported observations where available. `N/A` means the source did not supply an enrichment classification.

## Host-to-IP inventory

| Hostname | IPv4 | Reported tag | Original tier |
|---|---|---|---|
| about.roblox.com | 23.58.193.11 | N/A | Corporate |
| ads.roblox.com | 128.116.5.3 | N/A | Unmapped |
| affiliates.roblox.com | 128.116.5.3 | N/A | Unmapped |
| apis.roblox.com | — | — | Developer |
| auth.roblox.com | 104.18.2.63 | Cloudflare | Identity |
| billing.roblox.com | 128.116.5.3 | N/A | Financial |
| blog.roblox.com | 23.58.193.11 | N/A | Corporate |
| careers.roblox.com | 2.16.193.41 | N/A | Corporate |
| catalog.roblox.com | 128.116.31.3 | N/A | Economy/UI |
| chat.roblox.com | 128.116.5.3 | N/A | Frontend |
| community.roblox.com | 13.58.212.38 | N/A | Unmapped |
| contacts.roblox.com | 128.116.31.3 | N/A | Unmapped |
| content.roblox.com | 128.116.31.3 | N/A | CDN |
| corp.roblox.com | 2.21.216.67 | N/A | Corporate |
| corporate.roblox.com | 2.16.193.41 | N/A | Corporate |
| create.roblox.com | 18.239.105.65 | N/A | Developer |
| css.roblox.com | 52.217.131.120 | N/A | CDN |
| data.roblox.com | 128.116.5.3 | N/A | Unmapped |
| de.roblox.com | 3.141.118.184 | N/A | Locale |
| design.roblox.com | 18.239.18.7 | N/A | Unmapped |
| develop.roblox.com | 128.116.5.3 | N/A | Developer |
| developer.roblox.com | 18.164.52.109 | N/A | Developer |
| es.roblox.com | 18.222.10.86 | N/A | Locale |
| events.roblox.com | 18.239.105.8 | N/A | Frontend |
| explore.roblox.com | 199.60.103.2 | N/A | Unmapped |
| forum.roblox.com | 128.116.5.3 | N/A | Unmapped |
| fr.roblox.com | 3.141.118.184 | N/A | Locale |
| friends.roblox.com | 128.116.31.3 | N/A | Frontend |
| games.roblox.com | 128.116.31.3 | N/A | Frontend |
| gold.roblox.com | 128.116.44.3 | N/A | Unmapped |
| groups.roblox.com | 128.116.5.3 | N/A | Economy/Developer |
| help.roblox.com | 18.222.10.86 | N/A | Corporate |
| images.roblox.com | 16.182.104.96 | N/A | CDN |
| jira.roblox.com | 3.141.118.184 | N/A | Internal |
| job.roblox.com | 2.16.193.41 | N/A | Unmapped |
| jobs.roblox.com | 23.58.193.11 | N/A | Corporate |
| js.roblox.com | 52.217.197.8 | N/A | CDN |
| learn.roblox.com | 128.116.44.3 | N/A | Unmapped |
| lms.roblox.com | 128.116.31.3 | N/A | Internal |
| m.roblox.com | 128.116.5.3 | N/A | Frontend |
| mail.roblox.com | 142.251.168.121 | N/A | Google |
| news.roblox.com | 18.222.10.86 | N/A | Corporate |
| nl.roblox.com | 128.116.5.3 | N/A | Locale |
| partners.roblox.com | 8.42.96.86 | N/A | Unmapped |
| pay.roblox.com | 104.18.35.156 | Cloudflare | Financial |
| pt.roblox.com | 3.141.118.184 | N/A | Locale |
| research.roblox.com | 23.58.193.11 | N/A | Unmapped |
| sales.roblox.com | 13.58.212.38 | N/A | Financial |
| search.roblox.com | 128.116.31.3 | N/A | Unmapped |
| setup.roblox.com | 16.15.214.37 | N/A | Unmapped |
| share.roblox.com | 128.116.44.3 | N/A | Unmapped |
| shop.roblox.com | 3.173.161.43 | N/A | Frontend |
| silver.roblox.com | 128.116.114.3 | N/A | Unmapped |
| static.roblox.com | 128.116.5.3 | N/A | CDN |
| status.roblox.com | 52.222.201.53 | N/A | Corporate |
| survey.roblox.com | 2.16.245.178 | N/A | Unmapped |
| uk.roblox.com | 18.222.10.86 | N/A | Locale |
| upload.roblox.com | 3.173.161.96 | CloudFront | Developer |
| users.roblox.com | 128.116.31.3 | N/A | Identity |
| vpn2.roblox.com | 216.200.230.122 | N/A | VPN |
| web.roblox.com | 128.116.31.3 | N/A | Frontend |
| wiki.roblox.com | 18.239.105.93 | N/A | Unmapped |
| www.roblox.com | 128.116.31.3 | N/A | Frontend |

### Original enrichment record

The source provides one detailed enrichment result:

```json
{
  "hostname": "upload.roblox.com",
  "cpe": "cpe:/a:amazon:amazon_cloudfront",
  "edge": "server-3-173-161-96.ams56.r.cloudfront.net",
  "ip": "3.173.161.96",
  "ports": [80, 443],
  "tags": ["cloud", "cdn"],
  "vulnerabilities": []
}
```

This supports a CloudFront-associated edge attribution in the supplied scanner output.

It does not identify the origin server or establish the security posture of the upload application.

---

# ░▒▓ [03] THE CLUSTERS ▓▒░

The most immediately visible pattern is the concentration of hostnames on shared IP addresses.

## Major shared-address groups

```text
┌──────────────────────────────────────────────────────────────┐
│ IP                  HOST COUNT   HOSTS                       │
├──────────────────────────────────────────────────────────────┤
│ 128.116.5.3              11      ads, affiliates, billing,   │
│                                  chat, data, develop, forum,  │
│                                  groups, m, nl, static        │
│                                                              │
│ 128.116.31.3             10      catalog, contacts, content,  │
│                                  friends, games, lms, search, │
│                                  users, web, www              │
│                                                              │
│ 3.141.118.184             4      de, fr, jira, pt             │
│ 23.58.193.11              4      about, blog, jobs, research  │
│ 18.222.10.86              4      es, help, news, uk           │
│ 2.16.193.41               3      careers, corporate, job     │
│ 18.239.105.x              3      create, events, wiki         │
│ 13.58.212.38              2      community, sales             │
│ 104.18.x                  2      auth, pay                    │
│ 52.217.x                  2      css, js                      │
├──────────────────────────────────────────────────────────────┤
│ OTHER REPORTED SINGLE-HOST ADDRESSES                          │
│ explore, gold, images, learn, mail, partners, research,       │
│ setup, share, shop, silver, status, survey, upload, vpn2      │
└──────────────────────────────────────────────────────────────┘
```

The cluster labels above describe the supplied records, not verified ownership or application topology.

### Reading the shape

**128.116.x.x**

Multiple hostnames resolve to addresses in this range. The original inventory associates several core platform-facing hostnames with these destinations.

A shared destination could represent an edge or routing layer, but DNS evidence alone does not prove anycast, reverse-proxy implementation, or backend consolidation.

**18.x.x.x, 23.x.x.x, 2.x.x.x, 3.x.x.x, 13.x.x.x and 52.x.x.x**

These addresses appear in the original table as separate hosting or delivery destinations. Provider ownership must be checked against current registration and routing records.

**104.18.x.x**

The two supplied records are identified by the original enrichment as Cloudflare-associated.

**3.173.161.96**

The supplied enrichment explicitly identifies a CloudFront-associated record.

**199.60.103.2 and 216.200.230.122**

These two destinations deserve separate attribution because the original inventory does not provide a verified provider classification.

### Important distinction

A shared IP is not proof of a shared application.

A separate IP is not proof of a separate backend.

A provider-associated IP is not proof of the complete hosting architecture.

---

# ░▒▓ [04] THE OUTLIERS ▓▒░

Some entries deserve more attention because their naming, classification, or address differs from the dominant patterns.

## `vpn2.roblox.com` — 216.200.230.122

The hostname contains a VPN-related label.

Possible explanations include:

- Corporate remote-access infrastructure.
- A third-party hosted service.
- A historical or stale DNS record.
- A service name retained after infrastructure migration.

None is confirmed by the supplied DNS result.

The record establishes only that the original dataset associated this hostname with the stated IP.

A VPN hostname is not automatically a vulnerability.

## `jira.roblox.com` — 3.141.118.184

The hostname suggests an issue-tracking application.

It shares the reported address with three locale hostnames:

```text
fr.roblox.com
de.roblox.com
pt.roblox.com
jira.roblox.com
```

This does not prove that Jira is exposed or that an internal application is accessible.

The next question is whether the hostname presents a public application response, redirects, requires authentication, or is simply a DNS record without a usable service.

## `mail.roblox.com` — 142.251.168.121

The original dataset associates this address with Google infrastructure.

That does not establish the organization's mail-routing configuration.

A hostname's A record is not equivalent to an MX record.

Mail infrastructure should be investigated through:

- MX records.
- SPF records.
- DKIM selectors.
- DMARC policy.
- Certificate information where relevant.

## `lms.roblox.com`

The original table labels this hostname as internal.

Its reported address is shared with several other platform-facing hostnames.

The name may indicate a learning management service, but that interpretation is not established by DNS alone.

## `setup.roblox.com`

The supplied record identifies:

```text
16.15.214.37
```

The service function is not confirmed.

## `survey.roblox.com`

The supplied record identifies:

```text
2.16.245.178
```

The service function and current ownership require additional evidence.

### Outlier assessment

| Hostname | Why it stands out | What remains unknown |
|---|---|---|
| vpn2 | VPN-related naming | Actual service and operator |
| jira | Internal-tool naming | Public application behavior |
| explore | Separate address | Provider and application |
| setup | Unmapped destination | Actual function |
| survey | Unmapped destination | Hosting and ownership |
| partners | Separate destination | Provider and purpose |
| silver | Separate 128.116 address | Application role |
| gold | Separate 128.116 address | Application role |

These are investigation leads, not vulnerability findings.

---

# ░▒▓ [05] THE GAPS ▓▒░

The original inventory contains several different types of missing information.

They should not be treated as interchangeable.

## Missing from the host-to-IP table

`apis.roblox.com` appears in the narrative but has no address in the table.

This is a documentation discrepancy.

It does not prove that the hostname failed to resolve.

Possible explanations include:

1. It was omitted from the exported table.
2. The scanner did not collect an address.
3. The DNS response differed by resolver or time.
4. The record used a CNAME chain not preserved in the export.
5. The hostname was included in the narrative from a different source.

The original data cannot distinguish these explanations.

## Missing provider attribution

Most rows have an `N/A` tag.

That is a limitation of the enrichment dataset.

It does not establish the absence of:

- A CDN.
- A WAF.
- Reverse-proxy infrastructure.
- Cloud hosting.
- DDoS mitigation.
- Application-layer security controls.

## Missing service evidence

The table provides hostnames and addresses, but it does not provide complete application-level observations.

Missing information includes:

- HTTP status codes.
- Redirect destinations.
- TLS certificate chains.
- Certificate SANs.
- TLS protocol negotiation.
- Response headers.
- Application fingerprints.
- DNS record history.
- IPv6 results.
- Origin information.
- ASN attribution.
- RDAP ownership.
- Geographic variation.

## Missing API coverage

The original endpoint atlas references substantially more API functionality than appears in the DNS inventory.

The important distinction is:

**A domain missing from this dataset is not necessarily missing from Roblox's infrastructure.**

It may simply have been outside the scan's collection scope.

---

# ░▒▓ [06] THE TAG LIE ▓▒░

The original enrichment column contains a misleadingly simple classification:

```text
Cloudflare
CloudFront
N/A
```

The problem is not necessarily the scanner.

The problem is how the results are interpreted.

Consider:

```text
104.18.35.156  Cloudflare
104.18.2.63    Cloudflare
3.173.161.96   CloudFront
```

The CloudFront association is present in the JSON enrichment record, even though the original table labels the corresponding hostname `N/A`.

That is an inconsistency between the table and the enrichment record.

### What N/A actually means

```text
N/A = no enrichment classification supplied
```

It does not mean:

```text
N/A != no CDN
N/A != no WAF
N/A != no cloud provider
N/A != direct origin
N/A != unprotected
N/A != vulnerable
```

### Provider attribution requires evidence

The original draft groups multiple addresses into cloud-provider categories.

Those classifications should be checked individually.

The following is a list of addresses requiring attribution, not a verified provider inventory:

```text
3.141.118.184
3.173.161.43
3.173.161.96
18.239.105.8
18.239.105.65
18.239.105.93
18.222.10.86
18.164.52.109
18.239.18.7
52.217.131.120
52.217.197.8
52.222.201.53
16.15.214.37
16.182.104.96
13.58.212.38
2.16.193.41
2.16.245.178
2.21.216.67
23.58.193.11
8.42.96.86
199.60.103.2
216.200.230.122
142.251.168.121
```

### The actual finding

The enrichment output is incomplete.

The original dataset contains one explicit CloudFront enrichment record, two Cloudflare-tagged records, and many unclassified records.

A reliable report must distinguish scanner output from independently verified provider ownership.

---

# ░▒▓ [07] THE 128.116 QUESTION ▓▒░

Four addresses in the supplied inventory deserve separate examination:

```text
128.116.5.3
128.116.31.3
128.116.44.3
128.116.114.3
```

The original table associates these destinations with multiple Roblox hostnames.

The original narrative attributes the range to Roblox Corporation and interprets the destinations as a Roblox-operated edge layer.

That attribution requires current registry and routing evidence.

### What must be established

1. Who is the registered holder of the relevant address allocation?
2. What prefix is currently announced?
3. Which ASN originates the route?
4. Does reverse DNS provide useful information?
5. Are the addresses advertised from multiple locations?
6. Do certificates or HTTP responses indicate common infrastructure?
7. Are the observed addresses stable over time?

### Why this matters

If the addresses belong to Roblox and are used as a shared edge layer, that would be useful architectural information.

If they belong to another provider, the interpretation changes.

If the addresses are shared or routed through a larger service, the hostname-to-IP relationship still does not reveal the backend.

### What cannot be concluded yet

The original dataset does not establish:

- The presence or absence of DDoS mitigation.
- Whether a WAF is deployed.
- Whether the services use a reverse proxy.
- Whether the addresses are anycast.
- Whether the backend services are physically separated.
- Why authentication and payment use different reported destinations.

**The 128.116 range is an attribution question, not a confirmed security boundary.**

---

# ░▒▓ [08] THE MISSING GATEWAY ▓▒░

## `apis.roblox.com`

The Open Cloud API hostname is one of the most important omissions in the original host-to-IP table.

The original narrative identifies it as a primary API gateway.

The table does not provide a corresponding IP address.

This discrepancy deserves its own investigation.

### DNS questions

The following queries separately examine the relevant DNS record types:

```bash
dig apis.roblox.com A +noall +answer
dig apis.roblox.com AAAA +noall +answer
dig apis.roblox.com CNAME +noall +answer
```

The purpose is to preserve the actual DNS response rather than assuming an address or provider.

### HTTP metadata

A basic response check can establish whether an HTTPS endpoint responds:

```bash
curl -sS -D - -o /dev/null \
  https://apis.roblox.com/
```

A response does not establish that authenticated API functionality is accessible.

A denial or error response does not establish that the hostname is nonexistent.

### Open Cloud

Roblox Open Cloud provides documented programmatic interfaces for developer operations.

The original endpoint atlas references the following general functionality:

- Data stores.
- Memory stores.
- Ordered data stores.
- Universe management.
- Place management.
- Asset operations.
- User information.
- Group information.
- Group membership operations.
- User restrictions.

The exact endpoint paths, supported HTTP methods, authorization requirements, and versioning should be taken from the current official API reference.

### API key introspection

The original inventory references:

```text
POST apis.roblox.com/api-keys/v1/introspect
```

This should be treated as an API reference requiring documentation verification, not as an invitation to test arbitrary credentials.

### Important distinction

An API gateway is not the same thing as an API inventory.

A gateway can expose multiple endpoint families while the public DNS inventory contains only one hostname.

Likewise, a list of documented API domains is not proof that all of them were observed during this particular scan.

---

# ░▒▓ [09] THE ENDPOINT ATLAS ▓▒░

The original DNS table contains a relatively small selection of Roblox hostnames.

The endpoint atlas below separates hostnames present in the supplied inventory from additional API-related names cited in the original research notes.

**Present** means listed in the supplied inventory.

**Not present** means absent from that inventory, not absent from Roblox infrastructure.

## 9.1 Core identity and authentication

| Domain | Described function | In supplied dump |
|---|---|---|
| auth.roblox.com | Authentication sessions | Present |
| users.roblox.com | User information and profiles | Present |
| accountinformation.roblox.com | Account information | Not present |
| accountsettings.roblox.com | Account settings | Not present |
| twostepverification.roblox.com | Two-step verification | Not present |
| usermoderation.roblox.com | User moderation | Not present |
| punishments.roblox.com | Punishment-related information | Not present |

## 9.2 Economy and financial

| Domain | Described function | In supplied dump |
|---|---|---|
| pay.roblox.com | Payment-related functionality | Present |
| billing.roblox.com | Billing-related functionality | Present |
| catalog.roblox.com | Catalog | Present |
| groups.roblox.com | Group functionality | Present |
| sales.roblox.com | Sales-related functionality | Present |
| economy.roblox.com | Economy | Not present |
| economycreatorstats.roblox.com | Creator economy statistics | Not present |
| engagementpayouts.roblox.com | Engagement payout functionality | Not present |
| premiumfeatures.roblox.com | Premium features | Not present |
| trades.roblox.com | Trading | Not present |
| inventory.roblox.com | Inventory | Not present |
| itemconfiguration.roblox.com | Item configuration | Not present |

## 9.3 Developer and Open Cloud

| Domain | Described function | In supplied dump |
|---|---|---|
| develop.roblox.com | Development configuration | Present |
| developer.roblox.com | Developer resources | Present |
| create.roblox.com | Creator Hub | Present |
| upload.roblox.com | Upload-related service | Present |
| apis.roblox.com | Open Cloud gateway | Not in IP table |
| publish.roblox.com | Publishing-related operations | Not present |
| gamepersistence.roblox.com | Game persistence | Not present |
| gamejoin.roblox.com | Game joining | Not present |
| gameinternationalization.roblox.com | Game internationalization | Not present |
| localizationtables.roblox.com | Localization tables | Not present |
| translationroles.roblox.com | Translation roles | Not present |
| translations.roblox.com | Translation functionality | Not present |

## 9.4 Social, communication and community

| Domain | Described function | In supplied dump |
|---|---|---|
| chat.roblox.com | Chat functionality | Present |
| friends.roblox.com | Friends and followers | Present |
| contacts.roblox.com | Contacts | Present |
| forum.roblox.com | Community forum | Present |
| followings.roblox.com | Follow relationships | Not present |
| presence.roblox.com | Presence | Not present |
| privatemessages.roblox.com | Private messages | Not present |
| notifications.roblox.com | Notifications | Not present |
| badges.roblox.com | Badges | Not present |
| avatar.roblox.com | Avatar customization | Not present |
| voice.roblox.com | Voice functionality | Not present |
| chatmoderation.roblox.com | Chat moderation | Not present |

## 9.5 Content delivery and assets

| Domain | Described function | In supplied dump |
|---|---|---|
| content.roblox.com | Content delivery | Present |
| images.roblox.com | Image hosting | Present |
| static.roblox.com | Static assets | Present |
| js.roblox.com | JavaScript assets | Present |
| css.roblox.com | Stylesheets | Present |
| assetdelivery.roblox.com | Asset delivery | Not present |
| thumbnails.roblox.com | Thumbnail service | Not present |
| thumbnailsresizer.roblox.com | Thumbnail resizing | Not present |
| contentstore.roblox.com | Temporary content storage | Not present |

## 9.6 Discovery and games

| Domain | Described function | In supplied dump |
|---|---|---|
| games.roblox.com | Game discovery and game information | Present |
| explore.roblox.com | Discovery-related frontend | Present |
| events.roblox.com | Events | Present |
| search.roblox.com | Search | Present |
| matchmaking.roblox.com | Matchmaking | Not present |
| realtime.roblox.com | Realtime functionality | Not present |

## 9.7 Platform and client configuration

| Domain | Described function | In supplied dump |
|---|---|---|
| lms.roblox.com | Latency measurement | Present |
| clientsettings.roblox.com | Client configuration | Not present |
| clientsettingscdn.roblox.com | Client configuration CDN | Not present |
| client-telemetry.roblox.com | Client telemetry | Not present |
| metrics.roblox.com | Metrics | Not present |
| textfilter.roblox.com | Text filtering | Not present |
| locale.roblox.com | Locale management | Not present |
| abtesting.roblox.com | Experimentation framework | Not present |

## 9.8 Moderation and safety

| Domain | Described function | In supplied dump |
|---|---|---|
| groupsmoderation.roblox.com | Group moderation | Not present |
| usermoderation.roblox.com | User moderation | Not present |
| chatmoderation.roblox.com | Chat moderation | Not present |

### Atlas interpretation

The atlas is a research inventory, not a verified live-domain census.

A hostname may be documented, historical, deprecated, internal to a client, or inaccessible without an appropriate session.

A domain's inclusion does not prove that its endpoints are publicly accessible.

---

## 9.9 Open Cloud v2 endpoint families

The original research notes identify the following endpoint families under:

```text
apis.roblox.com/cloud/v2/
```

The paths below are schematic. They are intended to describe API resource families, not serve as a substitute for the current official endpoint reference.

### Data stores

```text
/cloud/v2/universes/{universe_id}/data-stores/...
```

Associated functionality described in the original notes:

- List data stores.
- Snapshot data stores.
- List data store entries.
- Create entries.
- Retrieve entries.
- Update entries.
- Increment entries.
- Delete entries.
- List entry revisions.

### Memory stores

```text
/cloud/v2/universes/{universe_id}/memory-stores/...
```

Associated functionality:

- Sorted maps.
- Listing items.
- Creating items.
- Retrieving items.
- Updating items.
- Deleting items.
- Queue operations.
- Reading queue items.
- Discarding queue items.
- Memory store flushing.

### Ordered data stores

```text
/cloud/v2/universes/{universe_id}/ordered-data-stores/...
```

Associated functionality:

- Listing stores.
- Creating stores.
- Retrieving entries.
- Updating entries.
- Incrementing values.
- Deleting entries.

### Universe and place management

```text
/cloud/v2/universes/{universe_id}/...
```

The original notes identify:

- GetUniverse.
- UpdateUniverse.
- PublishUniverseMessage.
- RestartUniverseServers.
- GetPlace.
- UpdatePlace.
- GetInstance.
- UpdateInstance.

These operations have different authorization and state-change implications. They should be documented individually from official API references.

### Asset operations

```text
/cloud/v2/assets/...
```

The original notes identify:

- GetAsset.
- ListAssetVersions.
- GetAssetVersion.
- Upload operations.

### User operations

```text
/cloud/v2/users/{user_id}/...
```

The original notes identify:

- GetUser.
- GenerateUserThumbnail.
- ListInventoryItems.
- ListAssetQuota.

### Group operations

```text
/cloud/v2/groups/{group_id}/...
```

The original notes identify:

- GetGroup.
- ListGroupMemberships.
- UpdateGroupMembership.
- ListGroupRoles.
- GetGroupRole.
- ListGroupJoinRequests.
- AcceptGroupJoinRequest.
- DeclineGroupJoinRequest.
- GetGroupShout.

### Endpoint atlas conclusion

The Open Cloud surface represents a collection of resource-oriented APIs.

Its existence does not mean the original DNS scanner enumerated these resources.

The endpoint families must be checked against the official documentation before publication as a current, complete API specification.

---

# ░▒▓ [10] TIER LIST ERRORS ▓▒░

The original narrative mixes logical application functions with physical network destinations.

These are different layers of analysis.

| Original claim | What the inventory actually establishes |
|---|---|
| groups.roblox.com handles group fund distributions | The hostname resolves to a shared IP in the supplied table. The actual backend function is not established by that IP. |
| sales.roblox.com handles marketplace transaction logging | The hostname shares a reported address with community.roblox.com. That does not establish shared application logic. |
| billing.roblox.com handles subscriptions | The hostname resolves to 128.116.5.3. The IP does not reveal the billing backend. |
| upload.roblox.com is an asset ingestion pipeline | The supplied enrichment associates the destination with CloudFront. The origin and ingestion architecture remain unknown. |
| auth and pay are isolated security services | Their addresses are separately reported as Cloudflare-associated. This does not prove they are the only protected services. |

### The architectural distinction

```text
LOGICAL FUNCTION
    What the application does.

DNS DESTINATION
    Where the hostname resolves.

NETWORK PROVIDER
    Who operates or announces the address.

EDGE SERVICE
    Which infrastructure responds to the request.

ORIGIN
    The backend service receiving forwarded traffic.

APPLICATION
    The actual software and business logic.
```

These layers should not be collapsed into one classification.

**The tier list describes intended functionality. The DNS table describes observed destinations. Neither alone provides a complete architecture diagram.**

---

# ░▒▓ [11] DEEP DIVES ▓▒░

## 11.1 `vpn2.roblox.com` — 216.200.230.122

The original dataset identifies a VPN-labeled hostname.

The address is not shared with the other major clusters in the supplied table.

### Questions worth investigating

- Who is the registered holder of the IP?
- Which ASN announces its prefix?
- Is reverse DNS configured?
- Does the hostname currently resolve?
- Does the service present a TLS certificate?
- Does the certificate contain additional hostnames?
- Does HTTPS return a response?
- Is the record still current?

The `vpn2` naming pattern may suggest related names, but it does not prove that `vpn1` or `vpn3` exist.

### Passive DNS checks

```bash
for host in \
  vpn.roblox.com \
  vpn1.roblox.com \
  vpn2.roblox.com \
  vpn3.roblox.com
do
    printf '\n### %s\n' "$host"
    dig "$host" A +noall +answer
    dig "$host" CNAME +noall +answer
done
```

The original notes also mention:

```text
nrt-gw.vpn.cn.roblox.com
```

This name should be independently verified before treating it as an active regional gateway.

A naming convention is not proof of operational infrastructure.

### Security relevance

VPN products have historically been affected by serious vulnerabilities.

However, identifying a VPN product requires actual service fingerprinting and corroborating evidence.

A DNS record alone cannot identify Fortinet, Pulse Secure, Cisco AnyConnect, or another VPN implementation.

---

## 11.2 `explore.roblox.com` — 199.60.103.2

This hostname is one of the more interesting address outliers.

The original dataset associates it with:

```text
199.60.103.2
```

The original narrative notes that this address does not fit the provider classifications assigned to several other records.

### What needs to be established

- RDAP registration.
- Origin ASN.
- Announced prefix.
- Reverse DNS.
- Certificate information.
- HTTP response behavior.
- Whether the address changes across resolvers or locations.

### Why it matters

If the destination belongs to a third-party SaaS provider, that could indicate an external dependency.

If it belongs to Roblox or a contracted infrastructure provider, the interpretation changes.

Neither conclusion should be published without registration and routing evidence.

### Passive checks

```bash
dig explore.roblox.com A +noall +answer
dig explore.roblox.com AAAA +noall +answer
dig explore.roblox.com CNAME +noall +answer

curl -sS -D - -o /dev/null \
  https://explore.roblox.com/
```

A successful HTTP response is evidence of a response at that time, not evidence of application ownership.

---

## 11.3 `upload.roblox.com` — 3.173.161.96

This is the only hostname in the supplied dataset with a detailed enrichment record.

Original JSON:

```json
{
  "cpes": [
    "cpe:/a:amazon:amazon_cloudfront"
  ],
  "hostnames": [
    "webslice.us",
    "server-3-173-161-96.ams56.r.cloudfront.net"
  ],
  "ip": "3.173.161.96",
  "ports": [80, 443],
  "tags": [
    "cloud",
    "cdn"
  ],
  "vulns": []
}
```

### Finding 1: CloudFront association

The enrichment identifies the address with a CloudFront-related CPE and reverse-DNS name.

The `ams56` label appears in the reported edge hostname.

This is consistent with a CloudFront edge association.

It does not reveal the origin.

### Finding 2: `webslice.us`

The enrichment also lists:

```text
webslice.us
```

This is a hostname association requiring explanation.

Possible reasons include:

- Shared infrastructure.
- Reverse-DNS enrichment.
- Certificate relationships.
- Scanner correlation.
- Unrelated co-tenancy.

The supplied JSON does not establish which explanation is correct.

It does not establish that Roblox owns `webslice.us`.

### Finding 3: Ports 80 and 443

The scanner reports:

```text
80
443
```

These are the reported open ports for the address at scan time.

They do not establish that the origin is exposed or that arbitrary upload functionality is available.

### Finding 4: No reported vulnerabilities

The source reports:

```json
"vulns": []
```

This means no vulnerabilities were listed in that particular enrichment result.

It does not mean the application has been comprehensively tested or has no vulnerabilities.

### Finding 5: Edge versus origin

The distinction matters:

```text
Client
  |
  v
CloudFront edge
  |
  v
Unknown origin
  |
  v
Application or storage backend
```

The supplied record identifies an edge-associated destination.

The origin remains unknown.

### Conclusion

`upload.roblox.com` is the strongest positively enriched CDN example in the supplied dataset.

It is not sufficient evidence to identify the underlying upload architecture.

---

# ░▒▓ [12] WHAT IS NOT HERE ▓▒░

A DNS inventory is only one part of infrastructure research.

The following evidence sources would materially improve the report.

## 12.1 Certificate Transparency

Certificate Transparency logs provide historical records of publicly logged TLS certificates.

They can reveal additional certificate-associated names.

They cannot independently prove that a hostname is currently active.

### Example

```bash
curl -s \
  'https://crt.sh/?q=%25.roblox.com&output=json' \
  | jq -r '.[].name_value' \
  | sort -u
```

### What to record

- Certificate subject.
- Subject Alternative Names.
- Issuer.
- Validity dates.
- First-seen information where available.
- Last-seen information where available.
- Related hostnames.

Wildcard certificates and historical records must be interpreted carefully.

## 12.2 MX records

The original table lists `mail.roblox.com`, but an A record does not describe the domain's mail-routing configuration.

Check the actual mail records:

```bash
dig roblox.com MX +noall +answer
dig roblox.com TXT +noall +answer
dig _dmarc.roblox.com TXT +noall +answer
```

Additional SPF and DKIM analysis may help establish published mail authentication policy.

## 12.3 RDAP registration

RDAP provides registration information associated with IP resources.

Example:

```bash
curl -s \
  'https://rdap.org/ip/199.60.103.2' \
  | jq .
```

Another example:

```bash
curl -s \
  'https://rdap.org/ip/216.200.230.122' \
  | jq .
```

Registration data is not the same as application ownership.

## 12.4 ASN and BGP attribution

BGP routing information helps identify which autonomous system announces a prefix.

For each IP, record:

```text
IP address
Registered organization
Origin ASN
Announced prefix
Route visibility
Observation timestamp
```

Do not classify an entire network using only its first octet.

## 12.5 Reverse DNS

```bash
dig -x 199.60.103.2 +noall +answer
dig -x 216.200.230.122 +noall +answer
dig -x 3.173.161.96 +noall +answer
```

Reverse DNS is useful context.

It is not independently authoritative evidence of application ownership.

## 12.6 TLS metadata

A TLS handshake can provide certificate and protocol information.

Example:

```bash
openssl s_client \
  -connect vpn2.roblox.com:443 \
  -servername vpn2.roblox.com \
  </dev/null 2>/dev/null \
  | openssl x509 \
      -noout \
      -subject \
      -issuer \
      -dates \
      -ext subjectAltName
```

Record:

- Certificate subject.
- Certificate issuer.
- SAN entries.
- Validity period.
- Negotiated TLS version.
- Connection errors.

A certificate does not prove that the application is vulnerable or that a particular backend is exposed.

## 12.7 HTTP response metadata

```bash
curl -sS -D - -o /dev/null \
  https://vpn2.roblox.com/
```

Capture:

- HTTP status.
- Redirect location.
- Server header.
- Content type.
- Cache headers.
- Security headers.
- Request identifiers where present.

Headers can be omitted, modified, or generated by an edge layer.

Do not treat a generic server header as proof of a specific origin.

## 12.8 IPv6

The original table focuses on IPv4.

An additional inventory should check AAAA records:

```bash
while IFS= read -r host; do
    [ -n "$host" ] || continue

    printf '\n### %s\n' "$host"
    dig "$host" AAAA +noall +answer

done < hosts.txt
```

IPv6 results should be stored separately from IPv4 observations.

---

# ░▒▓ [13] THE ARCHITECTURE ▓▒░

The available evidence supports a mixed public-facing footprint.

The original dataset identifies:

- Shared IP destinations.
- Cloudflare-associated records.
- A CloudFront-associated record.
- Several separately addressed hostnames.
- A set of records without provider enrichment.
- A documented API hostname that is missing from the host-to-IP table.

A conceptual view of the observations:

```text
                    PUBLIC DNS
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
      128.116.x      104.18.x       3.173.x
      Shared IPs     Cloudflare     CloudFront
          |             |             |
          v             v             v
      Multiple       auth/pay       upload
      hostnames
          |
          |
          +-------------------------------+
                                          |
                              OTHER REPORTED DESTINATIONS
                                          |
                            AWS-associated / unclassified
                                          |
                                          v
                                 Application responses
```

This is an observation map, not a verified network diagram.

### The central architectural thesis

> Roblox's observed DNS footprint includes shared network destinations and separately addressed cloud-associated services. The supplied evidence does not reveal the complete backend architecture, origin topology, or security-control deployment.

### What the evidence does support

1. Several hostnames share IP addresses.
2. The original enrichment explicitly associates two addresses with Cloudflare.
3. The original enrichment associates one address with CloudFront.
4. The table contains many `N/A` enrichment classifications.
5. `apis.roblox.com` appears in the narrative without an IP entry.
6. `vpn2.roblox.com` and `explore.roblox.com` have distinct destinations in the supplied dataset.

### What remains unproven

1. Whether the shared addresses are anycast.
2. Whether they are reverse proxies.
3. Whether they use a common backend.
4. Whether the reported provider associations remain current.
5. Whether the named internal-sounding hosts expose applications.
6. Whether any hostname has a security vulnerability.
7. Whether the listed domains represent the complete Roblox API surface.

---

# ░▒▓ [14] RECON NEXT STEPS ▓▒░

The next collection pass should prioritize reproducibility over volume.

Every observation should have a timestamp, raw output, and interpretation.

## 14.1 Build the hostname inventory

Create `hosts.txt` with one hostname per line.

```text
about.roblox.com
ads.roblox.com
affiliates.roblox.com
apis.roblox.com
auth.roblox.com
billing.roblox.com
blog.roblox.com
careers.roblox.com
catalog.roblox.com
chat.roblox.com
community.roblox.com
contacts.roblox.com
content.roblox.com
corp.roblox.com
corporate.roblox.com
create.roblox.com
css.roblox.com
data.roblox.com
de.roblox.com
design.roblox.com
develop.roblox.com
developer.roblox.com
es.roblox.com
events.roblox.com
explore.roblox.com
forum.roblox.com
fr.roblox.com
friends.roblox.com
games.roblox.com
gold.roblox.com
groups.roblox.com
help.roblox.com
images.roblox.com
jira.roblox.com
job.roblox.com
jobs.roblox.com
js.roblox.com
learn.roblox.com
lms.roblox.com
m.roblox.com
mail.roblox.com
news.roblox.com
nl.roblox.com
partners.roblox.com
pay.roblox.com
pt.roblox.com
research.roblox.com
sales.roblox.com
search.roblox.com
setup.roblox.com
share.roblox.com
shop.roblox.com
silver.roblox.com
static.roblox.com
status.roblox.com
survey.roblox.com
uk.roblox.com
upload.roblox.com
users.roblox.com
vpn2.roblox.com
web.roblox.com
wiki.roblox.com
www.roblox.com
```

## 14.2 Collect DNS records

```bash
#!/usr/bin/env bash

set -u

INPUT="hosts.txt"
OUTPUT="dns-results.txt"

if [[ ! -f "$INPUT" ]]; then
    echo "Missing hosts.txt"
    exit 1
fi

{
    echo "# Roblox DNS inventory"
    echo "# Timestamp: $(date -u +%FT%TZ)"
    echo

    while IFS= read -r host; do
        [[ -z "$host" ]] && continue

        echo "================================"
        echo "HOST: $host"
        echo "================================"

        for type in A AAAA CNAME; do
            echo
            echo "RECORD: $type"

            dig "$host" "$type" \
                +noall +answer
        done

        echo

    done < "$INPUT"

} > "$OUTPUT"

echo "Results written to $OUTPUT"
```

This preserves record types instead of combining all responses into an ambiguous address field.

## 14.3 Compare DNS observations

Run the collection at different times.

Record:

- Timestamp.
- Resolver.
- Record type.
- TTL.
- Returned value.
- Query status.

Different answers do not automatically indicate malicious activity.

They may result from CDN routing, DNS rotation, regional configuration, or ordinary infrastructure changes.

## 14.4 Inspect TLS certificates

For hosts that resolve and are in scope:

```bash
#!/usr/bin/env bash

while IFS= read -r host; do
    [[ -z "$host" ]] && continue

    echo
    echo "### $host"

    timeout 10 openssl s_client \
        -connect "${host}:443" \
        -servername "$host" \
        </dev/null 2>/dev/null \
        | openssl x509 \
            -noout \
            -subject \
            -issuer \
            -dates \
            -ext subjectAltName

done < hosts.txt
```

This records certificate information for analysis.

A failed handshake should be recorded as a failed observation, not as proof that the hostname is nonexistent.

## 14.5 Collect HTTP metadata

```bash
#!/usr/bin/env bash

while IFS= read -r host; do
    [[ -z "$host" ]] && continue

    echo
    echo "### $host"

    curl \
        --connect-timeout 5 \
        --max-time 10 \
        -sS \
        -D - \
        -o /dev/null \
        "https://${host}/"

done < hosts.txt
```

The procedure makes ordinary unauthenticated HTTPS requests.

Do not use it to submit credentials, modify accounts, upload files, or perform state-changing API operations.

## 14.6 Attribute distinct addresses

Deduplicate the IP list before looking up registration and routing information.

Suggested output format:

```text
IP
RDAP ORGANIZATION
ORIGIN ASN
ANNOUNCED PREFIX
REVERSE DNS
PROVIDER
TIMESTAMP
CONFIDENCE
```

Do not infer provider ownership from an address's first octet.

## 14.7 Reconcile the API atlas

For each API hostname, record:

```text
Hostname
Official documentation reference
Documented API version
Resource family
Authentication requirements
Observed DNS status
Observed HTTP status
Last verified
Confidence
```

Separate official documentation from community-maintained research.

## 14.8 Preserve evidence

Use a structured directory:

```text
roblox-recon/
|
+-- inventory/
|   +-- hosts.txt
|   +-- original-host-table.txt
|
+-- dns/
|   +-- initial-results.txt
|   +-- repeat-results.txt
|
+-- tls/
|   +-- certificates.txt
|
+-- http/
|   +-- response-headers.txt
|
+-- attribution/
|   +-- rdap.txt
|   +-- asn-bgp.txt
|   +-- reverse-dns.txt
|
+-- api/
|   +-- endpoint-atlas.md
|   +-- documentation-sources.md
|
+-- reports/
    +-- findings.md
    +-- limitations.md
```

---

# ░▒▓ [15] EVIDENCE & LIMITATIONS ▓▒░

A useful recon publication distinguishes direct observations, derived findings, hypotheses, and unsupported claims.

This section establishes that distinction for Issue #001.

## 15.1 Evidence classification

| Classification | Meaning | Example |
|---|---|---|
| Observed | Directly present in supplied data | `auth.roblox.com` maps to 104.18.2.63 |
| Derived | Calculated from observations | Multiple hostnames share an IP |
| Enriched | Added by an external scanner | CloudFront CPE |
| Hypothesis | Possible explanation | Shared reverse-proxy layer |
| Unverified | Requires independent evidence | VPN appliance identity |
| Unsupported | Not established by available evidence | No WAF exists |

## 15.2 Confidence levels

<text>High confidence</text>

The exact hostname and address appear in the supplied inventory.

<text>Moderate confidence</text>

An interpretation is consistent with multiple independent observations but still requires verification.

<text>Low confidence</text>

An interpretation is based primarily on naming, incomplete enrichment, or a single indirect clue.

<text>Unverified</text>

The available material does not support a conclusion.

## 15.3 Finding register

| ID | Finding | Evidence status |
|---|---|---|
| RZ-001 | Multiple hostnames resolve to 128.116.5.3 | Observed |
| RZ-002 | Multiple hostnames resolve to 128.116.31.3 | Observed |
| RZ-003 | auth.roblox.com is associated with 104.18.2.63 | Observed in source |
| RZ-004 | pay.roblox.com is associated with 104.18.35.156 | Observed in source |
| RZ-005 | upload.roblox.com has CloudFront enrichment | Observed in source |
| RZ-006 | apis.roblox.com lacks an IP in the supplied table | Observed |
| RZ-007 | vpn2.roblox.com has a distinct reported destination | Observed |
| RZ-008 | explore.roblox.com has a distinct reported destination | Observed |
| RZ-009 | The 128.116 addresses represent an internal edge layer | Hypothesis |
| RZ-010 | The named VPN endpoint is an exposed VPN appliance | Unverified |
| RZ-011 | The Jira hostname exposes an internal application | Unverified |
| RZ-012 | The upload origin is exposed | Unsupported |
| RZ-013 | N/A means no provider enrichment | Interpretation of source output |

## 15.4 Count reconciliation

The original narrative contains multiple approximate totals.

Before publication, recalculate them from the raw input.

Potential sources of discrepancy include:

- Duplicate hostnames.
- Duplicate IP addresses.
- Missing address values.
- Different definitions of a resolved host.
- Multiple record types.
- Historical versus current DNS results.
- Hostnames appearing in narrative text but not in the table.

In particular, distinguish:

```text
TOTAL UNIQUE HOSTNAMES
TOTAL HOSTNAMES WITH A RECORD
TOTAL UNIQUE IPV4 ADDRESSES
TOTAL UNIQUE IPV6 ADDRESSES
TOTAL CNAME RECORDS
TOTAL UNCLASSIFIED HOSTS
TOTAL PROVIDER-ATTRIBUTED HOSTS
```

These are different metrics.

## 15.5 Scope and authorization

This publication documents supplied DNS observations and publicly available metadata.

It does not establish authorization to test Roblox systems.

Any additional testing should remain within an explicitly authorized scope.

Avoid:

- Attempting to access other users' accounts.
- Testing leaked or arbitrary credentials.
- Accessing private user data.
- Modifying group membership.
- Publishing assets or files.
- Restarting game servers.
- Modifying production data stores.
- Testing state-changing API methods without authorization.

Public documentation is not permission to access private resources.

---

# ░▒▓ [16] THE TAKEAWAY ▓▒░

The original dump is a useful starting point, but it is not a complete map of Roblox's infrastructure.

The strongest observations are straightforward:

- Multiple Roblox hostnames share a small number of reported IP destinations.
- Two records are explicitly associated with Cloudflare in the original enrichment.
- One record is associated with CloudFront.
- Most hostnames have no provider classification in the supplied data.
- `apis.roblox.com` is discussed in the narrative but lacks an IP entry in the table.
- `vpn2.roblox.com` and `explore.roblox.com` have distinct destinations worth independently attributing.
- The endpoint atlas describes a broader API surface than the supplied DNS inventory captures.

### What Issue #001 establishes

A partial public-facing hostname and infrastructure inventory.

### What Issue #001 does not establish

A verified internal network map, complete API census, vulnerability assessment, or origin-server inventory.

### What Issue #002 should deliver

A stronger evidence package containing:

1. Repeated DNS observations.
2. IPv4 and IPv6 results.
3. Certificate Transparency references.
4. TLS certificate metadata.
5. RDAP registration records.
6. ASN and BGP attribution.
7. Reverse-DNS observations.
8. HTTP response metadata.
9. Verified API documentation references.
10. Reconciled hostname and address counts.
11. Clear separation of observation and interpretation.

The objective is not simply to find more hostnames.

It is to understand what each observation actually proves.

> **Do not confuse an address with an application, an edge with an origin, a hostname with a vulnerability, or a missing row with a missing service.**

---

# ░▒▓ [17] APPENDIX ▓▒░

## A. Terminology

| Term | Definition |
|---|---|
| DNS | Domain Name System |
| A record | IPv4 address record |
| AAAA record | IPv6 address record |
| CNAME | Canonical name alias |
| MX | Mail exchange record |
| TXT | DNS text record |
| TTL | Time to live |
| ASN | Autonomous System Number |
| BGP | Border Gateway Protocol |
| RDAP | Registration Data Access Protocol |
| CT | Certificate Transparency |
| TLS | Transport Layer Security |
| SAN | Subject Alternative Name |
| CDN | Content Delivery Network |
| WAF | Web Application Firewall |
| CPE | Common Platform Enumeration |
| API | Application Programming Interface |
| Origin | Backend destination behind an edge or proxy |

## B. Suggested evidence record

Every new observation should be documented using this structure:

```text
FINDING ID:
TARGET:
OBSERVATION DATE:
OBSERVATION TIME (UTC):
RECORD TYPE:
RESOLVER:
RAW RESULT:
TOOL:
TOOL VERSION:
SOURCE:
INTERPRETATION:
CONFIDENCE:
LIMITATIONS:
```

## C. Original dataset notes

The original inventory contains the following claims that should remain marked as approximate or unverified until independently reconciled:

```text
67 resolvable hostnames
68 total host references
21 hosts across two principal IPs
70+ API domains
Approximately 45+ additional API domains
Approximately 1/3 API coverage
14 single-host outliers
Approximately 55 AWS-associated hosts
```

These numbers are not all independently established by the supplied raw evidence.

The published inventory should be recalculated before treating them as exact.

## D. Suggested official references

Use the current documentation rather than relying exclusively on copied endpoint lists.

- Roblox Creator Hub: https://create.roblox.com/docs
- Roblox Open Cloud: https://create.roblox.com/docs/cloud
- Roblox API reference: https://create.roblox.com/docs/reference
- Roblox security information: https://about.roblox.com/security
- Certificate Transparency search: https://crt.sh/
- RDAP bootstrap service: https://rdap.org/
- IANA IP address registry: https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xhtml
- IANA AS number registry: https://www.iana.org/assignments/as-numbers/as-numbers.xhtml

Verify exact documentation paths and current endpoint behavior before citing them as authoritative support for individual claims.

---

# ═══════════════════════════════════════════════════════════
#                       END OF ISSUE #001
# ═══════════════════════════════════════════════════════════

```text
    ██████╗  ██████╗ ██████╗ ██╗      ██████╗ ██╗  ██╗
    ██╔══██╗██╔═══██╗██╔══██╗██║     ██╔═══██╗╚██╗██╔╝
    ██████╔╝██║   ██║██████╔╝██║     ██║   ██║ ╚███╔╝
    ██╔══██╗██║   ██║██╔══██╗██║     ██║   ██║ ██╔██╗
    ██║  ██║╚██████╔╝██████╔╝███████╗╚██████╔╝██╔╝ ██╗
    ╚═╝  ╚═╝ ╚═════╝ ╚═════╝ ╚══════╝ ╚═════╝ ╚═╝  ╚═╝

                 R O B L O X   R E C O N
                     ISSUE #001
                   ULTRA EDITION

                    made by SleepTheGod

             EVIDENCE FIRST. ATTRIBUTION SECOND.
                   CONCLUSIONS LAST.
```

**END OF TRANSMISSION.**
