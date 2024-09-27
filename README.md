# A mapping of music tracks across platforms
The purpose of this mapping is to map each music track across different services. Currently, this list supports YouTube, Apple Music and Spotify.

# Why?
Because of frustration with online converters. Here are some culprits:
- **Inaccurate**: When migrating a playlist from one service to another, either tracks are not accurate, or completely missing.
- **Unusable**: Either their user interface is infested with ads, or their **free tier** is not worth even looking at.
- **Impermanent**: These websites may go down anytime, or break for some reason. Those converters are even less permanent than the music platforms themselves.
- **Unreviewed**: The track mappings are not reviewed, so they are vulnerable to fake tracks.

And also:
- [File over App](https://stephango.com/file-over-app): The whole thing is a single well-documented human-readable CSV file with no frills, anything with a text editor can edit it. (Although the actual music files aren't accessible at all because of DRM, and there's a non-zero chance that these platforms may just stop existing)
- **Scriptable**: Just scribble up a quick and dirty script and you're on track. Or port the whole CSV into a SQLite database and run SQL queries over it.
- **(Free)²**: Free as in both meanings. The license ensures it stays that way (non-commercial and share-alike).

# How to contribute?
The contributions are done using pull requests and issues. Pull requests are for updating the mapping file, and won't be merged until every line of modification is reviewed and confirmed for **absolute** accuracy. Issues are for reporting missing tracks and inaccuracies, which must not be closed until all integrated to the mapping.
- **Use links from the primary artist's original page**: This makes sure the traffic reaches to them, instead of reuploaders. Most platforms reward artists based on the amount of traffic they bring.

YouTube is technically a video platform, but it also is a music platform. But it can be tricky to handle:
- **Make sure to read descriptions**: Most artists link to other services, which can be added to the mapping.
- **Use highest quality video**: YouTube generates videos on its own for some music videos, use these if the artist's own music channel does not exist. If artist's video contain extraneous content than the music track itself, prefer YouTube auto-generated one instead.
- **No X-hour repeated versions, "8D" versions, effects versions, mashups, fan clips, or whatever**: These are considered remixes as well. Remixes are considered separate tracks on their own, so map to their exact replicas, just like originals. Definitely do not map the original to a remix and vice versa.
- **Single, whole tracks only**: Do not try to match a whole album to a song. Artists sometimes upload the whole album as a single video, which we won't map, to keep things simple. (You can map each song in a playlist/album one by one and construct from it, see *scripting*)

# Format
The mapping is a CSV file, containing a row for each track, and column for each service. Instead of storing a full-length URL, the mapping stores *only* the identifier part to save space. The mapping file is encoded in UTF-8 format, with default CSV export settings of LibreOffice Calc 7.4.7.2.

- **Spotify**: `open.spotify.com/*/track/{identifier}` where identifier is a 22-character alphanumeric string.
- **YouTube**: `youtube.com/watch?v={identifier}` where identifier is a 11-character alphanumeric string.
- **Apple Music**: `music.apple.com/**/song/*/{identifier}` where identifier is an unsigned integer of at least 32 bits.

The `*` captures single path segment, and `**` captures all the path segments until the first instance of the next one then stops (excluding it). 

Example:
```
https://music.apple.com/us/song/meol/288667610
                        ^^      ^^^^ ^^^^^^^^^
                        **      *    identifier
```

The columns are: `id`, `applemusic`, `youtube`, and `spotify`. Increment the ID from the last row, and put platform identifiers to their corresponding columns.

The values are converted back to links using this formulae:
- **Spotify**: `{identifier}` -> `https://open.spotify.com/track/{identifier}` (will redirect to the region of your country)
- **YouTube**: `{identifier}` -> `https://youtube.com/watch?v={identifier}` (will just open the video page)
- **Apple Music**: `{identifier}` -> `https://music.apple.com/song/{identifier}` (will redirect to the region of your country)

# License
This work © 2024 is licensed under CC BY-NC-SA 4.0. To view a copy of this license, visit https://creativecommons.org/licenses/by-nc-sa/4.0/
