# Bitcoin Manual Seed Ceremony
Generate a BIP-39 seed phrase from physical dice rolls instead of trusting a device's TRNG. Includes printable conversion tables, a BIP39 word reference sheet, and a full offline key-ceremony procedure.

## ⚠️ Disclaimer

This project is provided for free, with no warranty of any kind. You are solely responsible for verifying every step yourself before using it to generate a seed phrase that will secure real funds. Test the entire process with a throwaway seed first. If you don't fully understand a step, stop and research it before continuing, mistakes here can mean permanent loss of funds.

## Contents

| File | Purpose |
|---|---|
| [`1-Binary-to-Decimal-Conversion-Table.pdf`](./1-Binary-to-Decimal-Conversion-Table.pdf) | Worksheet for converting dice rolls into the numbers used to pick your seed words |
| [`2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf`](./2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf) | Full BIP-39 word list indexed by decimal number (0–2047) |
| [`3-Seed-Phrase-Paper-Backup.pdf`](./3-Seed-Phrase-Paper-Backup.pdf) | Temporary paper backup form for your seed words, to be transferred to steel |
| [`Bitcoin-Single-Sig-Key-Ceremony.pdf`](./Bitcoin-Single-Sig-Key-Ceremony.pdf) | Full step-by-step procedure for a single-sig key ceremony, from preparation through funding test |

## How to Use `1-Binary-to-Decimal-Conversion-Table.pdf`

This worksheet converts physical dice rolls into the numbers used to pick your seed words from `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf`. Each of the 23 rows on the sheet corresponds to one seed word. Word 24 is **not** rolled, see the note below.

### What you need

- A printed copy of `1-Binary-to-Decimal-Conversion-Table.pdf`
- A printed copy of `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf`
- At least 6–10 dice, ideally casino-grade and from more than one manufacturer or style
- A cup or box to roll the dice fairly
- A pen
- A calculator to add up the place values (a standalone calculator, never a phone connected to the internet), or you can add them up by hand

Do this offline, away from cameras and networked devices. See the [key ceremony procedure](./Bitcoin-Single-Sig-Key-Ceremony.pdf) for full operational security guidance.

### Step 1 — Convert each dice roll to a 0 or 1

Each row has 11 columns, headed `1024 512 256 128 64 32 16 8 4 2 1`. These are binary place values — together they let you write any number from 0 to 2047, which is exactly the range of the BIP-39 word list (2048 words, indexed 0–2047).

Roll once for every column, left to right — either rolling one die 11 times, or rolling several of your dice together and reading them off in a consistent order — and convert each roll using this key:

| Die shows | Write |
|---|---|
| 1, 2, or 3 | `0` |
| 4, 5, or 6 | `1` |

Write the resulting `0` or `1` in the box under the matching column. By the time you reach the end of the row, you'll have 11 rolls converted into an 11-digit binary number.

### Step 2 — Convert the row to a decimal number

Add up only the column headings where you wrote a `1`. Ignore the columns where you wrote a `0`. The result is a number between 0 and 2047, write it in the box at the end of the row (labeled `1#`, `2#`, etc.).

**Worked example (row 1):**

| 1024 | 512 | 256 | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|------|-----|-----|-----|----|----|----|---|---|---|---|
|  1   |  0  |  1  |  0  |  1 |  0 |  1 | 0 | 1 | 0 | 1 |

`1024 + 256 + 64 + 16 + 4 + 1 = 1365`

Write **1365** in the box at the end of row 1. That's the number you'll look up in `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf` to find word 1 of your seed phrase.

### Step 3 — Repeat for rows 2 through 23

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

## Verify File Integrity (SHA-256)

Before using any of these documents for a real seed ceremony, verify that the file on your computer is byte-for-byte identical to what's published here (and here: https://www.thebitcoinrebel.com/projects/), not a corrupted download, and not a file that's been tampered with somewhere along the way.

| File | SHA-256 Checksum |
|---|---|
| `1-Binary-to-Decimal-Conversion-Table.pdf` | `19d9b9c54ff35dee3191a658c4da75d10528841f356557197301448972f96b64` |
| `2-BIP-39-Decimal-Binary-Word-Reference-Table.pdf` | `178b83d1d3e23c78de43164f77ca8da8c5ad927d14dd7e20b91b8fc6612a48c1` |
| `3-Seed-Phrase-Paper-Backup.pdf` | `f3024e45c1d91a7199f29fc0b308c3a674a406e52b5fa8295d1241ef9e838283` |
| `Bitcoin-Single-Sig-Key-Ceremony.pdf` | `135c01ff281ba090d103565db946259d4e201b5434b408179515950bcafcfcef` |

**Linux:** `sha256sum <file>`
**macOS:** `shasum -a 256 <file>`
**Windows (PowerShell):** `Get-FileHash .\<file> -Algorithm SHA256`
**Windows (Command Prompt):** `certutil -hashfile <file> SHA256`

A full walkthrough with screenshots-free step-by-step instructions for each OS is also published at [thebitcoinrebel.com/projects](https://thebitcoinrebel.com/projects).

If a checksum doesn't match, do not use the file, delete it, re-download it from this repository, and check again.