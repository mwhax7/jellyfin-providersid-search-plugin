# Jellyfin Providers ID Items Search API

A lightweight Jellyfin plugin exposing a simple API endpoint to search your Jellyfin library using external provider identifiers such as IMDb, TMDb or TVDb IDs.

This plugin is mainly designed as a companion for **Stremio ↔ Jellyfin integrations**, allowing Stremio addons to quickly determine whether a movie or TV show already exists in your local Jellyfin library.

---

## Why does this plugin exist?

Jellyfin stores metadata identifiers from multiple providers:

- IMDb (`tt1234567`)
- The Movie Database (TMDb)
- The TV Database (TVDb)
- Other metadata providers supported by Jellyfin

These identifiers are extremely useful because they provide a universal way to reference media, independently from file names or localized titles.

For example:

```
The Matrix (1999)
IMDb: tt0133093
TMDb: 603
```

A Stremio addon can identify a movie using its IMDb ID and ask Jellyfin:

> "Do you already have the movie with IMDb ID `tt0133093`?"

If Jellyfin answers yes, the addon can directly link the user to their local copy instead of showing an external stream.

---

## Stremio integration

### What is Stremio?

Stremio is a media aggregation platform based on addons.  
It provides a unified interface to discover movies and TV shows from various sources.

A Stremio addon can provide:

- Metadata catalogs
- Search capabilities
- Streaming sources

Each media item is generally identified by a universal identifier such as an IMDb ID.

---

### How this plugin helps

The default Jellyfin API does not provide a simple dedicated endpoint for searching media by provider IDs.

This plugin exposes an API that allows external applications to perform requests like:

```
GET /ProviderIds/Search?provider=imdb&id=tt0133093
```

and receive the matching Jellyfin item.

This makes it possible to build integrations where:

```
           Stremio
              |
              | IMDb / TMDb ID
              |
              v
  Providers ID Search API
              |
              | Search Jellyfin database
              |
              v
           Jellyfin
              |
              | Existing media found
              |
              v
       Play local file
```

---

## Use cases

### Stremio Jellyfin Addons

A Stremio addon can:

1. Receive a movie request from Stremio.
2. Extract its IMDb/TMDb identifier.
3. Query this plugin.
4. Check if the content already exists in Jellyfin.
5. Redirect playback to the local Jellyfin media.

This enables a seamless "local library first" experience.

---

## Installation

### Using the plugin repository

Add the following repository to Jellyfin:

```
https://raw.githubusercontent.com/mwhax7/jellyfin-providersid-search-plugin/refs/heads/main/manifest.json
```

Go to:

```
Dashboard → Plugins → Repositories
```

Add the repository, install **Providers ID Items Search API**, then restart Jellyfin.

---

### Manual installation

1. Download the latest release.
2. Copy the plugin files to your Jellyfin plugins directory.
3. Restart Jellyfin.

Typical plugin locations:

**Windows**

```
%ProgramData%\Jellyfin\Server\plugins
```

**Linux**

```
/var/lib/jellyfin/plugins
```

---

## API

### Search by provider ID

Example:

```
GET /ProviderIds/Search?provider=imdb&id=tt0133093
```

Supported providers depend on your Jellyfin metadata configuration.

Possible examples:

- `imdb`
- `tmdb`
- `tvdb`

---

## Compatibility

- Jellyfin Server
- External applications using provider identifiers
- Stremio companion addons

---

## Credits

Original project:

https://github.com/akarazniewicz/jellyfin-providersid-search-plugin

This repository is a fork maintained by **MWHAX7**.

---

## References

- Jellyfin Metadata Providers:
  https://jellyfin.org/docs/general/server/metadata/identifiers

- Original Stremio Jellyfin integration:
  https://github.com/akarazniewicz/stremio-jellyfin
