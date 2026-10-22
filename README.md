# War of Dots Replay Player

Watch War of Dots replays in the browser: https://aexer0e.dev/wod-replay-player/

Drop `.rep` files on the home page to watch them; they are kept in that browser only. Tournament brackets and their replays are listed below. Every page has a link that can be shared, such as `#/gt3/105/6-city` for one game.

## How this repository works

The player is the one from [More of Dots](https://github.com/aexer0e/more-of-dots). This repository holds no player or engine source and no build output. It contains:

- `.github/workflows/deploy.yml`, which checks out `more-of-dots`, compiles its replay engine to WebAssembly, builds the player page and publishes the result to GitHub Pages.
- `audio/`, the game's music and sound effects in web formats.
- `maps/`, the game's official map images, laid out as in the game's folder (`maps/assets/zolamare_maps/map56.png`). Replays on official maps only name their map, so the player and the replay cards need these images.
- `tournaments/`, one folder per tournament with its `.rep` files and `tournament.json`.

Changes to the player or the engine are made in `more-of-dots`.

## Deploying

The site is rebuilt when `VERSION` changes on the `more-of-dots` main branch, and when a push to this repository changes `tournaments/` or `maps/`. To deploy by hand, run the **Deploy site** workflow from the Actions tab and enter the `more-of-dots` branch, tag or commit to build.

`version.json` on the site records the version and commit it was built from.

## Tournaments

Each folder in `tournaments/` is one tournament; its name is the tournament's address, as in `#/gt3`. When a folder is renamed, list its old name under `"aliases"` in `tournament.json` so old links still open. It holds the replays and a `tournament.json` with:
- the name, format and start date
- the final results
- each player's bracket name and seed, and `username` when their in-game name is known
- the matches from Challonge

To add replays, put the `.rep` files in the folder and make sure both players of each replay have a `username`. The build assigns each replay to the match those two players played. When two players met more than once, name the replay's match in `"replays": { "file.rep": 113 }`. See `docs/web-player.md` in `more-of-dots` for the details.

## Maps

The files in `maps/assets/` are copied from the game's `assets` folder: every image in `fahero_maps`, `eronion_maps` and `custom_maps`, and the numbered maps in `zolamare_maps` without the large `world_map` images. After a game update adds maps, copy them in again and push.

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
