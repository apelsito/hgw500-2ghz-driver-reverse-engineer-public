# HGW500 2.4 GHz driver reverse engineering

> Public source snapshot. [Scope, provenance and verification](docs/PUBLIC_SNAPSHOT.md).

This repository preserves the Realtek RTL8192EE host-driver reconstruction used to understand the HGW500 radio. It holds the Linux 5.10 reference, original source comments, recovered operation tables and readable REA exports. The actively developed Linux 7.3 driver lives in the linked port repository.

## How this component fits

```mermaid
flowchart LR
    A["Original driver modules and GPL reference"] --> B["REA: C-like output and ARC assembly"]
    B --> C["Operation tables and callback reconstruction"]
    C --> D["Linux 5.10 baseline source"]
    D --> E["Paired build and byte comparison"]
    E -. "reference, not inherited acceptance" .-> F["Separate Linux 7.3 port"]
```

This diagram shows software ownership and analysis flow, not a complete board schematic.
The [system architecture](docs/SYSTEM_ARCHITECTURE.md) explains the ARC750 host,
MuC, AuC, DSP and Realtek 8051, with their memory and firmware boundaries.

## What is established

| Area | Recorded scope |
| --- | --- |
| Reference | Linux 5.10.260 / F363-F367r9 lineage; not the current 7.3 runtime. |
| Readable analysis | 366 unique function exports representing 370 reference symbols; C, assembly and retained REA reports. |
| Preservation | 625 original files and 3,630 original comments recorded by the existing preservation manifests. |
| Reconstruction | Historical paired build of 27 units and four modules; inspect the receipt for the exact baseline. |

## Start reading here

1. [Component handoff](docs/PROJECT_HANDOFF.md): inputs, evidence and next work.
2. [System architecture](docs/SYSTEM_ARCHITECTURE.md): processors and firmware families.
3. [Firmware contents](docs/FIRMWARE_MAP.md): OEM, Core Boot R1 and API v2 R3.
4. [Repository map](docs/REPOSITORY_MAP.md): host drivers, payloads and integration.
5. [Published checkout availability](docs/CHECKOUT_AVAILABILITY.md): actual remote source inventory.
6. [Preserved technical notes](docs/REFERENCE_NOTES.md): detailed earlier results, with workstation paths normalized.

## Work in progress

Recover missing types and indirect-call meaning against the preserved assembly. Keep the exact 5.10 reconstruction separate from changes made for 7.3. A matching module hash is not a radio traffic test.

## Offline verification


```sh
python tools/verify_public_export.py
```

This checks the published file inventory. Original preservation verifiers and
receipts describe the private source checkpoint; they can require omitted native
databases or external inputs and are not a fresh public-snapshot qualification.

## Preservation and scope

This is a public source snapshot; per-unit data and original private histories remain private. Original vendor/GPL source, notices, comments,
carriers and immutable evidence retain their authorship and provenance. Derived
C is labelled as inferred analysis, not silently presented as original source.
Credentials, unit identity, calibration, personalized rootfs/flash images and
workstation-specific configuration stay outside documentation exports.

Documentation prepared 2026-11-10 from recorded source/evidence. No device access,
firmware installation, radio change or new physical qualification is implied.
