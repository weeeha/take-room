# Take Room

Compare four AI-generated takes of one music prompt without losing your place in the bar.

Status: exploration, design draft. Live: https://take-room.vercel.app

## What it does

A text-to-music model gives you several takes of the same prompt. Picking the best one usually means playing each from the top and trying to remember how the last one felt. Take Room plays all four in lockstep, so switching keeps the groove going and you hear the difference at the same point in the loop.

- **Lockstep playback.** All four takes run on one clock. Only the take you select is audible.
- **Switch timing.** Instant (with a 10 ms crossfade so the cut never clicks), on the next beat, or on the next bar. A queued switch shows a dashed key until it lands.
- **Loudness matching.** Each take is levelled to the same RMS (-18 dBFS) and the adjustment is shown per take. Without it, the louder take tends to win.
- **Blind test.** Shuffles the order and hides the names until you turn it off.
- **Keep and drop.** Mark takes as you go; the summary under the lanes lists what you kept.

Keys: `1`-`4` switch, `Space` play or stop, `K` keep, `X` drop, arrow keys move between takes.

## How it is built

One static HTML file, no build step, no dependencies beyond two Google Fonts.

- The four takes are synthesized in the browser with an `OfflineAudioContext` (kick, snare, hats, bass and a Rhodes-style chord voice over a Dm9, G13, Cmaj9, A7 progression at 92 BPM). They stand in for model output; nothing here calls a generation model.
- Playback starts four looping `AudioBufferSourceNode`s at the same `AudioContext` time, each through its own `GainNode`. Switching schedules gain ramps on the audio clock, so beat and bar quantization land sample-accurately instead of on a timer.
- Waveforms are drawn on a dB scale (floor -42 dBFS) so the chords and bass show, not only the kick hits.
- Light and dark themes follow the system setting.

## Run it

Open `index.html` in a browser, or serve the folder:

```bash
npx serve .
```

## License

MIT, see [LICENSE](LICENSE).
