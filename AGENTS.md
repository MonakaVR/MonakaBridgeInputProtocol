# MonakaBridge Input Protocol (MBIP) repository instructions

This repository owns the generic input contract and pure adapter/normalization layer at the MonakaBridge boundary. It must remain independent of every particular input producer, physical device, vendor, manufacturer, model, transport, and runtime implementation.

## Highest-priority invariant: generic producer-independent input boundary

These constraints apply to code, tests, fixtures, configuration, scripts, packaging, and documentation. Any conforming input producer MUST be able to use the same public contract and generic adapter logic without producer-specific implementation changes.

The following five categories are **strictly prohibited**:

1. **Device-specific access:** implementing physical-device discovery, pairing, communication, control, acquisition, or recovery.
2. **Private dependencies:** importing, linking, embedding, wrapping, or reimplementing producer-specific SDKs, APIs, ABIs, drivers, private protocols, or internal packet/data models.
3. **Hard-coded device identity:** embedding physical addresses, vendor/product IDs, model-name tables, or implementation-specific identifiers as input mapping, validation, normalization, or admission policy.
4. **Identity-driven behavior:** branching on producer kind, manufacturer, model, device name, or magic values/prefixes in `source_id`, `device_id`, `publisher_id`, or `tracker_id` to choose special-case handling.
5. **Producer-specific runtime logic:** adding producer-specific packet decoding, coordinate transforms, tracking heuristics, lifecycle/reconnect control, resource lookups, or per-producer routing/registration exceptions.

**Do not enumerate concrete input producers, vendors, manufacturers, device families, or hardware models in this AGENTS.md.** Describe them only by abstract input roles and public contract terms. These restrictions cover unlisted and future implementations without exception.

Opaque device identities and descriptive metadata MAY be validated, preserved, normalized as allowed by public schemas, and transferred as data. Identity strings may be used as exact opaque keys for binding and equality checks; they MUST NOT be parsed or reinterpreted as implicit manufacturer, capability, coordinate system, or device type.

A violation is an **architecture defect and release blocker**, not an optional cleanup.

## Ownership and scope

- MBIP defines and validates the generic input-boundary contract and owns only the adapter semantics explicitly assigned to it.
- The current Phase 2C-P1 implementation is a **pure capability normalization/value adapter**, not a production receiver, loader, DLL ABI, pairing service, coordinate mapper, or device-specific runtime.
- Do NOT require all input implementations to be DLL modules or prescribe how the producer acquires or transports device observations. A future loadable-module ABI is a separate explicitly versioned design decision.
- Producer-owned responsibilities include native device access, pairing, raw packet interpretation, device-specific acquisition/recovery, and provision of truthful capability and optional device metadata.
- MonakaBridge owns input registry, logical tracker mapping, calibrated coordinate transformations, route policy, output hosting, and its management UI.
- MonakaVR owns consumer-side body assignment, Fusion/Fallback, IK, and final body-solving policy. Do not implement or duplicate these in MBIP.
- Do not move, fork, or duplicate MVIP's shared input/tracking contract into MBIP; do not invent a second capability vocabulary or independently maintained common codec.

## Source of truth and versioned contract discipline

- MVIP is the authority for public wire models, capability vocabulary and meanings, schemas, codec, validation, and version compatibility. MBIP MUST consume the exact **source/content-pinned** MVIP contract kit rather than assuming a live sibling checkout.
- For Phase 2C-P1, `docs/capability-mapping.md` and `schema/capability-mapping.json` own the deterministic MBIP mapping table and normalizer behavior. Generated dispatch tables MUST match that mapping file; do not implement ad hoc name-based dispatch.
- Static `capabilities` declare support within the admitted identity/session contract; support, sample presence, dynamic validity, freshness, and admission MUST remain distinct.
- Default STRICT behavior rejects inconsistent pose/state declarations, unauthorized identity/session changes, missing required declarations, and unknown capabilities. A legacy adaptation is permitted ONLY with an explicit provisioned/negotiated structural declaration bound to that exact identity/session and an approved migration scope. Do not infer support from sample presence, validity, packet unions, or producer names.
- Capability normalization MUST preserve input provenance, identities, timestamps, sample age, sequence, modality, coordinate/space and mapping revisions, and validity except for precisely authorized declaration normalization. Do not fabricate sensor channels or rotate identities/sessions.
- Do not invent or silently change fields, ABI/API layouts, units, coordinates, timestamps, version semantics, ownership, public symbols, or wire compatibility.
- Optional descriptive device metadata or render-resource references MAY be defined only by approved, versioned public protocol changes. MBIP MUST NOT discover private resource installations or bind asset paths based on producer identity.
- Do not add runtime network/socket listeners, producer process control, or output driver implementations to this protocol repository without a separately approved architectural change.
- Historical bootstrap instructions or task examples do not override the current normative adapter/contract documents. If the exact upstream kit, schema, or authorization is missing, stop at the dependency boundary instead of creating a substitute.

## Verification and acceptance gates

For each applicable contract/adapter change:

1. Test two synthetic input identities with identical public declarations and observations; outputs MUST follow the same generic path with no implementation-specific branches.
2. Verify the exact pinned MVIP version/manifest, canonical vocabulary and codec consistency, and generated mapping-table equality.
3. Test support/presence/validity separation, capability-set stability across temporary loss and reacquisition, missing/unknown declarations, and strict rejection cases.
4. Check that source/session/provenance, timestamp/age, sequence, coordinate frame, modality, and mapping revision stay unchanged through normalization except where an explicit versioned contract permits change.
5. Review new dependencies, APIs, fixtures, scripts, and documentation for device-specific access, hard-coded IDs, producer-specific branches, private protocol decoders, runtime control, or transport assumptions.
6. Run native/JVM build and cross-language contract tests where applicable, and verify that the adapter does not claim a new production receiver or runtime HIL readiness.

Report each build, test, compatibility check, and hardware result independently. Mark unexecuted checks `NOT RUN`. Synthetic/contract tests do not constitute physical-device or end-to-end acceptance.

## Git and task discipline

- Base active work on `dev` unless a task specifies another verified branch or commit.
- Keep changes scoped to this repository unless explicitly authorized otherwise.
- Preserve unrelated commits, files, and uncommitted work. Do not delete branches, force-push, rewrite shared history, rebase, amend unrelated commits, or reset away work.
- Keep commits small and reviewable. Record exact dependency/artifact provenance and checks performed.
- When task instructions conflict with producer-independence rules or the active contract, report and stop at the conflicting boundary instead of silently implementing an exception.
