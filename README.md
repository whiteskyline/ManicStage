# ManicStage

ManicStage is a native macOS app for syncing music to older Sony Walkman devices that use the `OMGAUDIO` library structure, including verified S, E, and A series SonicStage-era devices.

It is meant to restore the basic music-syncing workflow that used to depend on SonicStage, while running on modern macOS.

ManicStage is **not** a general-purpose music player. It is **not** a modern Walkman manager. Its first target is SonicStage-era Sony `OMGAUDIO` Walkman devices that have been verified or can be safely tested with a full backup.

## Important!!! Back up your Walkman first!!!

Before using ManicStage for the first time, back up the entire USB root directory of your Walkman to your computer.

Do not back up only the `OMGAUDIO` folder. Copy the whole mounted Walkman volume.

For example, if your Walkman appears as a mounted USB drive in Finder, copy everything from that drive into a safe folder on your Mac before initializing, importing, deleting, or syncing music with ManicStage.

This matters because older `OMGAUDIO` devices rely on multiple database files, indexes, search trees, and cached metadata. A full backup gives you a way to restore the original device state if something goes wrong.

> **After making a full backup, follow the [First-time usage](#first-time-usage) section below for your first setup and sync.**

## System requirements

- macOS 15 or later
- Apple Silicon Mac, M-series chip
- A Sony Walkman that mounts as a USB storage device and uses the `OMGAUDIO` library structure
- FFmpeg / FFprobe installed locally if you want ManicStage to transcode unsupported audio formats

## Device support

ManicStage primarily targets Sony Walkman devices that use the `OMGAUDIO` music library structure and originally depended on SonicStage for music transfer.

Verified devices:

- NW-S700 series
- NW-S600 series
- NW-E300 series
- NW-E400 series
- NW-E500 series
- NW-A600 series

Other Sony Walkman devices that require SonicStage for music syncing are theoretically supported, but they should be treated as unverified until tested.

If you try ManicStage with an unverified SonicStage-era Walkman, back up the entire Walkman USB root directory first, then verify the result on the device.

If you try ManicStage with another `OMGAUDIO` Walkman, please report:

- Device model
- macOS version
- Whether the device was detected
- Whether import, delete, sync, playback, search, and cover display worked
- Whether the device entered Simple Mode


## What ManicStage does

ManicStage provides a modern macOS workflow for older Sony Walkman devices:

- Detect mounted Sony Walkman / `OMGAUDIO` devices.
- Read the device music library.
- Show songs, albums, artists, and device information.
- Import local music files to the device.
- Delete songs from the device.
- Manage pending sync tasks before writing changes.
- Initialize a minimal usable empty library on an empty device.
- Preserve compatible MP3 audio when possible instead of re-encoding it.
- Maintain the library indexes, search trees, and cover cache required by S705-era devices.
- Use the user's local FFmpeg / FFprobe installation for audio probing, transcoding, metadata migration, and cover handling when needed.

## Screenshots

### Device information

Detect a mounted Walkman, review device information, and select the target disk when needed.

![ManicStage device information](assets/screenshots/device-info.png)

### Library

Browse songs on the device before importing or deleting music.

![ManicStage library](assets/screenshots/library.png)

### Sync tasks

Review pending imports and deletions before writing changes to the Walkman.

![ManicStage sync tasks](assets/screenshots/sync-tasks.png)

## What ManicStage does not do

ManicStage is intentionally narrow in scope:

- It is not a replacement for every SonicStage feature.
- It is not a general music player.
- It is not a manager for modern Walkman devices.
- It does not provide manual editing for song title, artist, album, genre, or release year.
- It does not provide manual cover replacement.
- It does not generate ATRAC Advanced Lossless.
- It does not depend on SonicStage or the proprietary OpenMG encoding chain.
- It does not guarantee full compatibility with every Walkman model.

## Installation

1. Download `ManicStage-<version>.dmg` from the release page.
2. Open the DMG.
3. Drag `ManicStage.app` into `/Applications`.
4. Launch ManicStage.

If macOS shows a security prompt, open the app according to your local macOS security settings.

## First-time usage

### 0. Back up the device

Before doing anything else, back up the entire Walkman USB root directory to your Mac.

This is the most important first-time step.

### 1. Connect the Walkman

Connect your Sony Walkman to your Mac using USB.

Wait until macOS mounts it as a USB volume.

### 2. Select the device disk

Open ManicStage and go to the device information page.

ManicStage can try to detect a mounted Sony Walkman / `OMGAUDIO` device automatically. If automatic detection does not find the device, manually select the mounted Walkman directory.

Choose the Walkman USB root directory, not only the `OMGAUDIO` subfolder.

### 3. Initialize an empty library if needed

If this is your first time using the device with ManicStage, or if the device has an empty or missing library, initialize an empty library.

This creates the minimal library structure required for ManicStage to manage music on the device.

Only do this after making a full backup.

### 4. Add or delete music

Open the library page to view songs already on the device.

To add music, click import and select local audio files from your Mac.

To delete music, select songs in the library and mark them for deletion.

Adding and deleting songs creates pending sync tasks. These changes are not written to the Walkman immediately.

### 5. Review sync tasks

Open the sync tasks page and review the pending imports and deletions.

This step lets you confirm what ManicStage is about to write to the device.

### 6. Click sync

Click sync to write the pending changes to the Walkman.

ManicStage will update the audio files, library database, indexes, search trees, and related cache files needed by the device.

### 7. Eject safely

When sync is finished, safely eject the Walkman from macOS before unplugging it.

Then check the device itself:

- Playback
- Song search
- Album search
- Cover display

If the device shows **Simple Mode**, it is usually not a fatal problem. Simple Mode means some advanced search features, such as Genre Search or Release Year Search, may be unavailable, but basic playback can still work.

## Known limits

- The library limit follows S705-era constraints. ManicStage currently treats the upper bound as about 511 songs.
- Manual metadata editing is not supported.
- Manual cover replacement is not supported.
- S705 cover data is album/group-level, not the per-track cover model used by many modern players.
- WMA is not preserved as a passthrough target and is usually transcoded.
- AAC-LC is usually transcoded in the current version.
- WAV is usually transcoded in the current version, unless using a lossless strategy that outputs 44.1 kHz / 16-bit / stereo WAV.
- Compatibility with devices outside the verified list should be considered experimental until confirmed.

## Audio import strategy

ManicStage follows a simple rule:

Keep the original audio when it is already compatible. Transcode only when needed.

| Input format | Current behavior |
| --- | --- |
| Compatible MP3 | Preserved without re-encoding |
| Incompatible MP3 | Transcoded to compatible MP3 |
| FLAC / ALAC / APE / DSD | Transcoded |
| Opus / Ogg Vorbis | Transcoded |
| WMA | Transcoded |
| AAC-LC | Usually transcoded in the current version |
| WAV | Usually transcoded in the current version; under a lossless strategy, it may be converted to 44.1 kHz / 16-bit / stereo WAV |

Recommended output targets:

- General balance between compatibility and capacity: LAME MP3 V2 VBR
- Higher quality lossy output: LAME MP3 V0 VBR
- Maximum lossy compatibility: MP3 320 kbps CBR
- Lossless strategy: 44.1 kHz / 16-bit / stereo WAV

## FFmpeg and FFprobe

ManicStage does **not** bundle FFmpeg or FFprobe.

When transcoding or audio probing is needed, ManicStage calls the FFmpeg / FFprobe tools installed on your Mac.

They are used for:

- Audio format detection
- Compatibility probing
- Transcoding unsupported formats
- Metadata migration
- Cover handling

You can install FFmpeg with Homebrew:

```bash
brew install ffmpeg
```

After installation, make sure `ffmpeg` and `ffprobe` are available in your shell path.

```bash
ffmpeg -version
ffprobe -version
```

If ManicStage cannot find FFmpeg / FFprobe, compatible MP3 passthrough may still work, but transcoding and some metadata or cover operations may be unavailable.

## Safety notes

- Always back up the whole Walkman USB root directory before first use.
- Do not unplug the device while sync is running.
- Eject the Walkman safely after syncing.
- Keep your original local music files on your Mac.
- Treat unsupported Walkman models as experimental until confirmed.

## Troubleshooting

### ManicStage does not detect my Walkman

- Make sure the Walkman is mounted in Finder.
- Confirm that the mounted volume contains an `OMGAUDIO` folder.
- Try selecting the mounted Walkman root directory manually.
- Do not select only the `OMGAUDIO` folder.

### Imported songs do not appear on the device

- Confirm that you clicked sync after importing.
- Eject the device safely and then check the Walkman.
- Make sure the library has not exceeded the supported song limit.

### The device enters Simple Mode

Simple Mode may indicate that some advanced search indexes are unavailable or limited.

Basic playback can still work, but features such as Genre Search or Release Year Search may not be available.

### Transcoding fails

- Install FFmpeg with Homebrew.
- Confirm `ffmpeg -version` and `ffprobe -version` work in Terminal.
- Try importing a compatible MP3 file to verify basic passthrough behavior.

## Support & contact

ManicStage is free to use. All features are free, and support is completely voluntary.

If ManicStage helped bring your old Walkman back into use, or if you appreciate this little project, any contribution is appreciated.

<p>
  <img src="assets/support/alipay-payment-card.png" alt="Alipay support QR code" width="220">
  <img src="assets/support/paypal-payment-card.png" alt="PayPal support QR code" width="220">
</p>

For device stories, compatibility feedback, logs, bug reports, or a friendly chat, you can reach me here:

- Email: [ming.horizon@icloud.com](mailto:ming.horizon@icloud.com)
- GitHub: [whiteskyline/ManicStage](https://github.com/whiteskyline/ManicStage)
- YouTube: [@ZaichuanLin](https://www.youtube.com/@ZaichuanLin)
- Xiaohongshu / 小红书: [GoodDog](https://www.xiaohongshu.com/user/profile/5bef04fc96d5be00014890f3)
- Reddit: [u/HorizonDog777](https://www.reddit.com/user/HorizonDog777/)
- WeChat / 微信: `HorizonDog`

The WeChat QR code is for contact, not payment.

<p>
  <img src="assets/support/wechat-contact.jpg" alt="WeChat contact QR code" width="220">
</p>

## Project status

ManicStage focuses first on reliable syncing for verified SonicStage-era `OMGAUDIO` Walkman devices.

Support for additional Walkman models may improve over time, but compatibility should be verified device by device.

If you test ManicStage on another Sony `OMGAUDIO` Walkman, please share the model and results.
