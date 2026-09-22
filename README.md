# Bitcoin Manual Seed Ceremony
Generate a BIP-39 seed phrase from physical dice rolls instead of trusting a device's TRNG. Includes printable conversion tables, a BIP39 word reference sheet, and a full offline key-ceremony procedure.

## ⚠️ Disclaimer

This project is provided for free, with no warranty of any kind. You are solely responsible for verifying every step yourself before using it to generate a seed phrase that will secure real funds. Test the entire process with a throwaway seed first. If you don't fully understand a step, stop and research it before continuing. Mistakes here can mean permanent loss of funds.

## Need Help?

If you'd like specialized, one-on-one consulting on your setup, you can contact us through [thebitcoinrebel.com](https://thebitcoinrebel.com).

## Acknowledgements

This project builds on the work of others who researched and published on dice-based seed generation long before this repository existed. Thank you to:

- **Valerio Vaccaro**: [TRMG](https://valerio-vaccaro.github.io/TRMG/), a free, open-source tool for converting dice rolls into BIP-39 entropy.
- **SeedSigner**: the open-source hardware wallet project, whose [dice verification documentation](https://github.com/SeedSigner/seedsigner/blob/dev/docs/dice_verification.md) explains the underlying math and security considerations of dice-based entropy in detail.
- **Arman The Parman"**: his [guide to generating a Bitcoin seed with dice](https://armantheparman.com/bitcoin-seed-with-dice/), which this project's dice-to-bit conversion method (faces 1–3 → 0, faces 4–6 → 1) is based on.

Without this prior open-source and educational work, this project would not have been possible.

## Contents

| File | Purpose |
|---|---|
| [`1-Binary-to-Decimal-Conversion-Table.pdf`](./1-Binary-to-Decimal-Conversion-Table.pdf) | Worksheet for converting dice rolls into the numbers used to pick your seed words |
| [`2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf`](./2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf) | Full BIP-39 word list indexed by decimal number (0–2047) |
| [`3-Seed-Phrase-Paper-Backup.pdf`](./3-Seed-Phrase-Paper-Backup.pdf) | Temporary paper backup form for your seed words, to be transferred to steel |
| [`Bitcoin-Single-Sig-Key-Ceremony.pdf`](./Bitcoin-Single-Sig-Key-Ceremony.pdf) | Full step-by-step procedure for a single-sig key ceremony, from preparation through funding test |

## How to Use `1-Binary-to-Decimal-Conversion-Table.pdf`

This worksheet converts physical dice rolls into the numbers used to pick your seed words from `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf`. Each of the 23 rows on the sheet corresponds to one seed word. Word 24 is **not** rolled. See the note below.

### What you need

- A printed copy of `1-Binary-to-Decimal-Conversion-Table.pdf`
- A printed copy of `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf`
- At least 6–10 dice, ideally casino-grade and from more than one manufacturer or style
- A cup or box to roll the dice fairly
- A pen
- A calculator to add up the place values (a standalone calculator, never a phone connected to the internet), or you can add them up by hand

Do this offline, away from cameras and networked devices. See the [key ceremony procedure](./Bitcoin-Single-Sig-Key-Ceremony.pdf) for full operational security guidance.

### Step 1: Convert each dice roll to a 0 or 1

Each row has 11 columns, headed `1024 512 256 128 64 32 16 8 4 2 1`. These are binary place values, together they let you write any number from 0 to 2047, which is exactly the range of the BIP-39 word list (2048 words, indexed 0–2047).

Roll once for every column, left to right, either rolling one die 11 times, or rolling several of your dice together and reading them off in a consistent order, and convert each roll using this key:

| Die shows | Write |
|---|---|
| 1, 2, or 3 | `0` |
| 4, 5, or 6 | `1` |

Write the resulting `0` or `1` in the box under the matching column. By the time you reach the end of the row, you'll have 11 rolls converted into an 11-digit binary number.

### Step 2: Convert the row to a decimal number

Add up only the column headings where you wrote a `1`. Ignore the columns where you wrote a `0`. The result is a number between 0 and 2047, write it in the box at the end of the row (labeled `1#`, `2#`, etc.).

**Worked example (row 1):**

| 1024 | 512 | 256 | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|------|-----|-----|-----|----|----|----|---|---|---|---|
|  1   |  0  |  1  |  0  |  1 |  0 |  1 | 0 | 1 | 0 | 1 |

`1024 + 256 + 64 + 16 + 4 + 1 = 1365`

Write **1365** in the box at the end of row 1. That's the number you'll look up in `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf` to find word 1 of your seed phrase.

In this example, decimal `1365` → binary `10101010101` → the word **primary**.

### Step 3: Repeat for rows 2 through 23

Do the same for every remaining row: 11 fresh dice rolls per row, converted to a binary number, then added up into a single decimal number from 0–2047. Each row gives you the number for the corresponding seed word, row 2 gives you word 2, and so on through row 23.

Write each word directly onto your [seed phrase backup sheet](./3-Seed-Phrase-Paper-Backup.pdf) (or steel backup) as you go, rather than waiting until the end.

### ⚠️ Critical: the word list is 0-indexed, not 1-indexed

The number you calculate (0–2047) is a direct index into the official BIP-39 word list, and that list starts counting at **0**, not 1.

If you look at the list directly on GitHub ([bitcoin/bips — bip-0039/english.txt](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt)), the first line (`abandon`) is displayed as **line 1** by GitHub's line-number viewer, but its correct BIP-39 index is **0**. This is just how GitHub numbers text files, it is not part of the standard.

`2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf` is built with this offset already accounted for, so as long as you use that table (not GitHub's line numbers) to look up your number, you'll get the correct word. This warning exists so that if you ever cross-check against the raw GitHub file, you don't get confused by the apparent mismatch and pick the wrong word.

### Word 24: not rolled

Word 24 is not generated with dice. It contains the mandatory BIP-39 checksum bits, which can only be calculated mathematically from words 1–23, it isn't a free choice like the others. Once you've entered words 1–23 into a compatible hardware wallet (e.g. SeedSigner, Jade+, Foundation Passport, ColdCard), the device calculates and displays word 24 for you.

### Why 1s, 2s, and 3s all mean "0"

This sheet uses a fixed, unbiased mapping (dice faces 1–3 → `0`, faces 4–6 → `1`) rather than assigning individual faces to specific bit positions. This averages out any small manufacturing bias in a given die across three faces instead of resting on a single face, which is the more bias-resistant of the two common dice-to-bit methods. Using several dice from different manufacturers or styles, rolled in a cup or box rather than by hand, further reduces the chance that any single die's bias skews your results, no realistic manufacturing bias in an ordinary die comes close to affecting the security of the resulting seed, but these habits cost nothing and remove any doubt.

## How to Use `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf`

This is the lookup table for turning the numbers you calculated with `1-Binary-to-Decimal-Conversion-Table.pdf` into actual seed words.

Each of the 23 pages lists two columns of entries, sorted from 0 to 2047. Every row shows three things: the **decimal number** (0–2047), the same value written out as an **11-bit binary number**, and the **word** it corresponds to in the official BIP-39 word list.

To use it: take the decimal number you calculated for a given row on the dice worksheet, find that same number in the "Decimal" column here, and the word next to it is your seed word for that position. The binary column is there so you can also cross-check your handwritten binary digits directly against the table, if you want a second way to catch a mistake.

Remember the list starts at **0** (`abandon`), not 1.

## How to Use `3-Seed-Phrase-Paper-Backup.pdf`

This form is where you record your seed phrase and wallet details right after generating them **on paper, offline, as a temporary backup only.** It is not a substitute for a steel backup; it exists to bridge the gap between generating your seed and engraving it onto steel.

### Before you start

Do this offline, away from cameras, microphones, and any networked device.

### Step 1: Multisig checkbox

At the top of page 1, check whether this seed is part of a multisig setup. If yes, check "Yes" and fill in the quorum (e.g. "2 of 3"). If this is a single-sig wallet, check "No".

If you're setting up a multisig wallet, it's also essential to save the **wallet descriptor** (or a full export of your multisig configuration from your coordinator, e.g. Sparrow). Without it, having all the individual seeds is not enough to restore your multisig wallet, the descriptor records exactly how those seeds combine (which xpubs, in what order, with what quorum and script type) and is required to reconstruct the wallet correctly.

### Step 2: Write down the 24 seed words

In the "Seed Words" grid, write words #1–24 exactly as they are numbered and exactly as your device displays them.

- Write **one word at a time**, copying it directly from its source, either the BIP-39 word reference table as you look up each word during the dice-to-word conversion, or your device's screen once you've entered the seed into it. Never from memory, and never by dictating it to someone else.
- After filling in all 24, go back and read each word on the page against your device's screen a second time, word by word, to catch any transcription mistakes.
- Word #24 always comes from your device, never from the reference table: it's the checksum word your hardware wallet calculates for you.

### Step 3: Wallet details

Fill in the panel below the seed words:

| Field | What to write |
|---|---|
| Master fingerprint | The 8-character hex ID your wallet displays for this seed (not secret, safe to write in plain text) |
| Derivation path | The derivation path shown by your wallet software (e.g. `m/84'/0'/0'`) |
| Script type | The address type this wallet uses (e.g. Native SegWit) |
| H.W or software wallet used | The device/app you used (e.g. SeedSigner, Sparrow) |
| Firmware version | The firmware version running on your hardware wallet at the time |
| Device serial no. | Your device's serial number, if it has one |

None of these fields reveal your seed on their own, but keep the whole page as confidential as the seed itself, since it identifies which wallet the seed unlocks.

Once this panel is filled in, also copy these same wallet detail fields: master fingerprint, derivation path, script type, wallet/device used, firmware version, device serial no., into your password manager, along with the **xpub**, **zpub**, and **first receiving address** from your watch-only wallet setup (e.g. Sparrow). **Do NOT enter the seed phrase itself into the password manager under any circumstances.** None of these additional values can move or spend funds on their own, they only let you identify the wallet, generate receiving addresses, and confirm you're looking at the right wallet later; the seed phrase must exist only on paper/steel, never digitally.

### Step 4: Notes on seed generation

Check the box that matches how this seed's entropy was actually generated: the hardware wallet's own random number generator (TRNG), or user-supplied entropy such as dice rolls.

### Step 5: SeedQR (page 2)

Page 2 holds a **SeedQR**: a QR-code encoding of your seed phrase, used to re-import your seed quickly into a compatible air-gapped wallet without typing all 24 words by hand.

The SeedQR format was created by the open-source [SeedSigner](https://github.com/SeedSigner/seedsigner) project.
See their [SeedQR documentation](https://github.com/SeedSigner/seedsigner/blob/dev/docs/seed_qr/README.md) for the full technical spec.

The blank QR grid used on this form is adapted from Blockstream's [CompactSeedQR template](https://storage.googleapis.com/dxp-production-assets/content/blockstream-jade/use-jade-air-gapped/create-a-seedqr-from-my-recovery-phrase/CompactSeedQRTemplate-new.pdf), published as part of their Jade air-gapped wallet documentation.

- Generate the SeedQR on your air-gapped hardware wallet (e.g. Jade+ or SeedSigner's "Seed QR" export feature).
- Hand-copy the black/white grid square by square from your device's screen onto the grid provided, checking your work square by square as you go.
- Write the master fingerprint in the box under the QR as well, so this page can be identified even if separated from page 1.
- **Never scan this QR with any device connected to the internet.** Only scan it with the same type of dedicated, offline hardware wallet used to generate it, and only when you actually need to restore from it.

### Step 6: Transfer to steel

Once you've verified every word and the wallet details are correct:

1. Copy everything: words, wallet details, and SeedQR, onto a fireproof, waterproof steel backup.
2. Verify the steel backup against this paper copy, word by word. Beyond this visual check, also perform the full dry-run recovery described in the [key ceremony procedure](./Bitcoin-Single-Sig-Key-Ceremony.pdf), restoring the seed from the steel backup into your device, to confirm the steel backup actually works, not just that it matches on paper.
3. Since the wallet details (master fingerprint, derivation path, script type, xpub, zpub, first address, etc.) also already live in your password manager from Step 3, you don't strictly need to keep this paper form around afterward for that information, the only thing that must survive is the seed phrase itself on steel. If you'd still rather keep this paper as a secondary reference, that's fine, but getting a copy onto steel is the non-negotiable part.

### Never digitize this page

Do not photograph, scan, or type any of these words, or the SeedQR, into any phone, computer, or cloud service, at any point, for any reason. A single photo of this page on a networked device can expose your entire wallet.

## How to Use `Bitcoin-Single-Sig-Key-Ceremony.pdf`

This is the master procedure that ties the other three documents together. It walks through the entire process end to end, gathering materials, a rehearsal with a throwaway test seed, generating real entropy with the dice worksheet, entering it into your hardware wallet, recording the backup, transferring to steel, and a small-amount funding test with dry-run recovery, followed by a Ceremony Record to fill in once you're done.

**To use it:** print it out and follow it phase by phase, in order, checking off each box as you complete it. Don't skip Phase 2 (Rehearsal), running through the whole workflow once with a disposable test seed is what catches mistakes before they matter. Have the other three documents (dice worksheet, word reference table, seed backup form) on hand, since this procedure references them directly at the relevant steps.

The Ceremony Record section at the end contains no seed material, only dates, wallet parameters (fingerprint, derivation path, script type), and a verification checklist, so it's safe to file with your other administrative records, separate from your actual seed backups.

## Verify File Integrity (SHA-256)

Before using any of these documents for a real seed ceremony, verify that the file on your computer is byte-for-byte identical to what's published here (and here: https://www.thebitcoinrebel.com/projects/), not a corrupted download, and not a file that's been tampered with somewhere along the way.

| File | SHA-256 Checksum |
|---|---|
| `1-Binary-to-Decimal-Conversion-Table.pdf` | `19d9b9c54ff35dee3191a658c4da75d10528841f356557197301448972f96b64` |
| `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf` | `178b83d1d3e23c78de43164f77ca8da8c5ad927d14dd7e20b91b8fc6612a48c1` |
| `3-Seed-Phrase-Paper-Backup.pdf` | `94f4fa143cd96becfb8b7d68157337a0d0669aa49322c4aba2e397f6cd0f80f5` |
| `Bitcoin-Single-Sig-Key-Ceremony.pdf` | `135c01ff281ba090d103565db946259d4e201b5434b408179515950bcafcfcef` |

**Linux:** `sha256sum <file>`
**macOS:** `shasum -a 256 <file>`
**Windows (PowerShell):** `Get-FileHash .\<file> -Algorithm SHA256`
**Windows (Command Prompt):** `certutil -hashfile <file> SHA256`

These same checksums are also published at [thebitcoinrebel.com/projects](https://www.thebitcoinrebel.com/projects/) as an independent reference. Check them there too, before trusting the values in this repo.

If a checksum doesn't match, do not use the file, delete it, re-download it from this repository, and check again.

