# 🎬 YouTube Playlist Indexer & Semantic Search Engine

<div align="center">

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![YouTube API](https://img.shields.io/badge/YouTube_Data_API-v3-FF0000?style=for-the-badge&logo=youtube&logoColor=white)
![Semantic Search](https://img.shields.io/badge/Search-Semantic-8A2BE2?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-success?style=for-the-badge)

### 🚀 Transform YouTube playlists into a searchable personal knowledge base

</div>

---

## 🎯 Problem Statement

Curating YouTube playlists is a fantastic way to organize learning resources, tutorials, podcasts, interviews, and research material.

However, once your collection grows, two major friction points emerge.

---

## 🚀 Origin Story

For a long time, I, **Vivek Sahay**, have relied heavily on YouTube playlists to organize learning material, engineering content, startup discussions, research videos, and technical tutorials.

As those playlists grew into hundreds of videos, I realized that saving information was easy, but retrieving it later was surprisingly difficult.

The same issues kept appearing:

- Valuable videos getting buried under newer additions
- Hours spent scrolling through playlists
- Inability to search by ideas or spoken discussions
- Friction when sharing saved content with others

This project is my attempt to solve that problem by transforming YouTube playlists into a searchable knowledge system powered by metadata, transcripts, and semantic search.

The vision is simple:

**Save once. Search forever.**

### 🔄 Context Switching

Sharing a saved video usually requires:

```text
WhatsApp / Slack
       ↓
Open YouTube
       ↓
Find Playlist
       ↓
Locate Video
       ↓
Copy Link
       ↓
Return to Chat
```

This workflow becomes repetitive and inefficient.

---

### 🔍 Discovery Bottleneck

As playlists scale beyond 100+ videos:

- Finding a specific explanation becomes difficult
- Searching for spoken content is impossible
- Valuable videos get buried
- Manual scrolling becomes the default search engine

> [!TIP]
> Playlists are excellent for storage but poor for retrieval.

---

# 🏗️ System Architecture

## ⚡ Option A: No-Code Automation

### Fastest Setup

```text
YouTube Playlist
        │
        ▼
 New Video Added
        │
        ▼
 Zapier / Make.com
        │
        ▼
 Notion / Sheets
```

### Benefits

✅ Zero coding

✅ Quick deployment

✅ Searchable database

✅ Cloud automation

---

## 🛠️ Option B: Semantic Search Engine

### Engineering-Focused Solution

Search videos by:

- Concepts
- Topics
- Spoken words
- Natural language queries

instead of relying on titles alone.

```text
YouTube API
     │
     ▼
Fetch Metadata
     │
     ▼
Extract Transcripts
     │
     ▼
Generate Embeddings
     │
     ▼
Store Vectors
     │
     ▼
Semantic Search
```

---

## 💡 Example Queries

```text
"Video explaining CAP theorem"

"Redis caching tutorial"

"Podcast discussing startup distribution"

"Rust ownership explained"
```

---

# 📋 Prerequisites

| Step | Task |
|--------|--------|
| 1️⃣ | Install Google API Client |
| 2️⃣ | Enable YouTube Data API v3 |
| 3️⃣ | Generate API Key |
| 4️⃣ | Copy Playlist ID |

---

## 📦 Install Dependencies

```bash
pip install google-api-python-client
```

---

## 🔑 API Setup

### Enable YouTube Data API

1. Open Google Cloud Console
2. Create or select a project
3. Enable **YouTube Data API v3**
4. Navigate to:

```text
APIs & Services
    ↓
Credentials
    ↓
Create API Key
```

> [!IMPORTANT]
> Never commit API keys to public repositories.

> [!WARNING]
> Store secrets using environment variables in production environments.

---

# 🐍 Reference Implementation

## extractor.py

```python
from googleapiclient.discovery import build
import os

API_KEY = os.getenv("YOUTUBE_API_KEY")
PLAYLIST_ID = "YOUR_PLAYLIST_ID"

youtube = build(
    "youtube",
    "v3",
    developerKey=API_KEY
)

def fetch_all_playlist_videos(playlist_id):
    """
    Fetch all videos from a playlist using
    YouTube API pagination.
    """

    videos = []
    next_page_token = None

    print("🚀 Starting playlist ingestion...")

    while True:

        request = youtube.playlistItems().list(
            part="snippet",
            playlistId=playlist_id,
            maxResults=50,
            pageToken=next_page_token
        )

        response = request.execute()

        for item in response.get("items", []):

            snippet = item["snippet"]

            video_id = snippet["resourceId"]["videoId"]

            videos.append({
                "title": snippet["title"],
                "description": snippet.get(
                    "description",
                    ""
                ),
                "url": (
                    f"https://www.youtube.com/"
                    f"watch?v={video_id}"
                )
            })

        next_page_token = response.get(
            "nextPageToken"
        )

        if not next_page_token:
            break

    return videos


if __name__ == "__main__":

    videos = fetch_all_playlist_videos(
        PLAYLIST_ID
    )

    print(
        f"\n✅ Successfully fetched "
        f"{len(videos)} videos.\n"
    )

    for i, video in enumerate(videos[:5], 1):

        print(
            f"{i}. {video['title']}"
        )

        print(
            f"🔗 {video['url']}\n"
        )
```

---

# 📊 Sample Output

```text
🚀 Starting playlist ingestion...

✅ Successfully fetched 312 videos.

1. Building a Vector Search Engine
   🔗 https://www.youtube.com/watch?v=xxxx

2. PostgreSQL Internals Explained
   🔗 https://www.youtube.com/watch?v=xxxx

3. CAP Theorem Deep Dive
   🔗 https://www.youtube.com/watch?v=xxxx
```

---

# 🧠 Semantic Search Pipeline

## Phase 1 — Metadata Collection

Collect:

- Video Title
- Description
- URL
- Publish Date

---

## Phase 2 — Transcript Extraction

```bash
pip install youtube-transcript-api
```

Retrieve spoken content for indexing.

---

## Phase 3 — Generate Embeddings

Possible embedding models:

| Model | Use Case |
|---------|---------|
| OpenAI Embeddings | High quality |
| BGE | Open source |
| E5 | Retrieval optimized |
| Sentence Transformers | Lightweight |

---

## Phase 4 — Vector Database

Supported options:

- ChromaDB
- Qdrant
- Pinecone
- Weaviate
- PostgreSQL + pgvector

---

## Phase 5 — Natural Language Search

```text
Find videos discussing event sourcing

Show Redis performance tutorials

Search clips mentioning PostgreSQL indexing

Videos about distributed systems
```

---

# 🗺️ Roadmap

## Core Features

- [ ] Export playlist metadata to CSV
- [ ] Transcript extraction support
- [ ] Embedding generation
- [ ] Local vector indexing
- [ ] Semantic search

---

## Integrations

- [ ] ChromaDB
- [ ] Qdrant
- [ ] Pinecone
- [ ] PostgreSQL + pgvector

---

## User Experience

- [ ] Streamlit Dashboard
- [ ] Search UI
- [ ] Video Preview Cards
- [ ] One-click Sharing
- [ ] Docker Deployment

---

# 📂 Project Structure

```text
youtube-playlist-indexer/
│
├── extractor.py
├── requirements.txt
├── README.md
│
├── data/
│   └── playlist.csv
│
├── transcripts/
│   └── subtitles.json
│
├── embeddings/
│   └── vectors.db
│
└── app/
    └── streamlit_app.py
```

---

# 🔥 Why This Project Exists

Most people use playlists as collections.

This project treats playlists as a searchable knowledge system.

```text
Save Once
     ↓
Index Everything
     ↓
Search Naturally
     ↓
Retrieve Instantly
```

> [!NOTE]
> The ultimate goal is to make YouTube playlists behave like a personal search engine.

---

# 🤝 Contributing

Contributions are welcome.

Ideas, pull requests, feature requests, and architectural improvements are encouraged.

---

# 📜 License

Distributed under the MIT License.

See the `LICENSE` file for more information.

---

<div align="center">

### ⭐ If this project helped you, consider giving it a star.

**Save Once • Search Forever**

</div>
