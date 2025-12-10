# DJ Brain: The Open-Source AI-Powered Jukebox System

## 🎯 Project Goal

To create an open-source, self-hosted, AI-managed digital jukebox system capable of playing local music (from a network-attached storage or mounted folder) via network endpoints (Raspberry Pi/tablets), managing a request queue, and incorporating a contextual AI DJ for continuous music flow.

## 📚 Key Documents

This plan should be read in conjunction with the other core project documents:

- **[README.md](./README.md):** Project overview and quick start guide.
- **[Architecture Overview](./ARCHITECTURE.md):** The high-level technical design and strategy.
- **[Contributing Guide](./CONTRIBUTING.md):** Instructions for setting up the development environment.
- **[API Documentation](./API_DOCUMENTATION.md):** The contract for the backend API.
- **[User Manual](./docs/index.md):** The end-user guide for installation and operation.

##  Tech Stack (Generic Open-Source Focus)

| Component | Technology | Rationale |
| :--- | :--- | :--- |
| **Server Host** | Generic Home Server with Docker | Can run on hardware as lightweight as a Raspberry Pi 4B for core jukebox features. A more powerful machine is required for the optional AI DJ Engine. |
| **Music Server** | Mopidy (via Docker) | Centralized music playback with robust JSON-RPC API for external control. |
| **Database** | MariaDB (via Docker) | A common, powerful, and open-source SQL database. |
| **Jukebox Backend** | PHP (Framework like Slim recommended) | Handles API calls from the Frontend, interfaces with Mopidy and MariaDB. |
| **Jukebox Frontend** | Vanilla JavaScript/HTML/CSS | Touch-friendly web interface for tablets and phones (responsive design). |
| **AI DJ Engine** | Python/Open-Source LLM (Running in Docker) | The "Brain" for queue monitoring. **This is the most resource-intensive component and can be considered an optional "plus" feature for users with capable hardware.** |
| **Hardware Endpoints**| Raspberry Pi / Squeezelite Clients | Cost-effective, low-power audio *renderers*. They receive audio from the server, not run the whole system. |

## 🏗️ Docker Compose Blueprint (`docker-compose.yml`)

This file will simultaneously launch your music server, database, and the environment for your PHP backend.

```yaml
version: "3.7"
services:
  # --- MOPIDY MUSIC SERVER ---
  mopidy:
    image: rawdlite/mopidy  # A good starting image for Mopidy with extensions
    container_name: djbrain_mopidy
    restart: unless-stopped
    ports:
      - "6680:6680" # HTTP/Websockets (for Frontend and APIs)
      - "6600:6600" # MPD (Music Player Daemon)
    volumes:
      # CRITICAL: Mount your music folder (path is set in the .env file)
      - ${MUSIC_PATH}:/data/music:ro
      # For Mopidy config and local data (e.g., library scan cache)
      - ./config/mopidy:/config
      - ./data/mopidy:/data
    # To play audio directly from the server's sound card (All-in-One setup),
    # uncomment the following line (Linux hosts only).
    # devices:
    #   - /dev/snd:/dev/snd

  # --- MARIADB DATABASE ---
  mariadb:
    image: mariadb:latest
    container_name: djbrain_mariadb
    restart: unless-stopped
    # Uncomment the port mapping below only if you need to access the database
    # directly from your host machine (outside of the Docker network).
    # ports:
    #   - "3306:3306"
    volumes:
      # Persistence for database files
      - ./data/mariadb:/var/lib/mysql
    # Environment variables are loaded from the .env file for security
    environment:
      - MYSQL_ROOT_PASSWORD=${DB_ROOT_PASSWORD}
      - MYSQL_DATABASE=${DB_DATABASE}
      - MYSQL_USER=${DB_USER}
      - MYSQL_PASSWORD=${DB_PASSWORD}

  # --- PHP API BACKEND (Phase 1.3 Target) ---
  php_backend:
    image: php:8.2-apache # A common PHP environment for Slim framework
    container_name: djbrain_api
    restart: unless-stopped
    volumes:
      # Mount the API and Frontend source code
      - ./src/api:/var/www/html
      - ./src/frontend:/var/www/html/frontend
    ports:
      - "8080:80" # Expose the web server
    depends_on:
      - mopidy
      - mariadb

  # --- PYTHON AI BRAIN (Phase 2.1 Target) ---
  # brain:
  #   build: ./src/brain
  #   container_name: djbrain_brain
  #   restart: unless-stopped
  #   depends_on:
  #     - mopidy
  #     - mariadb
```

## 🗺️ Project Roadmap (Phased Approach)

### Phase 0: Documentation & Scaffolding

* [x] **0.1 Project Definition:** Create the core `README.md`, `ARCHITECTURE.md`, `CONTRIBUTING.md`, and `project_plan.md` documents.
* [x] **0.2 User Manual:** Create the initial `docs/` structure and fill out the Installation, Configuration, Administration, and Troubleshooting guides.
* [x] **0.3 Configuration Scaffolding:** Create the `.env.example` file to establish a clear and secure configuration pattern.
* [x] **0.4 Docker Blueprint:** Refine the `docker-compose.yml` blueprint in the project plan to use environment variables.

### Phase 1: Foundation and Backend Integration (Focus: API & Request Handling)

* [ ] **1.1 Mopidy Setup:** Install Mopidy via Docker. Configure a volume mount for the local music library.
* [ ] **1.2 Database Schema Definition:** Set up the MariaDB container and define the initial `requests` table. A file `database/schema.sql` should be created with the following content:
  ```sql
  CREATE TABLE requests (
      id INT AUTO_INCREMENT PRIMARY KEY,
      track_uri VARCHAR(255) NOT NULL,
      user_identifier VARCHAR(255),
      requested_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
      status ENUM('pending', 'playing', 'played', 'error') NOT NULL DEFAULT 'pending'
  );
  ```
* [ ] **1.3 PHP Jukebox Backend (MVP):** Create a PHP API using the Slim Framework to handle core endpoints:
    *   `GET /api/search?q={term}`: Calls Mopidy's API to search the music library.
    *   `POST /api/request`: Accepts a `track_uri` and adds the request to the `requests` table in the database.
* [ ] **1.4 Basic Frontend (MVP):** Create a simple, functional HTML/JS page (no styling required) that includes:
    *   A text input field for search queries.
    *   A "Search" button.
    *   A list/area to display search results returned from the API.
    *   A "Request" button next to each search result.
* [ ] **1.5 API Unit Tests:** Create basic unit tests for the backend API to verify that the `/search` and `/request` endpoints function correctly.

### Phase 2: The "Brain" Activation and Multi-Room (Focus: Flow & Endpoints)

* [ ] **2.1 AI DJ Engine Setup:** Create a separate Python script (the **DJ Brain**) in a Docker container.
* [ ] **2.2 Core Logic:** Implement the central **`Brain_Loop`** which continuously:
    1.  Reads Mopidy's current queue status.
    2.  Reads the most recent **Pending** requests from the MariaDB database.
    3.  If the Mopidy queue is low, determines the best next song (either a user request or an AI-chosen track).
    4.  Calls the **Mopidy API** to inject the chosen track and updates the request status in the MariaDB to **Played**.
* [ ] **2.3 Contextual Logic (MVP):** Introduce the first flow rule: AI selection should prioritize the same **BPM/Mood** as the last three tracks.
* [ ] **2.4 Raspberry Pi Endpoint:** Set up the first Raspberry Pi running a client (like Squeezelite) to act as an audio renderer, controlled by the Mopidy server.

### Phase 3: Monetization and Polish (Focus: Stability & Features)

* [ ] **3.1 Priority Request Logic:** Implement logic in the `Brain_Loop` to allow requests marked as **Paid** (a new SQL field) to jump ahead of general requests.
* [ ] **3.2 Payment Integration:** Integrate Stripe/PayPal into the PHP Backend for priority requests.
* [ ] **3.3 Cooldown and Anti-Spam:** Implement user-based cooldowns and vote-to-skip features in the PHP backend/SQL log.
* [ ] **3.4 Front-End Refinement:** Finalize the responsive, touch-friendly UI for tablets and implement a basic theming structure (e.g., CSS variables for colors).
* [ ] **3.5 Admin Dashboard (MVP):** Create a basic admin page to view request history and manage simple settings.

---