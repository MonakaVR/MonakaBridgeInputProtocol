# MonakaBridge Input Protocol (MBIP)

Standard input protocol for [MonakaBridge](https://github.com/MonakaVR/MonakaBridge).

MBIP defines the contract used by input modules that provide tracker observations to MonakaBridge. It is intended to keep device-specific backends and MonakaBridge decoupled while giving MonakaBridge one stable input boundary.

## Scope

This repository owns the MonakaBridge input contract, including:

- input-facing data structures and ABI/API contracts;
- protocol/version compatibility rules;
- capability and tracker metadata required at the input boundary;
- validation rules, fixtures, and conformance material for MBIP implementations.

The concrete ABI, field set, and versioning policy are intentionally not frozen by this initial repository bootstrap.

## Out of scope

This repository does **not** own:

- PICO, VIVE, Slime-class IMU, or other device-specific access;
- pairing, transport discovery, or vendor runtime control;
- MonakaBridge mapping, calibration, routing, or UI;
- Fusion/Fallback, IK, or body-role solving;
- SteamVR or other output implementations;
- the MonakaVR-side input protocol.

Those responsibilities belong to their respective backend, MonakaBridge, MonakaVR, or output-module layers.

## Architecture

```text
Device / Backend
      |
      v
MBIP input module
      |
      v
MonakaBridge
      |
      +--> Output module(s)
      |
      +--> MonakaVR path
```

MBIP is the input boundary **into MonakaBridge**. It should not accumulate downstream runtime responsibilities.

## Repository status

Bootstrap stage.

The first implementation work should define the minimum contract required by MonakaBridge and its input modules, then lock down ABI/version semantics with tests and fixtures before broadening the surface.

## Branches

- `main` — stable repository baseline
- `dev` — integration branch for active development
