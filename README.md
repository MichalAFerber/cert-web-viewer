# Cert Viewer

A fast, mobile-first, **single-file** X.509 certificate & CSR viewer. Drag & drop
a `.pem`, `.crt`, `.cer`, `.der`, or `.csr` and read it decoded — **subject,
issuer, SAN, validity, key usage, and fingerprints**. It's a **viewer** — no
accounts, no uploads. Everything runs locally in your browser.

🔗 **Live:** <https://cert-viewer.us/>

![Single file](https://img.shields.io/badge/build-single%20HTML%20file-success) ![No build step](https://img.shields.io/badge/build%20step-none-success) ![License](https://img.shields.io/badge/license-MIT-blue)

> Part of the **[File Viewer](https://file-viewer.us/) family** — HTML, Markdown,
> ePUB, PDF, Data, DOCX, Sheets, EML, PPTX, Log, and Cert each have their own
> dedicated viewer. Use the **☰ menu** in the header to jump between them.

## Features

- 🪪 **Full X.509 decode** — subject & issuer distinguished names, version, serial,
  signature algorithm, and public-key algorithm + size (RSA bits / EC curve).
- ⏱️ **Validity at a glance** — a **VALID / EXPIRED / NOT YET VALID** badge with
  days remaining, plus the not-before / not-after timestamps.
- 🌐 **Subject Alternative Names** — DNS, IP, email, and URI entries listed out.
- 🔑 **Key usage & extended key usage** — decoded to readable names
  (serverAuth, clientAuth, codeSigning…), plus basic constraints (CA / path len).
- 🔒 **Fingerprints** — **SHA-1** and **SHA-256** of the certificate, computed
  locally with the Web Crypto API.
- 📝 **CSRs too** — PKCS#10 requests (`.csr`) show subject, requested SAN, and key.
- ☰ **Family menu** · 🫥 **auto-hiding header** · 🎨 **pick any background color**.
- 🪶 **One file, no build, no dependencies** — works offline, even from `file://`.
- 📊 **Privacy-friendly analytics** — self-hosted, cookieless
  [Plausible](https://plausible.io/); your certificates never leave your device.

## Supported file types

`.pem` `.crt` `.cer` `.der` `.csr` `.cert` `.p7b`

PKCS#12 bundles (`.p12` / `.pfx`) are password-protected key stores — export the
certificate to PEM/DER first (`openssl pkcs12 -in file.p12 -clcerts -nokeys -out cert.pem`).

## Quick start

**Just open it.** Download [`index.html`](index.html), double-click it — no
server, no build, no internet needed.

```sh
python3 -m http.server 8080   # then open http://localhost:8080
```

## Deploy to Cloudflare Pages

Connect the repo (**Workers & Pages → Create → Pages → Connect to Git**),
framework preset **None**, build command blank, output directory `/`. The
[`_headers`](_headers) file applies a strict CSP automatically. Add the custom
domain **cert-viewer.us** under the project's Custom domains tab.

## How it works

Everything is in [`index.html`](index.html) — **no third-party libraries.** A
compact **ASN.1 DER parser** (written for this project) walks the certificate
structure per RFC 5280 and decodes OIDs, distinguished names, validity times,
the public-key info, and extensions (SAN, key usage, EKU, basic constraints, key
identifiers). Fingerprints use the built-in **Web Crypto API**
(`crypto.subtle.digest`). PEM files are base64-decoded to DER first. The viewer's
CSP stays strict (`default-src 'none'`, no external assets).

**Note:** this is a decoder, **not a validator** — it does not build or verify
the trust chain, check revocation (CRL/OCSP), or validate the signature. It
reads what's in the file.

## Credits

No bundled libraries — the ASN.1/X.509 decoder is original to this project;
fingerprints use the browser's Web Crypto API. The shield icon is by the author.
Headings use the [JetBrains Mono](https://github.com/JetBrains/JetBrainsMono)
typeface (SIL OFL-1.1). Analytics by [Plausible](https://plausible.io/).

## License

[MIT](LICENSE) © 2026 Michal Ferber, aka **TechGuyWithABeard**.
