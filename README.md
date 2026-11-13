# Music

The Arche phone's music player, written in [Arche](https://github.com/arche-family/arche-lang): a WebAssembly component of the Arche phone's world, which the phone has from its first start. That world is Nimadir's mini-app world and what a phone adds to it, here the phone's shared files and its sound; its WIT is in `wit/` (from arche-os's `phone/wit`), and `world` names it. Nimadir's own shell, which has none of it, does not run it.

It works like Android's music players:

- The songs in the phone's shared Music folder (WAV, FLAC, Ogg Vorbis and MP3 files, which the phone decodes), a row each by name; the one that plays is marked.
- A tap plays a song from its start, in the place of what played.
- While one plays, its name and Stop are at the bottom. A song played to its end leaves nothing playing.

The phone plays the song, one at a time for the whole phone, and says what plays: a notification's chime pauses it a moment, and a call or an alarm stops it. The app lists the Music folder through the phone world's `files` interface and plays through its `audio` interface (`play`, `stop`, `playing`), which the phone gives an app whose manifest needs `files`. It is an app of the page: its taps come as click events.

## Build

With [arche-lang](https://github.com/arche-family/arche-lang) checked out next to this repository and built (`make -C stage0 && make -C compiler && make -C wasm`):

```sh
../arche-lang/wasm/nimadir-app.sh . music.component.wasm
```

`manifest` is what the phone's repository packages with the component. It gives the app's name, version and title, and what it may do: read the shared files (`files`), and so play them.

## Test

`media/Music` holds two short WAV files, a short MP3 and a text file, which is not music. `headless` holds the taps of a headless run, with `media/` shared as the phone shares its files (arche-wasm's `--share`) and the functions the phone's world adds: a song played, its call made, and Stop. `expected.txt` holds what arche-wasm's interpreter said, as Nimadir's shell does not run the phone's world. `app-check.sh` runs the app with arche-wasm, by the interpreter and by the JIT, each in a fresh copy of this directory, and compares what it says with `expected.txt`. CI runs it on every push. The song heard, through the phone's sound card, is tried on the phone by arche-os's `tests/phone.sh`.

```sh
../arche-lang/wasm/app-check.sh .
NIMADIR=~/nimadir ../arche-lang/wasm/app-expected.sh .   # expected.txt again, from arche-wasm
```

## License

MIT or Apache-2.0, at your option ([LICENSE](LICENSE)).
