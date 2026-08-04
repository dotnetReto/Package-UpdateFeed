# Package-UpdateFeed

Public distribution feed for encrypted runtime packages.

This repository contains only encrypted package assets and public manifests. It must never contain source code, plaintext runtime packages, passwords, private keys or signing keys.

## Layout

```text
feeds/
  <package-id>/
    test/
      manifest.json
      package.enc
    stable/
      manifest.json
      package.enc
```

Package IDs should be neutral and should not disclose customer, company or system names unnecessarily.

## Release flow

1. Publish a new encrypted package to `test`.
2. Validate it on selected test installations.
3. Promote the exact same `package.enc` to `stable`.
4. Verify that the SHA-256 value is unchanged.
5. Change only the stable manifest channel, publication time and package URL.

Clients never downgrade when changing channels. A client running a newer test version remains on that version until stable catches up.

## Security rules

- Every project uses a separate update password.
- Passwords remain in private calling scripts and are never committed here.
- Runtime packages are authenticated and encrypted before publication.
- Clients verify the encrypted package SHA-256 value.
- The decrypted package contains a file inventory with SHA-256 values.
- No production package should be published before updater validation and rollback tests pass.

No production package is published yet.
