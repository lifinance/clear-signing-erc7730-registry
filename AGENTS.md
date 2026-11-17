# AGENTS.md

LI.FI's fork of the ERC-7730 clear signing metadata registry. It holds JSON descriptors that let
wallets show human-readable details for contract calls and EIP-712 messages. LI.FI's own descriptor is
`registry/lifi/calldata-LIFIDiamond.json`; changes to it are prepared here and proposed upstream.

## Layout

| Path | What lives there |
| --- | --- |
| `registry/<entity>/` | One folder per submitting entity: `calldata-*.json`, `eip712-*.json`, shared `common-*.json`, optional `tests/` samples |
| `registry/lifi/` | LI.FI's descriptor for the LI.FI Diamond |
| `ercs/` | Descriptors for standard interfaces (ERC-20, ERC-721, ERC-4626, ERC-2612 permit) |
| `specs/` | The ERC-7730 spec text, the JSON schema `erc7730-v1.schema.json`, and `templates/` |
| `.github/workflows/` | Upstream CI (lint, format, setup); it runs on upstream's runners, not in this fork |

## Validate and format

There is no package.json or Makefile. Validation uses the `erc7730` Python package (Python 3.12+),
pinned for CI in `.github/requirements.txt`:

```bash
pip install erc7730
erc7730 lint registry/lifi/calldata-LIFIDiamond.json
erc7730 lint registry/**/eip712-*.json registry/**/calldata-*.json
erc7730 format
```

The repo reads no environment variables.

## Conventions

- A PR touches one entity folder only (one sub-folder of `registry/`).
- Descriptor files start with `calldata-` (contracts) or `eip712-` (EIP-712 messages); shared files
  included by them never use those prefixes.
- Every descriptor validates against `specs/erc7730-v1.schema.json`.
- Keep this fork close to upstream: change only LI.FI's entity folder and LI.FI-specific files, so syncs
  with upstream stay clean.

More: [README.md](README.md) (submission process), [specs/erc-7730.md](specs/erc-7730.md).

## Ownership

- Owner: API Expansion squad, reviewer Daniela (`@melianessa`).
- Review channel: `#dev-sc-review`.
