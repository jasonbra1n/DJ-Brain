# DJ Brain: The Open-Source AI-Powered Jukebox System

## 🎯 Project Goal

To create an open-source, self-hosted, AI-managed digital jukebox system capable of playing local music (from a Synology NAS) via network endpoints (Raspberry Pi/tablets), managing a request queue, and incorporating a contextual AI DJ for continuous music flow. This system aims to replicate and enhance the functionality of a professional music programmer/DJ.

## 💻 Tech Stack

| Component | Technology | Rationale |
| :--- | :--- | :--- |
| **Music Server** | Mopidy (via Docker on Synology DS220+) | Centralized music playback with robust JSON-RPC API for external control. |
| **Database** | MySQL/MariaDB | Familiar technology (SQL) for storing request history, user data, and payment logs. |
| **Jukebox Backend** | PHP (Framework like Slim/FastAPI recommended) | Handles API calls from the Frontend, interacts with Mopidy's API, and manages the SQL database. |
| **Jukebox Frontend** | Vanilla JavaScript/HTML/CSS | Touch-friendly web interface for tablets and phones (responsive design). |
| **AI DJ Engine** | Python/Open-Source LLM (Running in Docker) | For queue monitoring, contextual music suggestion, and flow control—the "Brain." |
| **Hardware** | Synology DS220+, Raspberry Pi (Endpoints), Touchscreen Tablet/Monitor. | Leveraging existing infrastructure. |

## 🗺️ Project Roadmap (Phased Approach)

### Phase 1: Foundation and Backend Integration (Focus: API & Request Handling)

**Goal:** Establish the music server, database, and a basic API layer using PHP and SQL.

* [ ] **1.1 Mopidy Setup:** Install Mopidy and Mopidy-Iris (web UI) via Docker on the Synology DS220+. Configure access to the NAS music library (`/volume1/Music`).
* [ ] **1.2 Database Setup:** Set up a MySQL/MariaDB container and define the initial schema (`requests` table with fields: `track_id`, `user_id`, `timestamp`, `status`).
* [ ] **1.3 PHP Jukebox Backend (MVP):** Create a PHP API (e.g., using Slim) to handle core endpoints:
    * `/api/search?q={term}`: Calls Mopidy's API to search the music library.
    * `/api/request`: Accepts a `track_id` and adds the request to the SQL database.
    * `/api/queue_mopidy`: Endpoint for the AI DJ to call Mopidy's `add` function.
* [ ] **1.4 Basic Frontend (JS):** Create a simple HTML/JS page that allows searching and submitting a request (stored in SQL, not yet playing).

### Phase 2: The "Brain" Activation and Multi-Room (Focus: Flow & Endpoints)

**Goal:** Implement the core AI flow-control logic and set up the first music zone.

* [ ] **2.1 AI DJ Engine Setup:** Create a separate Python script (the **DJ Brain**).
* [ ] **2.2 Core Logic:** Implement the central **`Brain_Loop`** which continuously:
    1.  Reads Mopidy's current queue status.
    2.  Reads the most recent **Pending** requests from the SQL database.
    3.  If the Mopidy queue is low, determines the best next song (either a user request or an AI-chosen track).
    4.  Calls the **Mopidy API** to inject the chosen track and updates the request status in the SQL database to **Played**.
* [ ] **2.3 Contextual Logic (MVP):** Introduce the first flow rule: AI selection should prioritize the same **BPM/Mood** as the last three tracks.
* [ ] **2.4 Raspberry Pi Endpoint:** Set up the first Raspberry Pi running a client (like Squeezelite) to act as an audio renderer, controlled by the Mopidy server.

### Phase 3: Monetization and Polish (Focus: Stability & Features)

**Goal:** Integrate payments, implement anti-spam logic, and finalize the UI.

* [ ] **3.1 Priority Request Logic:** Implement logic in the `Brain_Loop` to allow requests marked as **Paid** (a new SQL field) to jump ahead of general requests.
* [ ] **3.2 Payment Integration:** Integrate Stripe/PayPal into the PHP Backend to process payments for priority requests.
* [ ] **3.3 Cooldown and Anti-Spam:** Implement user-based cooldowns and vote-to-skip features in the PHP backend/SQL log.
* [ ] **3.4 Front-End Refinement:** Finalize the responsive, touch-friendly UI for tablets, including queue visibility and user history.

---

That plan looks solid! Where would you like to begin? We can start with a search for:

1.  **Mopidy and Synology Docker Setup** (Phase 1.1)
2.  **Mopidy API documentation** (needed for Phase 1.3 and 2.1)
3.  **PHP framework for simple APIs** (Phase 1.3)
