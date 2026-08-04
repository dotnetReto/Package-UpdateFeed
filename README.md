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

## Channel workflow

1. Publish a new encrypted package and manifest to `test`.
2. Validate the package on selected test installations.
3. Promote the exact same `package.enc` bytes and SHA-256 value to `stable`.
4. Create the stable manifest with `Channel` set to `stable` and the stable package URL.

The encrypted package itself is channel-neutral. Only the manifest identifies whether it belongs to the `test` or `stable` feed.

Clients never downgrade automatically. A client that switches from test version `2.5.0` to a stable feed still at `2.4.0` remains on `2.5.0` until stable catches up or publishes a higher version.

## Security notes

- Every package is encrypted before publication.
- Every project uses a separate update password.
- The update password remains in the private calling script and is never committed here.
- Clients verify the encrypted package SHA-256 value from the manifest.
- Manifest signing will be added before production use.

No production package is published yet.
