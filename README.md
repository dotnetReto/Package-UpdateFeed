# Package-UpdateFeed

Public distribution feed for encrypted runtime packages.

This repository contains only public manifests and encrypted package assets. It must never contain source code, plaintext runtime packages, passwords, private keys or signing keys.

## Preferred layout

```text
feeds/
  <opaque-package-id>/
    test/
      manifest.json
    stable/
      manifest.json
```

Package identities must be opaque. Public folder names, package IDs, release tags, asset names, release titles and notes must not disclose the product, customer, company, site or system name.

Recommended public naming:

```text
feed path : feeds/p-<random-id>/...
release   : p-<random-id>-v<version>
asset     : payload.bin
```

The actual encrypted package is stored as a GitHub Release asset. The manifest contains the public asset URL and SHA-256 hash. This keeps binary packages out of Git history while leaving the feed metadata simple and auditable.

Existing test fixtures may still keep an encrypted `package.enc` directly below a feed folder; plaintext packages are never allowed.

## Release flow

1. Build the runtime package locally.
2. Encrypt and authenticate it before anything is published.
3. Delete the temporary plaintext ZIP.
4. Upload only the encrypted payload using an opaque public identity.
5. Publish a `test/manifest.json` referencing that encrypted asset.
6. Validate the package on selected test installations, including rollback behavior.
7. Promote the exact same encrypted asset to stable by publishing `stable/manifest.json` with the same package URL and SHA-256 value.
8. Do not rebuild or re-encrypt during promotion.

Clients never downgrade when changing channels. A client running a newer test version remains on that version until stable catches up.

## Security rules

- Every project uses a separate update password.
- Passwords remain on trusted clients/private build environments and are never committed here.
- Runtime packages are authenticated and encrypted before publication.
- Current package format: PBKDF2-HMAC-SHA256, AES-256-CBC and HMAC-SHA256 (Encrypt-then-MAC).
- Clients verify the encrypted package SHA-256 value before decryption.
- Clients authenticate the encrypted payload before decryption.
- The decrypted package contains a file inventory with SHA-256 values and is parser-validated before activation.
- Updaters must preserve local configuration explicitly and support rollback.
- Public metadata must not contain identifiable product names.
- No production package should be promoted before updater validation and rollback tests pass.
