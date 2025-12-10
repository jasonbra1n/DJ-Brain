# DJ Brain 🧠

**DJ Brain is an open-source, self-hosted, AI-powered jukebox system.**

It allows DIY enthusiasts and venue operators to run their own jukebox using a private music library. The system is designed to be modular, with a web-based frontend for song requests, a robust backend for managing the queue, and an intelligent "AI DJ" to ensure the music never stops.

## ✨ Key Features

- **Self-Hosted:** Run it on your own hardware (Raspberry Pi 4B, mini-PC, NAS) with Docker.
- **BYOM (Bring Your Own Music):** Works with your local music library.
- **Web-Based Request System:** A touch-friendly interface for users to search and request songs.
- **Intelligent Queue Management:** An AI core that can intelligently select music to fill gaps, keeping the vibe going.
- **Multi-Room Audio:** Send music to different network audio players (e.g., Raspberry Pi).
- **Open Source & Extensible:** Built with common technologies (PHP, Python, JS) and licensed under MIT for maximum freedom.

## 🚀 Quick Start

1.  **Prerequisites:** Ensure you have `Docker` and `Docker Compose` installed on your server.
2.  **Clone the repository:**
    ```bash
    git clone https://github.com/jasonbra1n/DJ-Brain.git
    cd DJ-Brain
    ```
3.  **Configure:**
    -   Copy the example environment file: `cp .env.example .env`
    -   Edit the `.env` file to set your database passwords and music library path.
4.  **Launch:**
    ```bash
    docker-compose up -d
    ```
5.  **Access:**
    -   **Jukebox Frontend:** `http://<your-server-ip>:8080`
    -   **Mopidy Web UI (for admin):** `http://<your-server-ip>:6680`

## 📚 Documentation

- **[User Manual](./docs/index.md):** A complete guide for installation and operation.
- **Project Plan:** The high-level project roadmap.
- **Developer Guide:** Instructions for setting up the development environment and contributing.
- **API Documentation:** Details on the custom backend API endpoints.
- **Architecture Overview:** A high-level look at how the services interact.
