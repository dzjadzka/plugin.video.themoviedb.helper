# TMDb Helper - API Documentation for Integration

This document provides comprehensive documentation for integrating TMDb Helper into other Kodi projects and add-ons.

## Table of Contents

1. [Overview](#overview)
2. [Installation & Dependencies](#installation--dependencies)
3. [Plugin URL Structure](#plugin-url-structure)
4. [Available Routes & Info Parameters](#available-routes--info-parameters)
5. [RunScript Methods](#runscript-methods)
6. [Player Integration](#player-integration)
7. [Library Integration](#library-integration)
8. [API Integrations](#api-integrations)
9. [Context Menu Integration](#context-menu-integration)
10. [Practical Examples](#practical-examples)

---

## Overview

**TMDb Helper** is a comprehensive Kodi add-on that provides:
- Movie, TV Show, and Actor information from TMDb, TVDb, Trakt, OMDb, FanartTV, and MDbList
- Advanced player integration for seamless content playback
- Library auto-update functionality
- Extensive browse and discovery features
- Integration capabilities for other Kodi add-ons

**Plugin ID:** `plugin.video.themoviedb.helper`  
**Version:** 6.12.1+  
**License:** GPL v3

---

## Installation & Dependencies

### Required Dependencies

```xml
<import addon="xbmc.python" version="3.0.1"/>
<import addon="script.module.requests" version="2.9.1"/>
<import addon="script.module.pil" version="1.1.7"/>
<import addon="script.module.addon.signals" version="0.0.6" />
<import addon="script.module.jurialmunkey" version="0.2.29" />
<import addon="script.module.infotagger" version="0.0.5" />
<import addon="script.module.qrcode" version="6.1.0" />
```

### Installation

Install via jurialmunkey repository:
- **Repository URL:** https://jurialmunkey.github.io/repository.jurialmunkey/
- **Direct ZIP:** https://jurialmunkey.github.io/repository.jurialmunkey/repository.jurialmunkey-3.4.zip

---

## Plugin URL Structure

TMDb Helper uses the standard Kodi plugin URL format:

```
plugin://plugin.video.themoviedb.helper/?param1=value1&param2=value2
```

### Base URL Components

| Component | Description | Example |
|-----------|-------------|---------|
| `info` | The route/action to execute | `info=popular` |
| `tmdb_type` | Media type (movie/tv/person) | `tmdb_type=movie` |
| `tmdb_id` | TMDb ID of the item | `tmdb_id=550` |
| `query` | Search query | `query=Fight%20Club` |
| `type` | Content type filter | `type=movie` |
| `page` | Page number for pagination | `page=1` |

### Play URL Format

```
plugin://plugin.video.themoviedb.helper/?info=play&tmdb_id={tmdb_id}&tmdb_type={movie|tv}
```

For episodes:
```
plugin://plugin.video.themoviedb.helper/?info=play&tmdb_id={tmdb_id}&tmdb_type=tv&season={season}&episode={episode}
```

---

## Available Routes & Info Parameters

### Routes Without TMDb ID Required (ROUTE_NOID)

These routes work without requiring a specific TMDb ID and are used for browsing lists and discovery.

#### Discovery & Browse

| Info Parameter | Description | Additional Parameters |
|----------------|-------------|----------------------|
| `discover` | TMDb discover movies/shows | `tmdb_type=movie/tv`, various filters |
| `trakt_discover` | Trakt discover | `tmdb_type`, filters |
| `user_discover` | User-saved discover configurations | - |
| `search` | Multi-search (movies/shows/people) | `query` |
| `gemini` | AI-powered Gemini search | `query` |

#### Popular & Trending

| Info Parameter | Description | Media Type |
|----------------|-------------|------------|
| `popular` | Popular movies/shows | movie/tv |
| `trending_day` | Trending today | movie/tv |
| `trending_week` | Trending this week | movie/tv |
| `top_rated` | Top rated | movie/tv |
| `most_voted` | Most voted | movie/tv |
| `revenue_movies` | Highest revenue movies | movie |

#### Currently Airing & Upcoming

| Info Parameter | Description | Media Type |
|----------------|-------------|------------|
| `airing_today` | Airing today | tv |
| `on_the_air` | Currently on the air | tv |
| `now_playing` | Now playing in theaters | movie |
| `upcoming` | Upcoming releases | movie/tv |

#### Trakt Lists

| Info Parameter | Description | Requires Trakt Auth |
|----------------|-------------|---------------------|
| `trakt_watchlist` | User's watchlist | Yes |
| `trakt_watchlist_released` | Watchlist (released only) | Yes |
| `trakt_watchlist_anticipated` | Watchlist (anticipated) | Yes |
| `trakt_collection` | User's collection | Yes |
| `trakt_history` | Watch history | Yes |
| `trakt_favorites` | User favorites | Yes |
| `trakt_inprogress` | Currently watching | Yes |
| `trakt_ondeck` | On deck (next to watch) | Yes |
| `trakt_ondeck_unwatched` | On deck unwatched only | Yes |
| `trakt_nextepisodes` | Next episodes | Yes |
| `trakt_dropped` | Dropped shows | Yes |
| `trakt_mostplayed` | Most played items | Yes |
| `trakt_mostwatched` | Most watched items | Yes |
| `trakt_towatch` | To watch list | Yes |
| `trakt_trending` | Trakt trending | No |
| `trakt_popular` | Trakt popular | No |
| `trakt_anticipated` | Trakt anticipated | No |
| `trakt_boxoffice` | Box office | No |
| `trakt_recommendations` | Trakt recommendations | Yes |

#### Trakt Lists Management

| Info Parameter | Description |
|----------------|-------------|
| `trakt_mylists` | User's custom lists |
| `trakt_likedlists` | Liked lists |
| `trakt_popularlists` | Popular lists |
| `trakt_trendinglists` | Trending lists |
| `trakt_userlist` | Specific user list (requires `user`, `slug`) |
| `trakt_searchlists` | Search lists |

#### Trakt Calendar

| Info Parameter | Description |
|----------------|-------------|
| `trakt_calendar` | Combined calendar |
| `trakt_moviecalendar` | Movie calendar |
| `trakt_dvdcalendar` | DVD release calendar |
| `trakt_myairing` | My airing shows |

#### Library Integration

| Info Parameter | Description |
|----------------|-------------|
| `library_nextaired` | Next aired from library |
| `library_airingnext` | Library airing next |

#### MDbList Integration

| Info Parameter | Description |
|----------------|-------------|
| `mdblist_toplists` | Top MDbLists |
| `mdblist_yourlists` | Your MDbLists |
| `mdblist_userlist` | Specific MDbList |
| `mdblist_searchlists` | Search MDbLists |
| `mdblist_sortby` | Sort options |

#### Browse by Category

| Info Parameter | Description |
|----------------|-------------|
| `genres` | Browse by genre |
| `all_movies` | All movies |
| `all_tvshows` | All TV shows |
| `all_collections` | All collections |
| `all_keywords` | All keywords |
| `all_networks` | All networks |
| `all_studios` | All studios |
| `watch_providers` | Watch providers |

#### Random/Recommendations

| Info Parameter | Description |
|----------------|-------------|
| `random_popular` | Random popular |
| `random_trending` | Random trending |
| `random_anticipated` | Random anticipated |
| `random_genres` | Random by genre |
| `random_keywords` | Random by keyword |
| `random_networks` | Random by network |
| `random_studios` | Random by studio |
| `random_providers` | Random by provider |
| `random_mylists` | Random from my lists |
| `random_likedlists` | Random from liked lists |
| `random_popularlists` | Random from popular lists |
| `random_trendinglists` | Random from trending lists |
| `random_mostplayed` | Random most played |
| `random_mostviewers` | Random most watched |

#### TVDb Integration

| Info Parameter | Description |
|----------------|-------------|
| `dir_tvdb_awards` | TVDb awards directory |
| `dir_tvdb_award_categories` | Award categories |
| `tvdb_award_category` | Specific award category |
| `dir_tvdb_genres` | TVDb genres |
| `tvdb_genre` | Specific genre |

#### Trakt Advanced

| Info Parameter | Description |
|----------------|-------------|
| `trakt_becauseyouwatched` | Because you watched |
| `trakt_becausemostwatched` | Because most watched |
| `trakt_airingnext` | Airing next |
| `trakt_upnext` | Up next |
| `trakt_genres` | Trakt genres |
| `trakt_years` | By year |
| `trakt_inlists` | In lists |

### Routes Requiring TMDb ID (ROUTE_TMDBID)

These routes require a TMDb ID and are used for detailed information about specific items.

#### Details & Information

| Info Parameter | Description | Media Types |
|----------------|-------------|-------------|
| `details` | Full item details | movie/tv/person |
| `cast` | Cast members | movie/tv |
| `crew` | Crew members | movie/tv |
| `videos` | Trailers/videos | movie/tv/person |
| `reviews` | User reviews | movie/tv |
| `movie_keywords` | Movie keywords | movie |

#### Images

| Info Parameter | Description | Media Types |
|----------------|-------------|-------------|
| `fanart` | Fanart images | movie/tv/person |
| `posters` | Poster images | movie/tv |
| `images` | All images | movie/tv/person |
| `episode_thumbs` | Episode thumbnails | episode |

#### Related Content

| Info Parameter | Description | Media Types |
|----------------|-------------|-------------|
| `recommendations` | Recommended similar items | movie/tv |
| `similar` | Similar items | movie/tv |
| `collection` | Collection items | movie |
| `next_recommendation` | Next recommended episode | tv |

#### TV Show Specific

| Info Parameter | Description |
|----------------|-------------|
| `seasons` | TV show seasons |
| `flatseasons` | Flat season list |
| `episodes` | Season episodes |
| `specified_episodes` | Specific episodes |
| `anticipated_episodes` | Anticipated episodes |

#### Person Specific

| Info Parameter | Description |
|----------------|-------------|
| `stars_in_movies` | Movies starred in |
| `stars_in_tvshows` | TV shows starred in |
| `stars_in_both` | Both movies and TV |
| `crew_in_movies` | Movies crew worked on |
| `crew_in_tvshows` | TV shows crew worked on |
| `crew_in_both` | Both movies and TV |
| `credits_in_both` | All credits |

#### Trakt Integration (with TMDb ID)

| Info Parameter | Description |
|----------------|-------------|
| `trakt_upnext` | Trakt up next |
| `trakt_related` | Trakt related |
| `trakt_comments` | Trakt comments |
| `trakt_watchers` | Current watchers |

---

## RunScript Methods

RunScript allows executing TMDb Helper functions from other add-ons or skins.

### Basic RunScript Format

```python
RunScript(plugin.video.themoviedb.helper,method_name,param1=value1,param2=value2)
```

### Available Methods

#### Player Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `play` | Play content | `tmdb_id`, `tmdb_type`, `season`, `episode` |
| `play_using` | Play using specific player | Same as play + `player` |

#### Library Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `add_to_library` | Add item to library | `tmdb_id`, `tmdb_type` |
| `library_autoupdate` | Auto-update library | None |
| `refresh_details` | Refresh item details | `tmdb_id`, `tmdb_type` |

#### Trakt Methods

| Method | Description | Parameters | Requires Auth |
|--------|-------------|------------|---------------|
| `sync_trakt` | Sync with Trakt | None | Yes |
| `authenticate_trakt` | Authenticate Trakt | None | No |
| `revoke_trakt` | Revoke Trakt auth | None | Yes |
| `like_list` | Like a Trakt list | `user`, `slug` | Yes |
| `delete_list` | Delete a Trakt list | `user`, `slug` | Yes |
| `rename_list` | Rename a Trakt list | `user`, `slug` | Yes |
| `sort_list` | Sort a Trakt list | `user`, `slug` | Yes |
| `invalidate_trakt_sync` | Invalidate Trakt cache | None | Yes |
| `get_trakt_stats` | Get Trakt statistics | None | Yes |

#### TMDb Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `sync_tmdb` | Sync with TMDb | None |
| `delete_itemtype` | Delete cached item type | `tmdb_type` |

#### Context & Related Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `related_lists` | Show related lists | `tmdb_id`, `tmdb_type` |

#### Utility Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `split_value` | Split a value | `value`, `separator` |
| `kodi_setting` | Get/set Kodi setting | `setting`, `value` |
| `blur_image` | Blur an image | `image` |
| `image_colors` | Extract image colors | `image` |

#### Node/Shortcut Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `make_node` | Create a node/shortcut | Various |
| `remove_node` | Remove a node | `file`, `name` |

#### Modification Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `modify_artwork` | Modify artwork | `tmdb_id`, `tmdb_type` |
| `modify_identifier` | Modify identifier | `tmdb_id`, `tmdb_type` |

#### Settings Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `open_settings` | Open settings dialog | None |
| `provider_allowlist` | Configure provider allowlist | None |
| `customise_players` | Customize players | None |

#### Player Configuration

| Method | Description | Parameters |
|--------|-------------|------------|
| `set_chosenplayer` | Set chosen player | `player` |
| `set_defaultplayer` | Set default player | `player` |
| `update_players` | Update player configs | None |

#### Advanced Methods

| Method | Description | Parameters |
|--------|-------------|------------|
| `add_query` | Add query | Various |
| `add_path` | Add path | `path` |
| `add_tmdb` | Add TMDb item | `tmdb_id`, `tmdb_type` |
| `add_dbid` | Add database ID | `dbid`, `dbtype` |
| `call_id` | Call by ID | Various |
| `call_path` | Call by path | `path` |
| `call_update` | Call update | Various |
| `close_dialog` | Close dialog | None |
| `recache_kodidb` | Recache Kodi DB | None |
| `reset_path` | Reset path | `path` |
| `restart_service` | Restart service | None |
| `monitor_userlist` | Monitor user list | `user`, `slug` |
| `user_list` | User list actions | Various |
| `wikipedia` | Wikipedia lookup | `query` |
| `log_request` | Log request | Various |
| `log_sync` | Log sync | None |

---

## Player Integration

TMDb Helper includes a sophisticated player integration system that allows seamless playback with various players.

### Player Configuration Files

Players are configured via JSON files located in:
- **Bundled:** `resources/players/`
- **User Custom:** `special://profile/addon_data/plugin.video.themoviedb.helper/players/`

### Player JSON Structure

```json
{
    "name": "Player Name",
    "plugin": "plugin.id.of.player",
    "priority": 100,
    "provider": "Provider Name",
    "is_resolvable": "true",
    "assert": {
        "play_movie": ["title", "year"],
        "play_episode": ["showname", "season", "episode"]
    },
    "play_movie": [
        "plugin://plugin.id/path/",
        {"keyboard": "{title}"},
        {"title": "(?i).*{title}.*", "year": "{year}"}
    ],
    "play_episode": [
        "plugin://plugin.id/path/",
        {"keyboard": "{showname}"},
        {"season": "{season}", "episode": "{episode}"}
    ]
}
```

### Available Players (Examples)

- Netflix (`netflix.json`)
- Amazon Prime (`amazon.json`)
- Hulu (`hulu.json`)
- Emby/Jellyfin (`embycon.json`, `jellycon.json`)
- YouTube (`youtube.json`)
- And more...

### Player Variables

Variables available for player templates:

**Movie Variables:**
- `{title}` - Movie title
- `{year}` - Release year
- `{originaltitle}` - Original title
- `{imdb_id}` - IMDb ID
- `{tmdb_id}` - TMDb ID
- `{plot}` - Plot summary
- `{poster}` - Poster URL
- `{fanart}` - Fanart URL

**Episode Variables:**
- `{showname}` / `{tvshowtitle}` - Show title
- `{season}` - Season number
- `{episode}` - Episode number
- `{title}` - Episode title
- `{imdb_id}`, `{tmdb_id}`, `{tvdb_id}` - IDs
- All movie variables

---

## Library Integration

TMDb Helper can create and maintain a Kodi video library with strm files.

### Library STRM Format

TMDb Helper creates STRM files that point to the plugin for playback. The `islocal=True` parameter indicates the content is part of the Kodi library, which affects metadata handling and player selection behavior.

**Movies:**
```
plugin://plugin.video.themoviedb.helper/?info=play&tmdb_id={tmdb_id}&tmdb_type=movie&islocal=True
```

**TV Show Episodes:**
```
plugin://plugin.video.themoviedb.helper/?info=play&tmdb_id={tmdb_id}&tmdb_type=tv&season={season}&episode={episode}&islocal=True
```

The `islocal=True` parameter is recommended for library items as it:
- Enables proper library integration
- Affects watched status synchronization
- Influences player selection priority

### Library Paths

- **Base Directory:** `special://profile/addon_data/plugin.video.themoviedb.helper/`
- **Movies:** `{basedir}/movies/`
- **TV Shows:** `{basedir}/tvshows/`

### Library Auto-Update

Trigger library auto-update:
```python
RunScript(plugin.video.themoviedb.helper,library_autoupdate)
```

Or via cron (configured in settings).

---

## API Integrations

TMDb Helper integrates with multiple external APIs:

### TMDb (The Movie Database)

**Module:** `tmdbhelper.lib.api.tmdb.api`

**Features:**
- Movie/TV show/person details
- Search functionality
- Discover filters
- Images (posters, fanart, logos)
- Videos (trailers, clips)
- Recommendations & similar content
- Collections
- Keywords
- Reviews

**API Base:** `https://api.themoviedb.org/3`

### Trakt

**Module:** `tmdbhelper.lib.api.trakt.api`

**Features:**
- User authentication (OAuth)
- Watchlist, collection, history
- Progress tracking
- Recommendations
- Trending, popular lists
- User custom lists
- Calendar
- Statistics
- Sync functionality

**API Base:** `https://api.trakt.tv/`

### TVDb (TheTVDB)

**Module:** `tmdbhelper.lib.api.tvdb.api`

**Features:**
- Additional TV show metadata
- Episode information
- Awards data (TVDb subscription required)
- Genre classifications
- Extended episode details (TVDb subscription may be required for some data)

**Note:** Some advanced features and higher API rate limits require a TVDb subscription. Basic metadata is available without subscription. See https://thetvdb.com/subscribe for details.

### OMDb (Open Movie Database)

**Module:** `tmdbhelper.lib.api.omdb.api`

**Features:**
- IMDb ratings
- Rotten Tomatoes scores
- Additional movie metadata

### FanartTV

**Module:** `tmdbhelper.lib.api.fanarttv.api`

**Features:**
- High-quality artwork
- ClearLogos
- ClearArt
- Character Art
- Additional fanart

### MDbList

**Module:** `tmdbhelper.lib.api.mdblist.api`

**Features:**
- Curated lists
- List search
- User lists

### Gemini AI

**Module:** `tmdbhelper.lib.api.gemini.api`

**Features:**
- AI-powered content search
- Natural language queries

---

## Context Menu Integration

TMDb Helper provides context menu items for other add-ons to use.

### Standard Context Menu Items

```python
# Related lists
'command': 'RunScript(plugin.video.themoviedb.helper,related_lists,tmdb_id={tmdb_id},tmdb_type={tmdb_type})'

# Sync with Trakt
'command': 'RunScript(plugin.video.themoviedb.helper,sync_trakt)'

# Sync with TMDb
'command': 'RunScript(plugin.video.themoviedb.helper,sync_tmdb)'

# Refresh details
'command': 'RunScript(plugin.video.themoviedb.helper,refresh_details,tmdb_id={tmdb_id},tmdb_type={tmdb_type})'

# Modify artwork
'command': 'RunScript(plugin.video.themoviedb.helper,modify_artwork,tmdb_id={tmdb_id},tmdb_type={tmdb_type})'

# Add to library
'command': 'RunScript(plugin.video.themoviedb.helper,add_to_library,tmdb_id={tmdb_id},tmdb_type={tmdb_type})'
```

---

## Practical Examples

### Example 1: Play a Movie

```python
# Simple play
xbmc.executebuiltin('RunPlugin(plugin://plugin.video.themoviedb.helper/?info=play&tmdb_id=550&tmdb_type=movie)')

# Or using RunScript
xbmc.executebuiltin('RunScript(plugin.video.themoviedb.helper,play,tmdb_id=550,tmdb_type=movie)')
```

### Example 2: Play a TV Show Episode

```python
# Season 1, Episode 1
xbmc.executebuiltin('RunPlugin(plugin://plugin.video.themoviedb.helper/?info=play&tmdb_id=1399&tmdb_type=tv&season=1&episode=1)')
```

### Example 3: Browse Popular Movies

```python
# Navigate to popular movies
xbmc.executebuiltin('Container.Update(plugin://plugin.video.themoviedb.helper/?info=popular&tmdb_type=movie)')
```

### Example 4: Search for Content

```python
# Search for "Breaking Bad"
import urllib.parse
query = urllib.parse.quote_plus("Breaking Bad")
xbmc.executebuiltin(f'Container.Update(plugin://plugin.video.themoviedb.helper/?info=search&query={query}&type=tv)')
```

### Example 5: Show Movie Details

```python
# Show Fight Club details
xbmc.executebuiltin('Container.Update(plugin://plugin.video.themoviedb.helper/?info=details&tmdb_id=550&tmdb_type=movie)')
```

### Example 6: Browse Trakt Watchlist

```python
# Must have Trakt authenticated
xbmc.executebuiltin('Container.Update(plugin://plugin.video.themoviedb.helper/?info=trakt_watchlist&tmdb_type=movie)')
```

### Example 7: Discover Movies by Genre

```python
# Action movies, sorted by popularity
xbmc.executebuiltin('Container.Update(plugin://plugin.video.themoviedb.helper/?info=discover&tmdb_type=movie&with_genres=28&sort_by=popularity.desc)')
```

### Example 8: Get Movie Recommendations

```python
# Get recommendations for Fight Club
xbmc.executebuiltin('Container.Update(plugin://plugin.video.themoviedb.helper/?info=recommendations&tmdb_id=550&tmdb_type=movie)')
```

### Example 9: Browse TV Show Seasons

```python
# Browse Game of Thrones seasons
xbmc.executebuiltin('Container.Update(plugin://plugin.video.themoviedb.helper/?info=seasons&tmdb_id=1399&tmdb_type=tv)')
```

### Example 10: Add to Kodi Library

```python
# Add movie to library
xbmc.executebuiltin('RunScript(plugin.video.themoviedb.helper,add_to_library,tmdb_id=550,tmdb_type=movie)')
```

### Example 11: Create Custom Widget

```python
# Widget for trending movies (auto-reload)
url = 'plugin://plugin.video.themoviedb.helper/?info=trending_week&tmdb_type=movie&widget=true'
```

### Example 12: Use in Skin Shortcuts

```xml
<!-- Add to skin shortcuts -->
<item>
    <label>Popular Movies</label>
    <onclick>ActivateWindow(Videos,plugin://plugin.video.themoviedb.helper/?info=popular&tmdb_type=movie,return)</onclick>
    <icon>DefaultMovies.png</icon>
</item>
```

### Example 13: Python Integration

```python
import xbmcaddon
import xbmcgui
import urllib.parse

# Check if TMDb Helper is installed
try:
    tmdb_helper = xbmcaddon.Addon('plugin.video.themoviedb.helper')
    
    # Build URL
    params = {
        'info': 'details',
        'tmdb_id': '550',
        'tmdb_type': 'movie'
    }
    url = 'plugin://plugin.video.themoviedb.helper/?' + urllib.parse.urlencode(params)
    
    # Execute
    xbmc.executebuiltin(f'Container.Update({url})')
    
except RuntimeError:
    xbmcgui.Dialog().notification('Error', 'TMDb Helper not installed', xbmcgui.NOTIFICATION_ERROR)
```

### Example 14: Batch Operations

```python
# Get multiple types of info for an item
import xbmc

tmdb_id = '550'
tmdb_type = 'movie'

# Show details
xbmc.executebuiltin(f'RunScript(plugin.video.themoviedb.helper,related_lists,tmdb_id={tmdb_id},tmdb_type={tmdb_type})')
```

### Example 15: Using Variables in Skins

```xml
<!-- In skin XML -->
<control type="button">
    <label>Play with TMDb Helper</label>
    <onclick>RunScript(plugin.video.themoviedb.helper,play,tmdb_id=$INFO[ListItem.Property(tmdb_id)],tmdb_type=$INFO[ListItem.Property(tmdb_type)])</onclick>
</control>
```

---

## Advanced Integration Topics

### Widget Support

Add `widget=true` parameter for home screen widgets:
```
plugin://plugin.video.themoviedb.helper/?info=popular&tmdb_type=movie&widget=true
```

This enables auto-reload functionality for dynamic content updates.

### Forced Reload

For real-time updates, use forced reload:
```
plugin://plugin.video.themoviedb.helper/?info=trakt_ondeck&tmdb_type=tv&reload=forced
```

### Using with Container.Update

```python
# Replace current container
xbmc.executebuiltin('Container.Update(plugin://plugin.video.themoviedb.helper/?info=popular&tmdb_type=movie)')

# Open in new window
xbmc.executebuiltin('ActivateWindow(Videos,plugin://plugin.video.themoviedb.helper/?info=popular&tmdb_type=movie,return)')
```

### Multiple Path Support

For complex queries, use paths parameters:
```
plugin://plugin.video.themoviedb.helper/?info=details&paths_0=value1&paths_1=value2
```

Or use the `&&` separator:
```
plugin://plugin.video.themoviedb.helper/?info=play&tmdb_id=550&&paths_0=custom
```

---

## Common Parameters Reference

### Discovery Filters (for discover routes)

| Parameter | Description | Example |
|-----------|-------------|---------|
| `with_genres` | Genre IDs (comma-separated) | `with_genres=28,12` |
| `without_genres` | Exclude genres | `without_genres=27` |
| `with_cast` | Cast member ID | `with_cast=500` |
| `with_crew` | Crew member ID | `with_crew=7879` |
| `with_companies` | Production company ID | `with_companies=420` |
| `with_keywords` | Keyword IDs | `with_keywords=9715` |
| `year` | Release year | `year=2020` |
| `primary_release_year` | Primary release year | `primary_release_year=2020` |
| `vote_average.gte` | Minimum rating | `vote_average.gte=7` |
| `vote_average.lte` | Maximum rating | `vote_average.lte=9` |
| `with_runtime.gte` | Minimum runtime (minutes) | `with_runtime.gte=90` |
| `with_runtime.lte` | Maximum runtime (minutes) | `with_runtime.lte=120` |
| `sort_by` | Sort order | `sort_by=popularity.desc` |
| `region` | Region code | `region=US` |
| `with_original_language` | Original language | `with_original_language=en` |

### Sort Options

- `popularity.desc` / `popularity.asc`
- `vote_average.desc` / `vote_average.asc`
- `primary_release_date.desc` / `primary_release_date.asc`
- `revenue.desc` / `revenue.asc`
- `title.asc` / `title.desc`

---

## Error Handling

Always check if TMDb Helper is installed before attempting to use it:

```python
import xbmcaddon

def is_tmdb_helper_installed():
    try:
        xbmcaddon.Addon('plugin.video.themoviedb.helper')
        return True
    except RuntimeError:
        return False

if not is_tmdb_helper_installed():
    xbmcgui.Dialog().notification(
        'TMDb Helper Required',
        'Please install TMDb Helper from jurialmunkey repository',
        xbmcgui.NOTIFICATION_WARNING,
        5000
    )
```

---

## Performance Considerations

1. **Caching:** TMDb Helper caches API responses. Default cache duration is 30 days for static content, 7 days for dynamic lists.

2. **Pagination:** Use `page` parameter to load content in chunks rather than all at once.

3. **Widget Optimization:** When using in widgets, use `widget=true` parameter to enable smart caching.

4. **Rate Limiting:** TMDb Helper respects API rate limits. Excessive rapid requests may be throttled.

---

## Support & Resources

- **GitHub Repository:** https://github.com/jurialmunkey/plugin.video.themoviedb.helper
- **Wiki:** https://github.com/jurialmunkey/plugin.video.themoviedb.helper/wiki
- **Forum:** https://forum.kodi.tv/showthread.php?tid=345847
- **Issues:** https://github.com/jurialmunkey/plugin.video.themoviedb.helper/issues

---

## License

TMDb Helper is licensed under GPL-3.0-or-later.

When integrating TMDb Helper into your project:
- Respect the GPL license terms
- Credit the original author (jurialmunkey)
- Link to the original repository

---

## Data Attribution

When using TMDb Helper, you are accessing data from multiple sources:

- **TMDb:** https://www.themoviedb.org/
- **Trakt:** https://trakt.tv/
- **TVDb:** https://thetvdb.com/ (Consider supporting: https://thetvdb.com/subscribe)
- **OMDb:** http://www.omdbapi.com/
- **FanartTV:** https://fanart.tv/
- **MDbList:** https://mdblist.com/

Please respect their terms of service and consider supporting these services.

---

**Last Updated:** 2025-12-14  
**Plugin Version:** 6.12.1+  
**Document Version:** 1.0
