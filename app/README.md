# Encrypted content feed

The trip app reads its itinerary from here. Everything except `manifest.json` is ciphertext.

AES-256-GCM. The key is derived from **two** factors that only exist together on a phone
belonging to someone on the trip:

1. a 256-bit master key compiled into the iOS build, and
2. a 12-character access code, texted to each person and typed once.

Neither opens anything alone, which is why this is safe to serve from a public site.

`manifest.json` is deliberately in the clear — it carries only file names, byte counts,
SHA-256 hashes of the ciphertext, and the KDF salt. A salt is not a secret, and the app has to
read something before it can decide what to fetch.

Do not commit plaintext here, ever. It is generated in `Japan Docs/_app/`, validated and
secret-scanned there, and only the encrypted output is published.
