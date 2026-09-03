# Utter

Private, on-device voice dictation for macOS, Windows and Linux.

Hold a key, speak, release. The text lands in whatever field you were already
typing in — your editor, your browser, your terminal.

## Nothing leaves your machine

Speech is transcribed locally. There is no server, no account, no API key, and
no telemetry. It works with the wifi off, on a plane, in a hospital, under an
NDA. Your voice is never uploaded because there is nowhere for it to go.

That is the whole point of the product, not a setting you have to find.

## What makes it different

**It learns your vocabulary.** Most dictation tools mangle proper nouns —
*Vercel* becomes *versa*, *Supabase* becomes *super base*. Utter carries a
dictionary that biases the decoder toward the words you actually use, and a
confidence-weighted repair pass that fixes a word the model guessed at while
leaving alone a word it heard clearly. A rewrite may repair a guess; it may
never overrule something the model was sure of.

**It refuses to invent.** Whisper models hallucinate on silence — "thanks for
watching", "end of the video". Utter scores every result and drops fabrications
before they reach your clipboard, without discarding quiet or distant speech.

**Four model tiers.** Mini for speed, Fast and Quality for accuracy and other
languages. Switch without restarting.

## Install

Grab the latest build from [Releases](../../releases).

**macOS** — unzip and drag to Applications. The build is signed for development
rather than notarised, so the first launch needs right-click → Open. Grant
Microphone, Accessibility and Input Monitoring when asked: the first records,
the second places text in the focused field, the third hears the hotkey.

**Windows** — run the installer. SmartScreen will warn about an unrecognised
publisher, which is what an unsigned installer looks like.

The first run downloads the speech model once, then never again.

## Status

Actively developed. macOS is furthest along; Windows and Linux share an
Electron build. An Android version is in progress.

## Licence

Copyright © 2026. All rights reserved. See [LICENSE](LICENSE).

Third-party components and their licences are listed in
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
