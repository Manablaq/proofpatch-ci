# ProofPatch CI Evidence

Immutable CI evidence publisher for ProofPatch consensus-gated GenLayer upgrades.

## Purpose

This repository publishes machine-readable CI evidence consumed by the deployed `ProofPatchGovernor`.

It is intentionally separate from the protected application source repository so source provenance and CI provenance are independently bound in each upgrade proposal.

## Registered authority

When the Bradbury target policy is registered, this repository is intended to use:

- authority identifier: `Manablaq/proofpatch-ci`
- raw GitHub prefix: `https://raw.githubusercontent.com/Manablaq/proofpatch-ci/`

The on-chain ProofPatch policy is the authoritative registration record.

## Immutable evidence rule

Every CI evidence URL supplied to ProofPatch must contain an exact 40-character Git commit SHA. Branch URLs such as `main` must never be used as proposal evidence.

Valid form:

`https://raw.githubusercontent.com/Manablaq/proofpatch-ci/<40-hex-commit>/evidence/<record>.json`

Evidence records are append-only reviewer artifacts. If evidence needs correction, create a new record with a new evidence ID in a new commit and reference that exact immutable commit.

Do not reuse an evidence ID already consumed by a ProofPatch proposal.

## CI evidence contract

The deployed governor expects schema `proofpatch-evidence-v1` with evidence kind `ci`.

Every record binds:

- protected target address;
- parent source SHA-256;
- candidate source SHA-256;
- ProofPatch policy fingerprint;
- registered CI issuer;
- unique evidence ID;
- publication timestamp;
- expiry timestamp.

The following checks must each be the JSON boolean `true`:

- `genvm_lint`
- `typecheck`
- `schema`
- `direct_tests`
- `adversarial_tests`
- `proofpatch_interface_tests`

See `schema/proofpatch-ci-evidence-v1.schema.json`.

## Publication discipline

Do not publish a passing evidence record until the exact candidate bytes are frozen, its SHA-256 is known, the target policy fingerprint is known, all stated checks actually passed, and the timestamps describe the real publication window.

There is deliberately no fabricated PASS evidence in this repository.
