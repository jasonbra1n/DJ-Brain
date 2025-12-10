# 2. Configuration

This guide covers the primary configuration options for DJ Brain, from managing your music library to setting up advanced features.

## Core Configuration (`.env` file)

All essential system settings are managed in the `.env` file, located in the root of your `DJ-Brain` directory. A detailed explanation of each variable can be found in the comments of the `.env.example` file.

The most critical setting is `MUSIC_PATH`. This must be the absolute path to your music collection on the host machine. If your music is not loading, an incorrect path in this variable is the most likely cause.

## Music Library Management

### Initial Scan

As detailed in the installation guide, Mopidy performs a full scan of your music library on its first startup. This process runs in the background and can take a significant amount of time depending on the size of your collection.

### Forcing a Rescan

If you add, remove, or modify a large number of files in your music library, you may need to manually trigger a new scan.

1.  Connect to your device via SSH.
2.  Navigate to the `DJ-Brain` directory.
3.  Run the following command:
    ```bash
    docker-compose exec mopidy mopidy local scan
    ```
    This command tells the running Mopidy container to start a new scan of the local media.

---

## Network Audio Players (Future Feature)

As described in the **Architecture** document, DJ Brain is designed to support a central server with multiple remote audio players (e.g., Raspberry Pis running lightweight client software). This allows for synchronized, multi-room audio playback managed from a single library.

**This feature is planned for a future release (Phase 2).**

The setup will involve:
1.  Enabling Mopidy's audio streaming output.
2.  Configuring one or more remote devices to connect to Mopidy's stream.

Detailed instructions will be added to this section as the feature is developed.

---

## Advanced Mopidy Configuration

The default DJ Brain setup uses a standard configuration for Mopidy. However, Mopidy is a highly extensible platform, and you may wish to enable additional backends (e.g., for Spotify, SoundCloud, etc.) or customize its behavior.

Mopidy's configuration file is located at `config/mopidy/mopidy.conf`. Any changes made to this file will be persistent.

**Note:** Modifying `mopidy.conf` is an advanced topic. Refer to the [official Mopidy documentation](https://docs.mopidy.com/) for a full list of configuration options. Incorrect settings in this file can prevent the Mopidy service from starting.
