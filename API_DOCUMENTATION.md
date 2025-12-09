# DJ Brain API Documentation

The DJ Brain backend provides a RESTful API for interacting with the jukebox system. All endpoints are prefixed with `/api`.

## General

- **Base URL:** `http://<your-server-ip>:8080`
- **Response Format:** All responses are in JSON format.

---

## Endpoints

### `GET /api/search`

Searches the Mopidy music library.

**Query Parameters:**
- `q` (string, required): The search term.

**Example Request:**
`GET /api/search?q=Daft+Punk`

**Example Success Response (200 OK):**
```json
[
  {
    "uri": "local:track:Music/Daft%20Punk/Discovery/01%20One%20More%20Time.mp3",
    "title": "One More Time",
    "artist": "Daft Punk",
    "album": "Discovery"
  }
]
```

---

### `POST /api/request`

Adds a new song request to the database queue.

*... (Details to be added for this endpoint) ...*