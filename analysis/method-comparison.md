# Endpoint identification method comparison

The categories below follow the taxonomy in Heino, Hakkala, and Virtanen. The operational assessment is a portfolio interpretation for security architecture and SOC use.

| Method | Signal source | Coverage | Main strength | Main limitation | Suitable operational use |
|---|---|---:|---|---|---|
| HTTP metadata | User-Agent and application-layer fields | Narrow | Low processing cost | Spoofable and reduced by encryption | Supporting context for visible HTTP traffic |
| TLS fingerprinting | ClientHello or ServerHello features, including JA3/JA3S-style fingerprints | Broad for observed TLS | Works without an endpoint agent | Shared libraries, collisions, evasion, and protocol change | Threat hunting, traffic clustering, and weak identity evidence |
| Flow classification | Packet size, timing, burst, and session features | Broad | Can operate without payload decryption | Often requires full-flow processing and model maintenance | Retrospective classification and anomaly enrichment |
| Active scanning | Responses to controlled probes | Primarily servers | Identifies exposed services and versions | Creates traffic, ages quickly, and needs authorization | Asset discovery and vulnerability context |
| CMDB integration | Managed asset inventory | Managed assets | Adds ownership and expected-software context | Records can be incomplete or stale | Policy scoping and alert enrichment |
| Endpoint telemetry | Agent or EDR process and connection metadata | Managed endpoints | Strong connection-to-process attribution | Requires deployment, trust, secure transport, and governance | High-confidence correlation and adaptive policy |

## Design conclusion

A useful control plane combines broad passive visibility with stronger but narrower active evidence. Every observation should retain its source, collection time, and confidence. The firewall can enforce high-confidence policy decisions while sending ambiguous cases to the SIEM for correlation and analyst review.

## Interpretation rules

- Treat a fingerprint as an indicator, not proof of a specific application.
- Expire context when its age exceeds the operational tolerance of the policy.
- Distinguish directly observed endpoint metadata from inferred identity.
- Preserve the raw network and endpoint evidence used to calculate confidence.
- Test false positives before moving a correlation rule from alerting to blocking.

