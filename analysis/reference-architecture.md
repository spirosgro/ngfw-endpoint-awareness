# Vendor-neutral reference architecture

## Objective

Give an NGFW enough current endpoint context to make a narrower policy decision while preserving the evidence required for SOC investigation.

## Components

### Endpoint or EDR sensor

Records the process responsible for a connection, executable signer, device identifier, local address, user context, and security posture. The sensor must authenticate to the collection service and protect telemetry in transit.

### Context service

Normalizes endpoint telemetry, CMDB records, and authorized discovery results. It resolves conflicts, timestamps every assertion, and exposes confidence and freshness to downstream controls.

### NGFW or policy enforcement point

Inspects traffic, derives passive features, receives endpoint context, applies policy, and emits the evidence behind allow, alert, or block decisions. NIST SP 800-207 defines a policy enforcement point as the component that enables, monitors, and terminates connections according to policy decisions.

### SIEM or XDR platform

Correlates network events with endpoint evidence, identity, asset criticality, and threat intelligence. It should preserve both direct observations and inferred attributes so analysts can assess confidence.

## Minimum event contract

| Field | Purpose |
|---|---|
| `event_time` | Correlation and freshness checks |
| `device_id` | Stable endpoint reference |
| `source_ip` and `source_port` | Network-to-endpoint join keys |
| `destination_ip` and `destination_port` | Connection scope |
| `process_name` and `process_hash` | Application attribution |
| `signer` and `signature_state` | Trust evidence |
| `tls_fingerprint` | Passive network signal |
| `observation_source` | Distinguishes direct telemetry from inference |
| `confidence` | Supports risk-based policy |
| `policy_action` | Records allow, alert, or block |

## Security and governance controls

- Authenticate and authorize every telemetry producer.
- Protect integrity and confidentiality in transit and at rest.
- Set explicit retention periods for process, user, and connection metadata.
- Minimize personal data and restrict analyst access by role.
- Monitor clock skew because time alignment controls correlation quality.
- Define fallback behavior when endpoint context is absent or stale.

