# Astro Playlist - 832 channels, VERIFIED PLAYABLE

Full playlist restored and every channel tested. Dead links removed.

| Group | Channels | Verified |
|---|---|---|
| Astro (DASH ClearKey) | 210 | manifest OK + KID match + decrypt OK + valid h264 |
| External (HLS / DASH / MP4) | 622 | manifest fetch OK + media type confirmed |
| Removed (dead) | 124 | 403 / 404 / 461 / timeout / non-media |

## Links

    https://raw.githubusercontent.com/ZenithStellar/astro-playlist/main/astro.m3u
    https://raw.githubusercontent.com/ZenithStellar/astro-playlist/main/astro_clean_backup.m3u

## Why the ORIGINAL playlist failed to play

1. **HTTP 403** - Astro's CDN (CloudFront) checks the **User-Agent**. The same URL
   returns 200 with the per-channel UA from `#EXTVLCOPT` and 403 with `Mozilla/5.0`.
2. **DRM** - CENC **ClearKey** (not Widevine). Key carried in
   `#KODIPROP:inputstream.adaptive.license_key` as `KID:KEY`.
3. **Wrong path** - `default_primary.mpd` is retired (403). Use `default_ott.mpd`.

## Per-channel format

    #EXTINF:-1 tvg-name="TV1 HD" group-title="...",TV1 HD
    #KODIPROP:inputstreamaddon=inputstream.adaptive
    #KODIPROP:inputstream.adaptive.manifest_type=dash
    #KODIPROP:inputstream.adaptive.license_type=org.w3.clearkey
    #KODIPROP:inputstream.adaptive.license_key=912760c4...:bea2d0f8...
    #EXTVLCOPT:http-user-agent=com.genuine.leone
    https://linearjitp-playback.astro.com.my/dash-wv/linear/711/default_ott.mpd

## Players

- **Fermata 2.0** - reads `#KODIPROP:...license_key=KID:KEY` + `#EXTVLCOPT:http-user-agent`,
  sets `C.CLEARKEY_UUID` on ExoPlayer. Works as-is.
- **Kodi / TiviMate / VLC** - import the raw URL.
- Standalone: `astro_player/astro_dash_player.py`.

## Notes

- Astro public CIFI feed = 480x270 / 640x480 SD. HD 1080p needs the official Astro GO app.
- Keys rotate. If a channel stops, refresh the playlist - the new key is published here.
