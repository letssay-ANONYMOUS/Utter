# Utter

Private, on-device voice dictation for macOS, Windows and Linux.

> **This repository is the product page, not the source.** Utter is commercial and
> closed source; no implementation is published here. Builds are linked below.

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

Grab the latest build from [utter-releases](https://github.com/letssay-ANONYMOUS/utter-releases/releases).

**macOS** — unzip and drag to Applications. The build is signed for development
rather than notarised, so the first launch needs right-click → Open. Grant
Microphone, Accessibility and Input Monitoring when asked: the first records,
the second places text in the focused field, the third hears the hotkey.

**Windows** — run the installer. SmartScreen will warn about an unrecognised
publisher, which is what an unsigned installer looks like.

The first run downloads the speech model once, then never again.

## Platforms

| Platform | Build |
| --- | --- |
| macOS | Native, Swift |
| Windows · Linux | Electron, shared core |
| Android | Native, floating overlay rather than a keyboard |
| iOS | Custom keyboard extension |

## Where the time goes

Dictation is judged almost entirely on the delay between finishing a sentence and
seeing the text, and most of that delay is architecture rather than model quality.

**Opening the microphone costs 100–200 ms on macOS.** That is dead time before a
single sample exists, and it lands at the worst possible moment — the start.
Keeping the input warm removes it, at the cost of being careful about when the
device is genuinely released.

**Transcription streams rather than batching.** Waiting for the whole utterance
before starting means the user waits for the utterance *plus* the inference.
Feeding a rolling window means most of the work is done by the time they stop.

**Model size is a latency decision, not an accuracy one.** Smaller ASR models are
several times faster than larger ones on short takes and roughly tie on long ones
— which matters, because most dictation is short takes. The trade is punctuation,
so it belongs in the tier switch rather than in a global default.

**Long audio cannot just be cut into chunks.** Splitting a take mid-word makes the
model hallucinate across the seam, so segment boundaries follow silence, and
in-flight segments have to survive the end of a take rather than being dropped.

## Phone as a microphone, without trusting the middle

The phone can record while the desktop transcribes, over a relay that is assumed
hostile — it may be a tunnel provider, a CDN, or an attacker who has taken either.

Pairing is by QR code: the desktop generates a room id and a 32-byte key locally
and renders both into the code, so **the key travels as photons and never over the
network**. The relay learns the room id because it must route, and never learns the
key. Payload frames are AES-256-GCM with a per-direction monotonic sequence number.
The relay's own control frames are unauthenticated by definition, so they are
treated as hints only — never as data, never as instructions.

The protocol is specified separately from its implementation and carries
cross-language test vectors, so the Swift, Kotlin and Node ends can be shown to
agree rather than assumed to.

## Status

Actively developed. macOS is furthest along; Windows and Linux share an
Electron build. An Android version is in progress.

## Licence

Copyright © 2026. All rights reserved. See [LICENSE](LICENSE).

Third-party components and their licences are listed in
[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
