# Music

The Arche phone's music player, written in [Arche](https://github.com/arche-family/arche-lang): a WebAssembly component of the Arche phone's world, which the phone has from its first start. That world is Nimadir's mini-app world and what a phone adds to it, here the phone's shared files and its sound; its WIT is in `wit/` (from arche-os's `phone/wit`), and `world` names it. Nimadir's own shell, which has none of it, does not run it.

It works like a phone's music player:

- The songs in the phone's shared Music folder (MP3, FLAC, Ogg Vorbis, WAV and AAC in M4A files), a row each by their tags' title, artist and album (ID3v2.2 to 2.4, FLAC's and Vorbis's comments, MP4's), their cover where the tags have one, and their length; the one that plays is marked. Shuffle all.
- A tap plays a song from its start, the songs after it next. A song played to its end leaves for the next, as Repeat says (all, one, none).
- While one plays, a bar at the bottom: its cover, name and artist, how far it is, Pause and Next. A tap on it opens the player: the cover large, the song's name and artist, where it is and how long it is on a bar a tap moves along, Previous, Pause and Next, Shuffle and Repeat. The phone's Back closes it.
- The player reaches the screen's foot, its names, its seek bar with its knob, the times and the keys low on it, the cover above.

It speaks the phone's language (English, Uzbek or Russian, as `host-api`'s `locale` says), the count of songs with each language's plural, and makes its page again in the new one when the language changes, the song playing on; songs' titles and artists are shown as their tags have them.

The phone plays the song, one at a time for the whole phone, and says what plays: a notification's chime pauses it a moment, and a call or an alarm stops it. The app lists the Music folder through the phone world's `files` interface and reads a song's tags through `read-part`, and plays through its `audio` interface (`play`, `stop`, `playing`, `pause`, `resume`, `paused`, `position`, `duration`, `seek`), which the phone gives an app whose manifest needs `files`. It is an app of the page: its taps come as click events.

## Build

With [arche-lang](https://github.com/arche-family/arche-lang) checked out next to this repository and built (`make -C stage0 && make -C compiler && make -C wasm`):

```sh
../arche-lang/wasm/nimadir-app.sh . music.component.wasm
```

`manifest` is what the phone's repository packages with the component. It gives the app's name, version and title, and what it may do: read the shared files (`files`), and so play them.

## Test

`media/Music` holds two short WAV files, a short MP3 and a text file, which is not music. `headless` holds the taps of a headless run, with `media/` shared as the phone shares its files (arche-wasm's `--share`) and the functions the phone's world adds: a song played, its call made, and Stop. `expected.txt` holds what arche-wasm's interpreter said, as Nimadir's shell does not run the phone's world. `app-check.sh` runs the app with arche-wasm, by the interpreter and by the JIT, each in a fresh copy of this directory, and compares what it says with `expected.txt`. CI runs it when started by hand (Actions' Run workflow). The song heard, through the phone's sound card, is tried on the phone by arche-os's `tests/phone.sh`.

```sh
../arche-lang/wasm/app-check.sh .
NIMADIR=~/nimadir ../arche-lang/wasm/app-expected.sh .   # expected.txt again, from arche-wasm
```

## License

MIT or Apache-2.0, at your option ([LICENSE](LICENSE)).
