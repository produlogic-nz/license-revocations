# ProduLogic License Revocations

Public list of revoked ProduLogic Swift Transfer Receiver license IDs.

This repo's `revocations.json` is served by GitHub Pages at:

> **https://produlogic-nz.github.io/license-revocations/revocations.json**

Receivers fetch this URL on startup. If their `license_id` appears in
the `revoked` array, the receiver refuses to start.

## Safe to be public

Each entry is an opaque UUID — there's no customer name, no email, no
machine identifier, no expiry, nothing. Just a list of license IDs
the owner of the license-key-creator has chosen to revoke.

## Format

```json
{
  "version": 1,
  "updated_at": "2026-06-02T10:30:00",
  "revoked": [
    "uuid-1",
    "uuid-2"
  ]
}
```

## Who updates it

Rowan, via the License Key Creator GUI. Clicking Cancel on a license
in that GUI commits an append to this file and pushes to main.

## How receivers consume it

On startup, the receiver:

1. HTTP-GETs the URL above.
2. If it succeeds: caches the parsed JSON locally with a timestamp.
3. If it fails AND a cached copy < 7 days old exists: uses the cache.
4. If it fails AND no usable cache: receiver still starts but shows
   a "couldn't check revocations" banner (fail-open chosen 2026-06-02).
5. Compares the receiver's own `license_id` against the `revoked` list.
   If listed → refuses to start with a plain-English error pointing at
   ProduLogic for renewal.
