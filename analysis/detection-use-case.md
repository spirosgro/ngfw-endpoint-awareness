# Detection use case: rare TLS fingerprint with endpoint context

## Purpose

Demonstrate how endpoint awareness can turn a low-confidence network observation into a more useful SOC alert.

## Trigger

The NGFW reports a TLS fingerprint that is rare in the local environment or associated with a watchlist. A fingerprint match alone does not prove malicious activity.

## Correlation

Join the firewall event to endpoint network telemetry by device or source address and a short time window. Add the initiating process, signer status, device management state, and asset criticality.

## Risk logic

- Raise priority when an unsigned or unexpected process initiated the connection.
- Raise priority when the endpoint is unmanaged or has stale security posture.
- Reduce priority when the process is signed, approved, and common for the same destination class.
- Keep the fingerprint as supporting evidence rather than a definitive application identity.

## Analyst output

The alert should include the original fingerprint, connection tuple, firewall action, process identity, signer, device, user context when permitted, confidence, and the reason for the final severity.

## Validation plan

1. Adapt field names to the deployed NGFW and endpoint connectors.
2. Build an environment-specific baseline for fingerprint and process prevalence.
3. Replay known benign software updates and administrative tools.
4. Test with authorized simulations that generate unusual TLS clients.
5. Measure alert volume, join coverage, false-positive rate, and context freshness.
6. Keep the rule in alert-only mode until the evidence supports enforcement.

