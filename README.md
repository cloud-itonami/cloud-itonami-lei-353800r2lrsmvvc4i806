# cloud-itonami-lei-353800r2lrsmvvc4i806

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Recruit Holdings Co., Ltd..**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**Recruit Holdings Co., Ltd.**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Recruit Holdings Co., Ltd. — the registered legal name on the live
  GLEIF record is the Japanese `株式会社リクルートホールディングス` (language `ja`);
  `Recruit Holdings Co.,Ltd.` is carried there as an `ALTERNATIVE_LANGUAGE_LEGAL_NAME`
  in English. `facts.edn` below records the registry's own legal name.
- **LEI (ISO 17442)**: [353800R2LRSMVVC4I806](https://search.gleif.org/#/record/353800R2LRSMVVC4I806) (GLEIF-verified)
- **Jurisdiction**: JP
- **Website**: https://recruit-holdings.com
- **Ticker**: 6098 (TSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 20 verified registry facts with per-fact provenance (9 about the
  entity, its registration, issuer, legal form, securities count, children count and
  both parent-reporting exceptions; 2 one-per-ISIN; 9 one-per-direct-child).
  **Generated** — see below.
- `scripts/verify-facts.cljs` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljs           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO requests back the file (`CHECKED 11` when it was written,
2026-08-23T06:51Z, golden copy 2026-08-22T16:00Z) — the LEI record (legal name
`株式会社リクルートホールディングス`, jurisdiction `JP`, entity **ACTIVE**,
registration **ISSUED** with the next renewal due 2027-08-08, last updated
2026-08-08, `FULLY_CORROBORATED`, conformity flag `CONFORMING`, BIC
`RCRTJPJJXXX`, corporate number `0100-01-060426`; entity status and
registration status are different fields and are recorded separately), its
**2 ISINs** (`JP3970300004`, `US75629J1016`), read from `meta.pagination.total`
of a 15-per-page request — the whole list fits in that single page, so each
identifier is also mirrored as its own `:security` entity (the `:source/note` on
the count says whether the list under a count is mirrored or only counted, so a
bare count is never ambiguous) — its managing LOU and LEI-issuer accreditation
(株式会社東京証券取引所, LEI `353800279ADEFGKNTV65`, marketing name Japan
Exchange Group / Tokyo Stock Exchange (JPX/TSE), accredited 2017-07-21),
registration authority `RA000412` (Companies Registration, 法務局 / Legal
Affairs Bureau, Japan), ISO 20275 legal form `T417` (`株式会社`, JP), reporting
exceptions at both consolidation levels (`NO_KNOWN_PERSON` — GLEIF's
relationship data records no accounting-consolidation parent above this entity,
consistent with a listed holding company at the top of its own group), and
**9 direct children**, read from `meta.pagination.total` of the cited page and,
because they fit in that one page, each mirrored as a `:direct-child` entity:
USG Public-Sourcing, RGF Staffing Belgium, USG Professionals, Bright Plus
Outsourcing Solutions, UNIQUE, Start People, Solvus, RECEPTEL and Bright Plus —
all nine Belgian (`BE`) and ACTIVE, which is the Belgian staffing group
consolidated directly under the holding company in GLEIF's data. That is what
GLEIF's relationship data records as direct children, not a roster of the
group's operating companies: an entity absent from that page either has no LEI
or reports no `IS_DIRECTLY_CONSOLIDATED_BY` relationship to this one, and this
file records only what the cited pages say. The `direct-parent` and
`ultimate-parent` endpoints answered
`404` because GLEIF publishes the exception side of that pair for this entity,
which the checker treats as a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the
live sources, `1` a citation broke or a fact drifted, `3` the check could not be
performed at all — an absent `facts.edn`, or every request failing at the
transport level. A check that could not run must not be indistinguishable from a
check that ran and found nothing, so it refuses to report a pass rather than
exiting 0. All outcomes were exercised before this landed: unmodified `0`;
`:securities/isin-count` edited `2` → `3` → `1` naming
`DRIFT gleif-isins :securities/isin-count`; the `gleif-isin-jp3970300004`
entity deleted → `1` naming `ADDED gleif-isin-jp3970300004`; that same entity's
`:securities/isin` value rewritten to a non-existent identifier → `1` naming
`DRIFT gleif-isin-jp3970300004 :securities/isin`; the measured
`:relationship/direct-child-count` rewritten `9` → `0` → `1` naming
`DRIFT gleif-direct-children-count`; `:company/jurisdiction` rewritten to
`US-DE` → `1` naming `DRIFT gleif-lei-record :company/jurisdiction`; a
direct child's `:company/legal-name` rewritten → `1` naming
`DRIFT gleif-direct-child-967600uszdtfrbbhrf24 :company/legal-name`; the GLEIF
host in the checker rewritten to an unresolvable name → `3` (`INCONCLUSIVE …
refusing to report a pass`).

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
