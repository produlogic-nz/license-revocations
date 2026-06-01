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
2. If it succeeds: caches the parsed JSON locally with a timestamp,
   then enforces against the live list.
3. If it fails AND a cached copy exists: uses the cache, regardless
   of age. An offline customer can run indefinitely on cached state;
   the moment they reconnect, the live fetch refreshes and enforces
   any newer cancellations.
4. If it fails AND no usable cache exists (truly cold first launch on
   this PC): receiver starts with a fail-open warning logged to the
   issues panel (Rowan's 2026-06-02 choice).
5. Compares the receiver's own `license_id` against the `revoked` list.
   If listed → refuses to start with a plain-English error pointing at
   ProduLogic for renewal.
