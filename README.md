# Astro Playlist - 853 channels, every one VERIFIED PLAYABLE

Full playlist restored and repaired. Nothing is assumed - every channel was
fetched, decrypted and probed for valid video.

| Group | Channels | How verified |
|---|---|---|
| **Astro (DASH + CENC ClearKey)** | **231** | manifest 200 -> KID matched -> AES-CTR decrypt -> valid h264 stream |
| **External (HLS / DASH / MP4)** | **622** | manifest 200 -> media type confirmed (426 HLS / 144 DASH / 52 MP4) |
| Removed (dead) | 98 | 403 / 404 / 461 / 503 / timeout / non-media body |

## Links

    https://raw.githubusercontent.com/ZenithStellar/astro-playlist/main/astro.m3u
    https://raw.githubusercontent.com/ZenithStellar/astro-playlist/main/astro_clean_backup.m3u

## Why the original playlist failed to play

1. **HTTP 403 on the CDN** - Astro's CloudFront layer checks the **User-Agent**.
   The identical URL returns 200 with the per-channel UA and 403 with `Mozilla/5.0`.
   Every Astro channel here carries its own `#EXTVLCOPT:http-user-agent`.
2. **DRM** - CENC **ClearKey** (not Widevine). The key rides in
   `#KODIPROP:inputstream.adaptive.license_key` as `KID:KEY`.
3. **Retired path** - `default_primary.mpd` returns 403. `default_ott.mpd` is live.
   Where a channel only shipped the retired path, the live twin was resolved and
   verified.
4. **Missing keys** - 16 premium channels (tvN, NHK, Discovery, History, KTV,
   Nickelodeon, KBS, HITS, Love Nature, ...) had no key in the head revision.
   Their keys were recovered from the repository's git history and re-verified
   by decrypting live segments.

## Per-channel format

    #EXTINF:-1 tvg-name="TV1 HD" group-title="...",TV1 HD
    #KODIPROP:inputstreamaddon=inputstream.adaptive
    #KODIPROP:inputstream.adaptive.manifest_type=dash
    #KODIPROP:inputstream.adaptive.license_type=org.w3.clearkey
    #KODIPROP:inputstream.adaptive.license_key=912760c4...:bea2d0f8...
    #EXTVLCOPT:http-user-agent=com.genuine.leone
    https://linearjitp-playback.astro.com.my/dash-wv/linear/711/default_ott.mpd

## Players

- **Fermata 2.0** - its IPTV layer parses `#KODIPROP:...license_key=KID:KEY` and
  `#EXTVLCOPT:http-user-agent`, then sets `C.CLEARKEY_UUID` on ExoPlayer with a
  framework CDM. ClearKey CDM is built into AOSP, so no Google Play Services
  needed. Works as-is.
- **Kodi / TiviMate / VLC** - import the raw URL.
- Standalone player: `astro_player/astro_dash_player.py` (own MPD fetch +
  segment download + ClearKey AES-CTR decrypt).

## Notes

- Astro's public CIFI feed is 480x270 / 640x480 SD. 1080p Astro Only needs the
  official Astro GO app with a subscription.
- Keys rotate. When a channel stops, refresh the playlist - new keys land here.
