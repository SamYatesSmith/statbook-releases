# statbook-releases

Public, append-only trust artifacts for **[Statbook](https://statbook.co.uk)** —
a versioned registry of UK statutory figures.

This repository exists so anyone can verify a Statbook release **without
trusting Statbook's servers** (NF-4, NF-10). It contains only public trust
artifacts — no application code:

- `releases/<version>/manifest.json` — per-record SHA-256 hashes,
  diff-from-previous, and release notes.
- `releases/<version>/manifest.json.minisig` — the manifest's detached
  minisign signature.
- `releases/<version>/dataset.json`, `dataset.sqlite` — the release dataset
  (the free, delayed bulk download; FR-25).
- `keys/minisign.pub` — the public key every manifest is signed with.
- `CORRECTIONS.md` — the numbered corrections log.

## Signing key (ADR-0007)

Every release manifest is signed with this minisign public key
(key ID `12D5059844B2CD8F`):

```
untrusted comment: minisign public key 12D5059844B2CD8F
RWSPzbJEmAXVEvTqYZ8O9iHuOcOUii5lCNZM5t9bdoQh4ZnwKsxk/+bd
```

The same key is published on the site
(<https://statbook.co.uk/assurance/#verify>) and in every API response under
`meta.signing`. Cross-check all three — no single source can substitute a
key of its own.

## Verify a release

```sh
# 1. verify the manifest's signature against the published key
minisign -Vm releases/1.0.1/manifest.json -p keys/minisign.pub

# 2. take any record from the API, recompute its SHA-256, and confirm the
#    hash appears in manifest.json under record_hashes. Full walkthrough:
#    https://statbook.co.uk/assurance/#verify
```

A self-contained reference verifier ships in the main project as
`pipeline/verify_receipt.py`.

## Append-only — history is never rewritten

This repository is **append-only and never force-pushed** (NF-10). New
releases and corrections arrive as new commits; nothing is ever removed or
amended. A rewrite of this history is detectable by any prior cloner and
would itself be evidence of tampering. If you rely on Statbook, clone this
repository and keep it: a divergence between your clone and this remote is a
signal worth investigating.
