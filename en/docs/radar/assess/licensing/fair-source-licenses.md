---
title: Fair Source Licenses
description: The three licenses recommended by fair.io, the Functional Source License, the Fair Core License, and the Business Source License, compared.
date: 2026-09-30
author: Christopher Steel
---

# Fair Source Licenses

## Overview

Fair Source is a middle ground between closed source and Open Source: code is public to read, usable with limited restrictions, and becomes Open Source after a set delay <a id="cite-fair-source-nd-a-1"></a>[(Fair Source, n.d.-a)](#ref-fair-source-nd-a).

Fair Source Software (FSS) meets three criteria:

1. Its source is publicly available to read.
2. It allows use, modification, and redistribution, with minimal restrictions that protect the producer's business model.
3. It undergoes Delayed Open Source Publication (DOSP), meaning each release is later republished under an OSI-approved license on a planned schedule.

Points one and three are intended as bright lines; point two is left open so licenses can vary. DOSP is the stated differentiator from Open Core, and the safeguard if the vendor folds or changes direction. The initiative was started by Sentry in 2024 and is governed through its GitHub repository <a id="cite-fair-source-nd-a-2"></a>[(Fair Source, n.d.-a)](#ref-fair-source-nd-a).

## The licenses

Fair.io recommends three licenses: the Functional Source License as its flagship, with the Fair Core License and the Business Source License as alternates <a id="cite-fair-source-nd-b-1"></a>[(Fair Source, n.d.-b)](#ref-fair-source-nd-b). It notes other licenses may also align with its goals.

### Functional Source License (FSL)

[FSL](https://fsl.software/) is a simple non-compete license, created by Sentry, that converts to Apache 2.0 or MIT after two years <a id="cite-fsl-nd-1"></a>[(Functional Source License, n.d.)](#ref-fsl-nd).

- Current texts: [FSL-1.1-ALv2](https://github.com/getsentry/fsl.software/blob/main/FSL-1.1-ALv2.template.md) and [FSL-1.1-MIT](https://github.com/getsentry/fsl.software/blob/main/FSL-1.1-MIT.template.md).
- Permitted: running it for almost any purpose, studying, modifying, and redistributing changes; the one restriction is a use that undermines the producer, such as a competing product.
- Conversion: the two-year clock runs per released version (a Git commit, a package, or other distribution), so a commit older than two years is already usable under the change license.
- Change license is fixed: only Apache 2.0 or MIT; any other choice must not be called FSL.
- Designed for SaaS companies; the FSL authors describe it as an evolved BSL, with no per-project Additional Use Grant and a shorter delay.

### Fair Core License (FCL)

[FCL](https://fcl.dev/) is an FSL variant from Keygen that adds license-key gating for commercial features, blending FSL 1.1 with terms drawn from the Elastic License 2.0 <a id="cite-keygen-nd-1"></a>[(Keygen, n.d.)](#ref-keygen-nd).

- Current texts: [FCL-1.0-ALv2](https://github.com/keygen-sh/fcl.dev/tree/master/FCL-1.0-ALv2.md) and [FCL-1.0-MIT](https://github.com/keygen-sh/fcl.dev/tree/master/FCL-1.0-MIT.md); drafted by Heather Meeker.
- Permitted: read, use, modify, and redistribute for any use that does not compete with the author's business; the license also protects license-key-gated features from unauthorized access.
- Conversion: two years per released version, to Apache 2.0 or MIT, same mechanics as FSL.
- Fit: self-hosted or on-premise products with an enterprise tier, keeping core and commercial features in one codebase under one license. Unlike Open Core, the commercial features also become Open Source.

### Business Source License (BUSL or BSL)

[BUSL](https://mariadb.com/bsl11/) is the older, more complex non-compete license from MariaDB, and the direct predecessor of FSL <a id="cite-mariadb-nd-1"></a>[(MariaDB, n.d.;](#ref-mariadb-nd) <a id="cite-fair-source-nd-b-2"></a>[Fair Source, n.d.-b)](#ref-fair-source-nd-b).

- Permitted: non-production use is always free; production use is allowed only as far as the licensor's Additional Use Grant says, which differs per project.
- Conversion: on the licensor's Change Date or the fourth anniversary of first public distribution, whichever comes first.
- Change license: chosen by the licensor, but must be GPL 2.0 or later, or a GPL-compatible Open Source license.
- Caution: because each Additional Use Grant is custom, every BUSL project must be read individually.

## Comparison

FSL and FCL share the same two-year, per-version conversion to Apache 2.0 or MIT; BUSL differs on almost every axis.

| Attribute | FSL | FCL | BUSL |
| --- | --- | --- | --- |
| Fair.io status | Recommended (flagship) | Alternate | Alternate |
| Steward | Sentry | Keygen | MariaDB |
| Current version | 1.1 | 1.0 | 1.1 |
| Main restriction | No competing use | No competing use, license-key gating of commercial features | Production use limited to the Additional Use Grant |
| Delay to Open Source | 2 years per version | 2 years per version | Change Date, at most 4 years |
| Change license | Apache 2.0 or MIT | Apache 2.0 or MIT | Licensor's choice, GPL 2.0+ compatible |
| Terms vary per project | No | No | Yes, via the Additional Use Grant |
| Typical fit | SaaS products | Self-hosted products with paid tiers | Varies by licensor |

Sources: <a id="cite-fsl-nd-2"></a>[Functional Source License (n.d.)](#ref-fsl-nd), <a id="cite-keygen-nd-2"></a>[Keygen (n.d.)](#ref-keygen-nd), <a id="cite-mariadb-nd-2"></a>[MariaDB (n.d.)](#ref-mariadb-nd).

## Considerations

Fair Source software is not Open Source at release, so vetting it means checking what the non-compete covers and when each version converts.

- Not OSI Open Source until conversion; policies that require OSI licenses will treat current versions as proprietary.
- "Competing use" is defined by the producer's business, so it can shift if the producer changes what it sells.
- Per-version conversion means older releases are Open Source today, a usable fallback if the vendor folds.
- FSL and FCL avoid copyleft; their change licenses are permissive. BUSL converts to a GPL-compatible license, which may be copyleft.
- For BUSL, read the specific Additional Use Grant and Change Date; they decide whether production use is free.
- For FCL, commercial features stay behind license keys until each version converts.

## References

<a id="ref-fair-source-nd-a"></a>Fair Source. (n.d.-a). *About Fair Source*. Retrieved September 30, 2026, from <https://fair.io/about/> [↩ 1](#cite-fair-source-nd-a-1) [↩ 2](#cite-fair-source-nd-a-2)

<a id="ref-fair-source-nd-b"></a>Fair Source. (n.d.-b). *Fair Source licenses*. Retrieved September 30, 2026, from <https://fair.io/licenses/> [↩ 1](#cite-fair-source-nd-b-1) [↩ 2](#cite-fair-source-nd-b-2)

<a id="ref-fsl-nd"></a>Functional Source License. (n.d.). *FSL: Functional Source License*. Sentry. Retrieved September 30, 2026, from <https://fsl.software/> [↩ 1](#cite-fsl-nd-1) [↩ 2](#cite-fsl-nd-2)

<a id="ref-keygen-nd"></a>Keygen. (n.d.). *FCL: Fair Core License*. Retrieved September 30, 2026, from <https://fcl.dev/> [↩ 1](#cite-keygen-nd-1) [↩ 2](#cite-keygen-nd-2)

<a id="ref-mariadb-nd"></a>MariaDB. (n.d.). *MariaDB Business Source License (BSL): Frequently asked questions*. Retrieved September 30, 2026, from <https://mariadb.com/bsl-faq-mariadb/> [↩ 1](#cite-mariadb-nd-1) [↩ 2](#cite-mariadb-nd-2)
