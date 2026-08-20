# Metering

Tracks request, token, time, rate, and concurrency consumption and produces verifiable per-invocation usage receipts.

Requirements:

- counts are associated with stable invocation and lease IDs;
- concurrent quota reservation and finalization are atomic;
- retransmitted terminal events or receipts do not double-charge;
- completed, cancelled, failed, and incomplete streams remain distinguishable;
- upstream-reported, locally measured, estimated, and unavailable usage are not conflated;
- receipt persistence and quota state survive daemon restart;
- peers can compare receipts without interpreting them as proof of quality or payment.

Financial settlement and disputes are outside this package.
