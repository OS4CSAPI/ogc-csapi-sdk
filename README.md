# OGC CSAPI SDK

**Sensor System Integration Toolkit for OGC API – Connected Systems**

> **Status:** v0.1 draft. See [`docs/plans/v0.1-plan.md`](docs/plans/v0.1-plan.md)
> for what is in scope, what is deferred, and the audit-grounded evidence base.

The OGC CSAPI SDK helps integrators take an existing sensor system —
sensors, platforms, devices, actuators, or controllable systems with their
own data feeds — and expose it through OGC API – Connected Systems for
discovery, observation access, validation, and visual inspection.

The SDK is designed around both directions of CSAPI interaction:

- **Outbound (v0.1):** publishing system metadata, deployments, procedures,
  sampling features, datastreams, observations, and system events.
- **Inbound (v0.2 backlog):** accepting command feasibility requests,
  commands, command status updates, and command results through CSAPI
  control resources.

The v0.1 goal is narrow and concrete: enable a technical team to map one
existing sensor data source to CSAPI, publish observations through a
CSAPI server, validate the endpoint against the OGC ETS, and inspect the
result visually — in one afternoon.

This is an integration toolkit, not a language-binding SDK. Where mature
CSAPI client libraries already exist (`OS4CSAPI/ogc-client` for
TypeScript, `geopython/OWSLib` for Python), the SDK uses and documents
them rather than reimplementing them.

## Repository layout

- [`docs/plans/`](docs/plans/) — release plans (start with [`v0.1-plan.md`](docs/plans/v0.1-plan.md))
- [`docs/research/`](docs/research/) — primary-source ecosystem audit and review notes
- [`docs/governance/`](docs/governance/) — charter, decision log, license policy (forthcoming)

## License

Apache License 2.0 — see [`LICENSE`](LICENSE).
