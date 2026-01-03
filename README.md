# playlist_to_album

Convert Roon playlist exports into album-like structures with properly sequenced tracks and optimized metadata.

## Overview

`playlist_to_album` is a Python script that transforms Roon-exported playlists into clean, album-style folders. It processes FLAC files by renaming them according to their playlist order, updating track metadata, and resizing embedded artwork for optimal storage efficiency.

### What It Does

When you export a playlist from Roon to local media, you get a complex folder structure with tracks organized by artist and album. This script:

1. **Reads the playlist order** from the `.m3u` file in the `Playlists` folder
2. **Renames tracks** to reflect their playlist sequence: `01 - Artist Name - Track Title.flac`
3. **Updates FLAC metadata**:
   - Sets track numbers to `x/y` format (e.g., `1/21`, `2/21`)
   - Sets disc to `1/1`
4. **Optimizes embedded artwork**:
   - Resizes to max 800px on the longest dimension
   - Maintains aspect ratio
   - Only downscales (never upscales)
5. **Optionally cleans up** the original artist/album folder structure

## Installation

### Prerequisites

- Python 3.6 or later
- pip (Python package manager)

### Install Dependencies

```bash
pip install mutagen Pillow
```

### Install Script

1. Download `playlist_to_album` from this repository
2. Make it executable:

```bash
chmod +x playlist_to_album
```

3. Optionally, move it to your PATH:

```bash
# Example: move to /usr/local/bin
sudo mv playlist_to_album /usr/local/bin/
```

## Usage

### Basic Syntax

```bash
playlist_to_album <folder> [mode]
```

### Modes

| Mode | Description |
|------|-------------|
| `full` (default) | Convert tracks and delete original folder structure |
| `convert_only` | Convert tracks but keep original folder structure |
| `cleanup_only` | Only delete original folder structure (no conversion) |

### Examples

**Default mode (full conversion and cleanup):**
```bash
playlist_to_album ~/Music/MyPlaylist
```

**Convert only (preserve original structure):**
```bash
playlist_to_album ~/Music/MyPlaylist convert_only
```

**Cleanup only (remove original folders without processing):**
```bash
playlist_to_album ~/Music/MyPlaylist cleanup_only
```

## Expected Folder Structure

### Input (Roon Export)

```
MyPlaylist/
├── Playlists/
│   └── MyPlaylist.m3u
├── Artist 1/
│   └── Album 1/
│       ├── 1-01 Track.flac
│       └── 1-02 Track.flac
└── Artist 2/
    └── Album 2/
        └── 1-01 Track.flac
```

### Output (After Processing)

```
MyPlaylist/
├── 01 - Artist 1 - First Track.flac
├── 02 - Artist 1 - Second Track.flac
└── 03 - Artist 2 - Third Track.flac
```

## Features

### Metadata Handling

- **Track Numbers**: Updated to reflect playlist position (e.g., `1/21`, `2/21`, etc.)
- **Disc Numbers**: Always set to `1/1`
- **Artist & Title**: Preserved from original FLAC metadata
- **Other Tags**: All other metadata remains unchanged

### Artwork Optimization

- Automatically resizes embedded album artwork
- Maximum dimension: 800 pixels
- Maintains original aspect ratio
- Only downscales (smaller images are preserved as-is)
- Supports all common image formats embedded in FLAC files

### Error Handling

- **Missing files**: Script stops with clear error message if M3U references non-existent files
- **Missing M3U**: Reports error if no `.m3u` file found in `Playlists` folder
- **Orphaned files**: Warns about FLAC files not referenced in the playlist
- **Invalid paths**: Validates folder existence before processing

### Logging

The script provides detailed console output:
- Current processing mode
- Number of tracks found
- Progress for each track
- Artwork resizing status
- Warnings for unreferenced files
- Completion status

## Example Output

```
[FULL] Starting playlist_to_album processing
[FULL] Root directory: /Users/leon/Music/Jazz_Fusion_Mix
[FULL] Parsing M3U: Jazz_Fusion_Mix.m3u
[FULL] Found 21 tracks in playlist
[FULL] Processing 21 tracks...
[FULL] Processing track 1/21: Keep That Same Old Feeling
[FULL]   Resizing artwork for track 1
[FULL] Processing track 2/21: Westchester Lady
[FULL]   Resizing artwork for track 2
...
[FULL] Cleaning up input data...
[FULL]   Removing: Various Artists
[FULL]   Removing: Bob James
[FULL]   Removing: Playlists
[FULL] Cleanup complete
[FULL] Processing complete
```

## Warnings

If the script encounters FLAC files in the artist/album structure that aren't referenced in the playlist, it will display warnings at the end:

```
============================================================
WARNINGS:
  Various Artists/Compilation/Bonus Track.flac
============================================================
```

## Use Cases

### Portable Music Players

Perfect for high-end portable audio players (e.g., Astell&Kern) where you want curated playlists presented as cohesive albums with optimized artwork for storage efficiency.

### Music Organization

Create themed compilations from your Roon library with proper track sequencing and clean filenames.

### Backup & Archive

Convert playlists to standalone, self-contained folders that are easy to archive or share.

## Technical Details

- **Language**: Python 3
- **Dependencies**: 
  - `mutagen` - FLAC metadata manipulation
  - `Pillow` - Image processing
- **Compatibility**: macOS, Linux (any Unix-like system with Python 3)
- **File Format**: FLAC only

## Limitations

- Only processes FLAC files (not MP3, AAC, etc.)
- Assumes single playlist per folder (uses first `.m3u` if multiple exist)
- Requires all files referenced in M3U to be present

## Contributing

Issues and pull requests welcome! Please ensure any code changes maintain backwards compatibility with the existing command-line interface.

## License

MIT License - feel free to use and modify as needed.

## Author

Created for managing high-quality audio playlists exported from Roon Music Server.
