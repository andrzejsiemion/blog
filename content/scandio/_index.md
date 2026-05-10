---
title: "Scandio"
date: 2026-05-09
draft: false
showtoc: false
ShowRssButtonInSectionTermList: false
---

Scandio is a loyalty-card manager for iPhone. Add a card by scanning its barcode, give it a name and a colour, and the next time you're at the checkout you pull it up in two taps and let the cashier scan it off the screen.

The app is built around the idea that this kind of data has no business leaving your phone. There is no account, no sign-in, and no cloud. Everything you add stays in the app's private container on your device.

## What it does

- Stores loyalty cards with a name, barcode, tile colour, optional store logo, and a category icon.
- Renders seven barcode types: EAN-13, EAN-8, ITF, QR, Code 128, PDF417, and Aztec.
- Boosts screen brightness automatically when a barcode is on screen so it scans cleanly even on cheap supermarket scanners.
- Suggests a category icon from the store name on devices that support Apple Intelligence — the suggestion runs on Apple's on-device language model and never leaves the phone.
- Sorts and filters cards, marks favourites, and surfaces the ones you use most in a Quick Access carousel.
- Exports and imports a `.scandiobackup` file so you can move cards between devices or keep your own copy.

## How to use it

The [user manual](/scandio/manual/) walks through adding cards, scanning barcodes, the Quick Access carousel, search and sort, backup/restore, and every Settings option, with annotated screenshots for each step.

## Privacy

Scandio collects nothing, transmits nothing, and contains no analytics, advertising identifiers, or third-party SDKs. The full [privacy policy](/scandio/privacy/) covers what the app stores locally, how camera and photo-library access is used, and how the optional Apple Intelligence category suggestion works.

## Source code

Scandio is open source. The full source — SwiftUI views, SwiftData model, barcode renderers, backup format — lives at [github.com/andrzejsiemion/scandio-ios](https://github.com/andrzejsiemion/scandio-ios).
