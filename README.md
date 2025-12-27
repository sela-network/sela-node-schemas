# Sela Semantic Schemas

Platform-specific schemas for semantic web extraction.

## Structure

```
schemas/
├── platforms/
│   ├── twitter/
│   │   ├── _platform.json    # Full platform schema
│   │   ├── search.json       # Search results page
│   │   ├── timeline.json     # Home timeline
│   │   └── profile.json      # User profile
│   └── linkedin/
│       ├── _platform.json
│       ├── feed.json         # Home feed
│       ├── search.json       # Search results
│       └── profile.json      # User profile
├── index.json                # Schema index
└── README.md
```

## Usage

### Raw GitHub URL

```bash
# Base URL
https://raw.githubusercontent.com/{owner}/sela-node-v2/main/schemas

# Get platform schema
GET {base}/platforms/twitter/_platform.json

# Get individual page type
GET {base}/platforms/twitter/search.json

# Get index
GET {base}/index.json
```

### Environment Configuration

```bash
# .env
SCHEMA_REGISTRY_URL=https://raw.githubusercontent.com/{owner}/sela-node-v2/main/schemas
```

## Schema Format

### Platform Schema (`_platform.json`)

```json
{
  "platform_id": "twitter",
  "display_name": "Twitter / X",
  "domains": ["twitter.com", "x.com"],
  "metadata": {
    "cid": "bafk...",
    "version": "1.0.0"
  },
  "page_types": { ... },
  "default_page_type": "timeline"
}
```

### Page Type Schema

```json
{
  "platform_id": "twitter",
  "page_type_id": "search",
  "display_name": "Search Results",
  "url_patterns": ["/search", "/search?*"],
  "root_selector": "main",
  "content": { ... },
  "actions": [ ... ],
  "metadata": { ... }
}
```

## Regenerating Schemas

From `peer-node/src-tauri`:

```bash
cargo run --bin export_schemas
```

## Adding New Platforms

1. Create schema builder in `peer-node/src-tauri/src/semantic/mod.rs`
2. Add to `export_schemas.rs`
3. Run `cargo run --bin export_schemas`
4. Commit and push

## CID (Content Identifier)

Each schema has a CID generated using BLAKE3 hash. The CID changes when schema content changes, enabling:
- Cache invalidation
- Version tracking
- Content verification

## License

MIT
