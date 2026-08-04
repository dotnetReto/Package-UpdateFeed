# Package-UpdateFeed

Public distribution feed for encrypted runtime packages.

This repository contains only encrypted package assets and public manifests. It must never contain source code, plaintext runtime packages, passwords, private keys or signing keys.

## Layout

```text
feeds/
  <package-id>/
    stable/
      manifest.json
      package.enc
```

Package IDs should be neutral and should not disclose customer, company or system names unnecessarily.

## Security notes

- Every package is encrypted before publication.
- Every project uses a separate update password.
- The update password remains in the private calling script and is never committed here.
- Clients verify the encrypted package SHA-256 value from the manifest.
- Manifest signing will be added before production use.

No production package is published yet.
