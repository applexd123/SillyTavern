# SillyTavern REST API Endpoints Documentation

## Table of Contents

1. [Authentication Endpoints](#authentication-endpoints)
2. [Character Management](#character-management)
3. [Chat Management](#chat-management)
4. [AI Model Integration](#ai-model-integration)
5. [Vector Embeddings](#vector-embeddings)
6. [Settings Management](#settings-management)
7. [User Management](#user-management)
8. [File Management](#file-management)
9. [Image Generation](#image-generation)
10. [Search and Discovery](#search-and-discovery)
11. [Extensions](#extensions)
12. [Statistics](#statistics)
13. [Utilities](#utilities)

## Authentication Endpoints

### POST /api/users/login
Authenticate user with handle and password.

**Request:**
```json
{
  "handle": "username",
  "password": "user_password"
}
```

**Response (Success):**
```json
{
  "success": true,
  "handle": "username",
  "name": "Display Name",
  "admin": false
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Invalid credentials"
}
```

### POST /api/users/logout
Logout current user and destroy session.

**Response:**
```json
{
  "success": true
}
```

### GET /api/users/me
Get current user information.

**Response:**
```json
{
  "handle": "username",
  "name": "Display Name",
  "admin": false,
  "created": 1704067200000,
  "avatar": "/img/default-user.png"
}
```

## Character Management

### GET /api/characters
Get list of all characters for the current user.

**Query Parameters:**
- `full` (boolean): Return full character data instead of summary

**Response:**
```json
[
  {
    "name": "Alice",
    "description": "A friendly AI assistant",
    "avatar": "alice.png",
    "create_date": "2024-01-01T00:00:00.000Z",
    "chat": "alice_20240101",
    "tags": ["assistant", "friendly"],
    "fav": false
  }
]
```

### GET /api/characters/:name
Get specific character data.

**Parameters:**
- `name` (string): Character name

**Response:**
```json
{
  "name": "Alice",
  "description": "A friendly AI assistant who loves to help",
  "personality": "Helpful, curious, and optimistic",
  "first_mes": "Hello! I'm Alice, how can I help you today?",
  "scenario": "A conversation with an AI assistant",
  "mes_example": "<START>\n{{user}}: Hi\n{{char}}: Hello there!",
  "create_date": "2024-01-01T00:00:00.000Z",
  "avatar": "alice.png",
  "chat": "alice_20240101",
  "tags": ["assistant", "friendly"],
  "fav": false,
  "talkativeness": 0.5,
  "extensions": {}
}
```

### POST /api/characters/create
Create a new character.

**Request:**
```json
{
  "name": "Bob",
  "description": "A wise mentor character",
  "personality": "Wise, patient, and knowledgeable",
  "first_mes": "Greetings, young one. What wisdom do you seek?",
  "scenario": "Learning from a wise mentor",
  "mes_example": "<START>\n{{user}}: Teach me\n{{char}}: I shall guide you",
  "avatar": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD...",
  "tags": ["mentor", "wise"]
}
```

**Response:**
```json
{
  "success": true,
  "name": "Bob",
  "avatar": "bob.png"
}
```

### POST /api/characters/edit
Edit existing character.

**Request:**
```json
{
  "name": "Alice",
  "description": "An updated description",
  "personality": "Updated personality traits",
  "avatar": "new_avatar_data"
}
```

**Response:**
```json
{
  "success": true
}
```

### POST /api/characters/delete
Delete a character and associated data.

**Request:**
```json
{
  "name": "CharacterToDelete"
}
```

**Response:**
```json
{
  "success": true
}
```

### POST /api/characters/import
Import character from various formats (PNG, JSON, etc.).

**Request (Multipart Form):**
- `avatar`: Character file (PNG with metadata, JSON, etc.)

**Response:**
```json
{
  "success": true,
  "name": "ImportedCharacter",
  "avatar": "imported_character.png"
}
```

### POST /api/characters/export/:name
Export character data.

**Parameters:**
- `name` (string): Character name

**Query Parameters:**
- `format` (string): Export format ("json", "png", "yaml")

**Response:**
Binary file download or JSON data depending on format.

## Chat Management

### GET /api/chats
Get list of all chats for current user.

**Query Parameters:**
- `character` (string): Filter by character name

**Response:**
```json
[
  {
    "file_name": "alice_20240101.jsonl",
    "character_name": "Alice",
    "create_date": "2024-01-01T00:00:00.000Z",
    "last_mes": "That's a great question!",
    "mes_count": 42
  }
]
```

### POST /api/chats/get
Get chat messages.

**Request:**
```json
{
  "ch_name": "Alice",
  "file_name": "alice_20240101.jsonl"
}
```

**Response:**
```json
[
  {
    "name": "User",
    "mes": "Hello Alice!",
    "send_date": "2024-01-01T10:00:00.000Z",
    "is_user": true,
    "is_system": false,
    "extra": {}
  },
  {
    "name": "Alice",
    "mes": "Hello! Nice to meet you!",
    "send_date": "2024-01-01T10:00:05.000Z",
    "is_user": false,
    "is_system": false,
    "extra": {
      "api": "openai",
      "model": "gpt-3.5-turbo"
    }
  }
]
```

### POST /api/chats/save
Save chat messages.

**Request:**
```json
{
  "ch_name": "Alice",
  "file_name": "alice_20240101.jsonl",
  "chat": [
    {
      "name": "User",
      "mes": "New message",
      "send_date": "2024-01-01T10:05:00.000Z",
      "is_user": true
    }
  ]
}
```

**Response:**
```json
{
  "success": true,
  "file_name": "alice_20240101.jsonl"
}
```

### POST /api/chats/delete
Delete a chat file.

**Request:**
```json
{
  "ch_name": "Alice",
  "file_name": "alice_20240101.jsonl"
}
```

**Response:**
```json
{
  "success": true
}
```

### POST /api/chats/rename
Rename a chat file.

**Request:**
```json
{
  "ch_name": "Alice",
  "original_file": "alice_20240101.jsonl",
  "renamed_file": "alice_conversation.jsonl"
}
```

**Response:**
```json
{
  "success": true
}
```

## AI Model Integration

### POST /api/openai/generate
Generate text using OpenAI-compatible API.

**Request:**
```json
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant."
    },
    {
      "role": "user",
      "content": "Hello!"
    }
  ],
  "max_tokens": 150,
  "temperature": 0.7,
  "top_p": 1.0,
  "frequency_penalty": 0.0,
  "presence_penalty": 0.0,
  "stream": false
}
```

**Response:**
```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "Hello! How can I help you today?"
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 20,
    "completion_tokens": 10,
    "total_tokens": 30
  }
}
```

### POST /api/openai/caption-image
Generate image captions using vision models.

**Request:**
```json
{
  "api": "openai",
  "model": "gpt-4-vision-preview",
  "prompt": "Describe this image in detail",
  "image": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD...",
  "reverse_proxy": false
}
```

**Response:**
```json
{
  "caption": "The image shows a beautiful sunset over a mountain landscape with vibrant orange and pink colors in the sky."
}
```

### POST /api/anthropic/generate
Generate text using Anthropic Claude API.

**Request:**
```json
{
  "model": "claude-3-sonnet-20240229",
  "messages": [
    {
      "role": "user",
      "content": "Hello Claude!"
    }
  ],
  "max_tokens": 150,
  "temperature": 0.7
}
```

**Response:**
```json
{
  "content": [
    {
      "type": "text",
      "text": "Hello! I'm Claude, an AI assistant. How can I help you today?"
    }
  ],
  "usage": {
    "input_tokens": 10,
    "output_tokens": 15
  }
}
```

### POST /api/novelai/generate
Generate text using NovelAI API.

**Request:**
```json
{
  "model": "kayra-v1",
  "input": "Once upon a time",
  "parameters": {
    "max_length": 150,
    "temperature": 0.7,
    "top_p": 0.9,
    "repetition_penalty": 1.1
  }
}
```

**Response:**
```json
{
  "output": "Once upon a time, in a land far away, there lived a brave knight who embarked on an epic quest..."
}
```

### GET /api/horde/models
Get available AI Horde models.

**Response:**
```json
[
  {
    "name": "PygmalionAI/mythalion-13b",
    "count": 5,
    "performance": 8.2,
    "queued": 12,
    "eta": 45
  }
]
```

### POST /api/horde/generate
Generate text using AI Horde.

**Request:**
```json
{
  "prompt": "Hello world",
  "params": {
    "max_length": 100,
    "temperature": 0.7,
    "top_p": 0.9,
    "models": ["PygmalionAI/mythalion-13b"]
  }
}
```

**Response:**
```json
{
  "id": "12345-67890-abcdef",
  "kudos": 10,
  "generations": [
    {
      "text": "Hello world! This is a generated response from the AI Horde.",
      "seed": 42,
      "id": "gen-12345"
    }
  ]
}
```

## Vector Embeddings

### POST /api/vectors/query
Query vector database for similar content.

**Request:**
```json
{
  "source": "characters",
  "input": "friendly assistant character",
  "top_k": 5,
  "threshold": 0.7
}
```

**Response:**
```json
{
  "results": [
    {
      "text": "Alice: A friendly AI assistant who loves to help",
      "score": 0.92,
      "metadata": {
        "character": "Alice",
        "type": "description"
      }
    },
    {
      "text": "Bob: A helpful companion for daily tasks",
      "score": 0.85,
      "metadata": {
        "character": "Bob",
        "type": "description"
      }
    }
  ]
}
```

### POST /api/vectors/insert
Insert text into vector database.

**Request:**
```json
{
  "source": "characters",
  "input": "Charlie: A mysterious character with hidden depths",
  "metadata": {
    "character": "Charlie",
    "type": "description",
    "created": "2024-01-01T00:00:00.000Z"
  }
}
```

**Response:**
```json
{
  "success": true,
  "inserted": 1
}
```

### POST /api/vectors/delete
Delete entries from vector database.

**Request:**
```json
{
  "source": "characters",
  "metadata_filter": {
    "character": "Charlie"
  }
}
```

**Response:**
```json
{
  "success": true,
  "deleted": 3
}
```

### GET /api/vectors/collections
Get list of vector collections.

**Response:**
```json
{
  "collections": [
    {
      "name": "characters",
      "count": 150,
      "size": "2.3MB"
    },
    {
      "name": "chats",
      "count": 1200,
      "size": "15.7MB"
    }
  ]
}
```

## Settings Management

### GET /api/settings
Get user settings.

**Response:**
```json
{
  "username": "user",
  "theme": "dark",
  "language": "en",
  "api_server": "http://localhost:5000",
  "api_server_textgenerationwebui": "http://localhost:5000",
  "openai_setting_names": ["Default"],
  "novel_setting_names": ["Default"],
  "power_user": {
    "tokenizer": "openai",
    "token_padding": 64,
    "pygmalion_formatting": 0
  },
  "extensions": {
    "enabled": true,
    "autoUpdate": true
  }
}
```

### POST /api/settings/save
Save user settings.

**Request:**
```json
{
  "username": "user",
  "theme": "light",
  "language": "es",
  "api_server": "http://localhost:8080",
  "power_user": {
    "tokenizer": "llama",
    "token_padding": 128
  }
}
```

**Response:**
```json
{
  "success": true,
  "backup": "settings_user_20240101_120000.json"
}
```

### GET /api/settings/presets/:type
Get presets for a specific type.

**Parameters:**
- `type` (string): Preset type ("openai", "novel", "textgen", etc.)

**Response:**
```json
[
  {
    "name": "Creative Writing",
    "temperature": 0.9,
    "top_p": 0.9,
    "repetition_penalty": 1.1
  },
  {
    "name": "Factual",
    "temperature": 0.3,
    "top_p": 0.8,
    "repetition_penalty": 1.0
  }
]
```

### POST /api/settings/presets/:type
Save a preset.

**Parameters:**
- `type` (string): Preset type

**Request:**
```json
{
  "name": "My Custom Preset",
  "temperature": 0.8,
  "top_p": 0.95,
  "max_tokens": 200
}
```

**Response:**
```json
{
  "success": true,
  "name": "My Custom Preset"
}
```

### DELETE /api/settings/presets/:type/:name
Delete a preset.

**Parameters:**
- `type` (string): Preset type
- `name` (string): Preset name

**Response:**
```json
{
  "success": true
}
```

## User Management

### GET /api/users/all
Get list of all users (admin only).

**Response:**
```json
[
  {
    "handle": "admin",
    "name": "Administrator",
    "admin": true,
    "enabled": true,
    "created": 1704067200000,
    "avatar": "/img/admin-avatar.png"
  },
  {
    "handle": "user1",
    "name": "Regular User",
    "admin": false,
    "enabled": true,
    "created": 1704153600000,
    "avatar": "/img/default-user.png"
  }
]
```

### POST /api/users/create
Create a new user (admin only).

**Request:**
```json
{
  "handle": "newuser",
  "name": "New User",
  "password": "secure_password",
  "admin": false
}
```

**Response:**
```json
{
  "success": true,
  "handle": "newuser",
  "name": "New User"
}
```

### POST /api/users/update/:handle
Update user information (admin or self).

**Parameters:**
- `handle` (string): User handle

**Request:**
```json
{
  "name": "Updated Name",
  "enabled": true,
  "admin": false
}
```

**Response:**
```json
{
  "success": true
}
```

### DELETE /api/users/:handle
Delete a user (admin only).

**Parameters:**
- `handle` (string): User handle

**Response:**
```json
{
  "success": true
}
```

### POST /api/users/change-password
Change user password.

**Request:**
```json
{
  "current_password": "old_password",
  "new_password": "new_secure_password"
}
```

**Response:**
```json
{
  "success": true
}
```

## File Management

### GET /api/assets
Get list of user assets.

**Query Parameters:**
- `type` (string): Asset type ("avatar", "background", "image")

**Response:**
```json
[
  {
    "name": "character_avatar.png",
    "type": "avatar",
    "size": 45632,
    "created": "2024-01-01T00:00:00.000Z"
  },
  {
    "name": "background.jpg",
    "type": "background", 
    "size": 156789,
    "created": "2024-01-01T00:00:00.000Z"
  }
]
```

### POST /api/assets/upload
Upload asset file.

**Request (Multipart Form):**
- `file`: File to upload
- `type`: Asset type ("avatar", "background", "image")

**Response:**
```json
{
  "success": true,
  "filename": "uploaded_image.png",
  "url": "/assets/uploaded_image.png"
}
```

### DELETE /api/assets/:filename
Delete an asset file.

**Parameters:**
- `filename` (string): File name to delete

**Response:**
```json
{
  "success": true
}
```

### GET /api/files
Get list of uploaded files.

**Response:**
```json
[
  {
    "name": "document.pdf",
    "size": 1024000,
    "type": "application/pdf",
    "uploaded": "2024-01-01T00:00:00.000Z"
  }
]
```

### POST /api/files/upload
Upload a file.

**Request (Multipart Form):**
- `file`: File to upload

**Response:**
```json
{
  "success": true,
  "filename": "document.pdf",
  "url": "/files/document.pdf",
  "size": 1024000
}
```

## Image Generation

### POST /api/sd/generate
Generate image using Stable Diffusion.

**Request:**
```json
{
  "prompt": "a beautiful landscape with mountains and a lake",
  "negative_prompt": "blurry, low quality",
  "width": 512,
  "height": 512,
  "steps": 20,
  "cfg_scale": 7.5,
  "sampler": "Euler a",
  "seed": 42
}
```

**Response:**
```json
{
  "images": [
    "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAA..."
  ],
  "info": {
    "seed": 42,
    "steps": 20,
    "cfg_scale": 7.5
  }
}
```

### GET /api/sd/models
Get available Stable Diffusion models.

**Response:**
```json
[
  {
    "title": "v1-5-pruned.ckpt",
    "model_name": "v1-5-pruned",
    "hash": "cc6cb27103",
    "filename": "v1-5-pruned.ckpt"
  }
]
```

### GET /api/sd/samplers
Get available samplers.

**Response:**
```json
[
  {
    "name": "Euler a",
    "aliases": ["euler_a"]
  },
  {
    "name": "DPM++ 2M Karras",
    "aliases": ["dpmpp_2m_karras"]
  }
]
```

## Search and Discovery

### POST /api/search/characters
Search characters by various criteria.

**Request:**
```json
{
  "query": "friendly assistant",
  "tags": ["helper", "ai"],
  "sort": "name",
  "limit": 10,
  "offset": 0
}
```

**Response:**
```json
{
  "results": [
    {
      "name": "Alice",
      "description": "A friendly AI assistant",
      "avatar": "alice.png",
      "tags": ["assistant", "friendly"],
      "score": 0.95
    }
  ],
  "total": 1,
  "limit": 10,
  "offset": 0
}
```

### POST /api/search/chats
Search chat messages.

**Request:**
```json
{
  "query": "hello world",
  "character": "Alice",
  "date_from": "2024-01-01",
  "date_to": "2024-01-31",
  "limit": 20
}
```

**Response:**
```json
{
  "results": [
    {
      "character": "Alice",
      "chat_file": "alice_20240101.jsonl",
      "message": "Hello world! How are you?",
      "sender": "Alice",
      "date": "2024-01-01T10:00:00.000Z",
      "context": "...previous message... Hello world! How are you? ...next message..."
    }
  ],
  "total": 1
}
```

## Extensions

### GET /api/extensions
Get list of installed extensions.

**Response:**
```json
[
  {
    "name": "character-expressions",
    "version": "1.2.0",
    "enabled": true,
    "description": "Adds character expressions support",
    "author": "SillyTavern",
    "url": "https://github.com/SillyTavern/Extension-CharacterExpressions"
  }
]
```

### POST /api/extensions/install
Install an extension.

**Request:**
```json
{
  "url": "https://github.com/username/extension-name"
}
```

**Response:**
```json
{
  "success": true,
  "name": "extension-name",
  "version": "1.0.0"
}
```

### POST /api/extensions/toggle/:name
Enable/disable an extension.

**Parameters:**
- `name` (string): Extension name

**Request:**
```json
{
  "enabled": true
}
```

**Response:**
```json
{
  "success": true,
  "enabled": true
}
```

### DELETE /api/extensions/:name
Uninstall an extension.

**Parameters:**
- `name` (string): Extension name

**Response:**
```json
{
  "success": true
}
```

## Statistics

### GET /api/stats
Get usage statistics.

**Response:**
```json
{
  "characters": {
    "total": 25,
    "created_today": 2
  },
  "chats": {
    "total": 150,
    "messages_today": 45
  },
  "tokens": {
    "total_used": 125000,
    "used_today": 2500
  },
  "uptime": 86400
}
```

### GET /api/stats/characters
Get character statistics.

**Response:**
```json
{
  "most_popular": [
    {
      "name": "Alice",
      "message_count": 500,
      "last_chat": "2024-01-01T10:00:00.000Z"
    }
  ],
  "recently_created": [
    {
      "name": "Bob",
      "created": "2024-01-01T09:00:00.000Z"
    }
  ]
}
```

### GET /api/stats/tokens
Get token usage statistics.

**Query Parameters:**
- `period` (string): Time period ("day", "week", "month")

**Response:**
```json
{
  "period": "day",
  "usage": [
    {
      "date": "2024-01-01",
      "tokens": 2500,
      "cost": 0.05
    }
  ],
  "total": {
    "tokens": 2500,
    "cost": 0.05
  }
}
```

## Utilities

### GET /api/ping
Health check endpoint.

**Response:**
```json
{
  "result": "pong",
  "timestamp": "2024-01-01T10:00:00.000Z",
  "version": "1.13.2"
}
```

### GET /api/version
Get server version information.

**Response:**
```json
{
  "agent": "SillyTavern",
  "version": "1.13.2",
  "pkgVersion": "1.13.2",
  "gitRevision": "abc123def456",
  "gitBranch": "main"
}
```

### POST /api/tokenizers/encode
Encode text to tokens.

**Request:**
```json
{
  "text": "Hello world!",
  "tokenizer": "openai"
}
```

**Response:**
```json
{
  "tokens": [15496, 1917, 0],
  "count": 3
}
```

### POST /api/tokenizers/decode
Decode tokens to text.

**Request:**
```json
{
  "tokens": [15496, 1917, 0],
  "tokenizer": "openai"
}
```

**Response:**
```json
{
  "text": "Hello world!"
}
```

### POST /api/translate
Translate text.

**Request:**
```json
{
  "text": "Hello world",
  "from": "en",
  "to": "es",
  "service": "google"
}
```

**Response:**
```json
{
  "translated": "Hola mundo",
  "from": "en",
  "to": "es"
}
```

### GET /api/themes
Get available themes.

**Response:**
```json
[
  {
    "name": "dark",
    "display_name": "Dark Theme",
    "author": "SillyTavern",
    "version": "1.0.0"
  },
  {
    "name": "light", 
    "display_name": "Light Theme",
    "author": "SillyTavern",
    "version": "1.0.0"
  }
]
```

### POST /api/backup
Create system backup.

**Request:**
```json
{
  "include_chats": true,
  "include_characters": true,
  "include_settings": true
}
```

**Response:**
```json
{
  "success": true,
  "filename": "backup_20240101_120000.zip",
  "size": 15728640,
  "url": "/backups/backup_20240101_120000.zip"
}
```

### POST /api/restore
Restore from backup.

**Request (Multipart Form):**
- `backup`: Backup file to restore

**Response:**
```json
{
  "success": true,
  "restored": {
    "characters": 25,
    "chats": 150,
    "settings": 1
  }
}
```

## Error Responses

All endpoints may return error responses with the following format:

### 400 Bad Request
```json
{
  "error": "Invalid request parameters",
  "details": "Missing required field: name"
}
```

### 401 Unauthorized
```json
{
  "error": "Authentication required",
  "redirect": "/login"
}
```

### 403 Forbidden
```json
{
  "error": "Insufficient permissions",
  "required": "admin"
}
```

### 404 Not Found
```json
{
  "error": "Resource not found",
  "resource": "character",
  "name": "NonExistentCharacter"
}
```

### 429 Too Many Requests
```json
{
  "error": "Rate limit exceeded",
  "retry_after": 60
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal server error",
  "timestamp": "2024-01-01T10:00:00.000Z",
  "request_id": "req_12345"
}
```

## Rate Limiting

Most endpoints are subject to rate limiting:

- **Authentication**: 5 requests per minute
- **AI Generation**: 10 requests per minute
- **File Upload**: 20 requests per hour
- **General API**: 100 requests per minute

Rate limit headers are included in responses:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1704067260
```

## Authentication

Most endpoints require authentication via session cookies. Include the session cookie in requests:

```bash
curl -X GET "http://localhost:8000/api/characters" \
  -H "Cookie: session=abc123def456"
```

For programmatic access, first login to obtain a session:

```bash
curl -X POST "http://localhost:8000/api/users/login" \
  -H "Content-Type: application/json" \
  -d '{"handle": "username", "password": "password"}' \
  -c cookies.txt

curl -X GET "http://localhost:8000/api/characters" \
  -b cookies.txt
```

## CSRF Protection

When CSRF protection is enabled, include the CSRF token in POST requests:

```bash
curl -X POST "http://localhost:8000/api/characters/create" \
  -H "Content-Type: application/json" \
  -H "X-CSRF-Token: csrf_token_here" \
  -d '{"name": "New Character", "description": "..."}'
```

This documentation covers all major REST API endpoints in SillyTavern. For the most up-to-date information, refer to the source code in the `src/endpoints/` directory.