# Astro Playlist (WORKING - Astro DASH ClearKey)

175 channel Astro Malaysia - **verified working** (manifest OK, ClearKey KID match,
decrypt OK, valid h264 output for every channel).

## Why the old playlist failed

1. **HTTP 403** - Astro's CDN (CloudFront) checks the **User-Agent**. The same URL
   returns 200 with the per-channel UA from `#EXTVLCOPT` and 403 with `Mozilla/5.0`.
   Every channel in this playlist carries its own UA.
2. **DRM** - CENC **ClearKey** (not Widevine). The key is carried in
   `#KODIPROP:inputstream.adaptive.license_key` as `KID:KEY`.

## Links

    https://raw.githubusercontent.com/ZenithStellar/astro-playlist/main/astro.m3u
    https://raw.githubusercontent.com/ZenithStellar/astro-playlist/main/astro_clean_backup.m3u

## Format (per channel)

    #EXTINF:-1 tvg-name="TV1 HD" group-title="Astro Live (ClearKey)",TV1 HD
    #KODIPROP:inputstreamaddon=inputstream.adaptive
    #KODIPROP:inputstream.adaptive.manifest_type=dash
    #KODIPROP:inputstream.adaptive.license_type=org.w3.clearkey
    #KODIPROP:inputstream.adaptive.license_key=912760c4...:bea2d0f8...
    #EXTVLCOPT:http-user-agent=com.genuine.leone
    https://linearjitp-playback.astro.com.my/dash-wv/linear/711/default_ott.mpd

Use `default_ott.mpd` - the older `default_primary.mpd` path is retired (403).

## Players

- **Kodi / TiviMate / VLC** - import the raw URL directly (they read the KODIPROP + UA).
- **Fermata 2.0** - the built-in IPTV player parses `#KODIPROP:...license_key=KID:KEY`
  and `#EXTVLCOPT:http-user-agent`, so this playlist works there as-is.
- Standalone player: `astro_player/astro_dash_player.py` (see its README).

## Notes

- Astro's public CIFI feed is 480x270 / 640x480 SD. HD (1080p) Astro Only needs
  the official Astro GO app with a subscription.
- Keys rotate. If a channel stops, refresh the playlist - the new key is published here.
