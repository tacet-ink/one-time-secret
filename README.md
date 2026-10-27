# one-time-secret

One-time secrets for [默·Tacet](https://tacet.ink) (Tacet): a read-once,
zero-knowledge secret sharing page, published here for verification.
This is the first page of the one-time-secret family (once secret);
the family name is chosen so the second page, *request a secret*
(`/sreq`, not yet published), fits under the same roof.

## What is in this repo

| File | What it is |
| --- | --- |
| `secret.html` | The page itself, byte-identical to the deployed `tacet.ink/secret`. Single file, all CSS and JS inline. |
| `SHA256SUMS` | SHA-256 manifest for the distributed files, in standard `sha256sum` format. |
| `LICENSE` | MIT. |

`secret.html` is an **as-deployed copy**: it was ingested from the
production tree with zero modification, so the SHA-256 recorded here is
the hash of what actually ships.

## How it works

- **Read-once.** A secret link is a one-time voucher: the first
  retrieval burns the secret. Opening the link again (or a second
  copy of it) cannot recover it.
- **Zero-knowledge server.** The secret is encrypted in the browser
  (AES-GCM-256, WebCrypto) before it goes anywhere. The server stores
  ciphertext and link metadata only; it never sees plaintext.
- **The link alone is not enough.** The decryption key lives in the
  URL fragment (the `#` part), which browsers never send to the
  server. A person holding only the link cannot open the secret
  without the key that accompanied it.

Live instance: [tacet.ink/secret](https://tacet.ink/secret).

## Verification

The manifest covers the distributed files with standard two-column
`sha256sum` format:

```sh
sha256sum -c SHA256SUMS
```

To verify the file as GitHub actually serves it:

```sh
curl -fsSL https://raw.githubusercontent.com/tacet-ink/one-time-secret/main/secret.html \
  | sha256sum
```

and compare the value against the `secret.html` line in `SHA256SUMS`.

**Honest boundary.** This is a top-level page: its inline scripts are
part of the page itself, and the page ships **no SRI** (subresource
integrity) on any external subresource. The SHA-256 manifest therefore
verifies the page bytes as distributed; it does not verify anything
beyond them.

## Roadmap note

The *request a secret* page (`/sreq`) is planned and will be added to
this repo when it ships. The cryptographic primitives the family is
built on live in the public
[journal-core](https://github.com/tacet-ink/journal-core) repository.

## License

MIT. See [LICENSE](./LICENSE).
