# War of Dots Replay Player

Watch War of Dots replays in the browser: https://aexer0e.dev/wod-replay-player/

Open a `.rep` file with the button, drop it on the page, or link to one with `?replay=<url>`.

## How this repository works

The player is the one from [More of Dots](https://github.com/aexer0e/more-of-dots). This repository holds no player or engine source and no build output. It contains:

- `.github/workflows/deploy.yml`, which checks out `more-of-dots`, compiles its replay engine to WebAssembly, builds the player page and publishes the result to GitHub Pages.
- `audio/`, the game's music and sound effects in web formats.

Changes to the player or the engine are made in `more-of-dots`.

## Deploying

The site is rebuilt when `VERSION` changes on the `more-of-dots` main branch. To deploy by hand, run the **Deploy site** workflow from the Actions tab and enter the `more-of-dots` branch, tag or commit to build.

`version.json` on the site records the version and commit it was built from.

## Audio

The files in `audio/` come from the game's `assets` folder:

| File | Source | Conversion |
|---|---|---|
| `music.mp3` | `music/won.wav` | MP3, 160 kbit/s |
| `fighting.flac` | `sound_effects/fighting.wav` | FLAC, 16 bit |
| `produce_unit.wav` | `sound_effects/produce_unit.wav` | none |
| `victory.wav` | `sound_effects/victory.wav` | none |
| `defeat.wav` | `sound_effects/defeat.wav` | none |

To regenerate the converted files:

```bash
ffmpeg -i music/won.wav -c:a libmp3lame -b:a 160k music.mp3
```

```bash
ffmpeg -i sound_effects/fighting.wav -c:a flac -sample_fmt s16 -compression_level 8 fighting.flac
```
