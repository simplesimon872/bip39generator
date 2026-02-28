# Secure BIP-39 Mnemonic Generator

A single, self-contained HTML file that generates a valid 12-word BIP-39 recovery phrase entirely in your browser. No server, no dependencies, no network requests.

Compatible with BTC, ETH, SOL and any wallet that uses the BIP-39 standard.

**[Try it here](https://simplesimon872.github.io/bip39generator/)** — or download and run it offline (recommended).

---

## Why does this exist?

Most online seed phrase generators require you to trust that the site is doing what it claims, that it hasn't been modified, and that it isn't sending your phrase anywhere. That's a lot of trust for something that controls access to your funds.

This tool is designed to be the opposite of that:

- The entire source is a single HTML file you can read in any text editor
- It makes zero network requests — you can verify this in your browser's DevTools (Network tab)
- It uses your browser's built-in CSPRNG (`window.crypto.getRandomValues`) for entropy — the same API used by password managers
- The SHA-256 checksum is computed by a self-contained pure-JS implementation, so the tool works correctly when opened from a local file on any browser without needing an internet connection
- The published SHA-256 hash of the file lets you verify you have an unmodified copy

The safest way to use it is to download the file, disconnect from the internet, open it locally, generate your phrase, write it down, then close the file.

---

## How it works

Follows the [BIP-39 specification](https://github.com/trezor/python-mnemonic/blob/master/src/mnemonic/wordlist/english.txt) exactly:

1. Generate 128 bits of cryptographically secure random entropy via `window.crypto.getRandomValues()`
2. Compute SHA-256 of the entropy bytes
3. Take the first 4 bits of the hash as a checksum (128 ÷ 32 = 4 bits)
4. Append the checksum to the entropy to get 132 bits
5. Split into 12 chunks of 11 bits each
6. Map each 11-bit value (0–2047) to the standard BIP-39 English wordlist
7. The result is a valid 12-word mnemonic with a correct checksum, compatible with any BIP-39 wallet

---

## Verify the file

The SHA-256 hash of the current release is:

```
SHA-256: EB49F7FF174FF34640F94A6045A81C4926E373D61A4B3EDE1A2D71976B9906F6
```

You can verify the file you downloaded or the hosted version matches this hash exactly.

**Windows (PowerShell):**
```powershell
# verify a local file
Get-FileHash index.html -Algorithm SHA256

# or download and verify the hosted version
Invoke-WebRequest https://simplesimon872.github.io/bip39generator/ -OutFile index.html
Get-FileHash index.html -Algorithm SHA256
```

**Mac / Linux:**
```bash
# verify a local file
sha256sum index.html

# or download and verify the hosted version
curl -O https://simplesimon872.github.io/bip39generator/
sha256sum index.html
```

Compare the output to the hash above. If they match, the file is identical to the one published in this repo and has not been modified in transit or on the server.

The hash is also published in [`checksums.txt`](./checksums.txt) in this repo as a second reference point.

---

## Download and run offline (recommended)

1. Download `index.html` from the [latest release](https://simplesimon872.github.io/bip39generator/releases/latest)
2. Verify the SHA-256 hash (see above)
3. Move the file to an air-gapped machine or disconnect from the internet
4. Open the file in any browser — it needs no internet connection to work
5. Generate your phrase, write it down on paper
6. Close the browser and clear any history if desired

---

## Security notes

- **Write your phrase down on paper.** Do not store it in a text file, screenshot, cloud service, password manager, or anywhere digital.
- **Never enter a real seed phrase into any website**, including this one. This tool only generates new phrases — it has no input field.
- The "Copy to Clipboard" button is provided for convenience when testing. For real wallet setup, write the words down manually and do not copy them.
- This tool generates the mnemonic only. Deriving wallet addresses from a mnemonic involves additional steps (PBKDF2, BIP-32/44) that are outside the scope of this tool.
- 12-word phrases provide 128 bits of entropy, which is considered secure for all practical purposes. If you require 24-word phrases (256 bits), this tool does not currently support that.

---

## Verifying the source yourself

You don't need to trust this README. The entire tool is ~150 lines of readable HTML, CSS and JavaScript with no minification, no obfuscation and no external resources. Open `index.html` in a text editor and read it. Things to check:

- Entropy comes only from `window.crypto.getRandomValues()` — search the file for `Math.random` and you should find nothing
- There are no `fetch()`, `XMLHttpRequest`, `WebSocket` or `<script src="">` calls — the file loads nothing from the network
- The wordlist is the standard BIP-39 English list — you can cross-reference it against the [official list](https://github.com/trezor/python-mnemonic/blob/master/src/mnemonic/wordlist/english.txt)

---

## Releases and versioning

Each release has a version tag (e.g. `v1.0`) with the `index.html` attached as a release asset and the SHA-256 hash in the release notes. Once published, release assets on GitHub are immutable — the file attached to `v1.0` will always be the same file.

If the tool is ever updated, a new release will be created with a new version tag and a new hash. Old releases remain available and verifiable.

---

## Credits

Created by [@SimpleSimon872](https://arena.social/SimpleSimon872)

SHA-256 implementation based on the well-known pure-JS version by [Chris Veness](https://gist.github.com/chrisveness/e5a07769d06ed02a2587cc1f3907c70e) (MIT licence), included inline to remove any dependency on `crypto.subtle` which requires a secure context and would fail when opening the file locally in Chrome or Edge.

---

## Licence

MIT — do what you want with it, just don't remove the attribution.
