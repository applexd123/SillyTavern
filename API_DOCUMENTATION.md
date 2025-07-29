# SillyTavern API Documentation

## Table of Contents

1. [Overview](#overview)
2. [Server Setup and Configuration](#server-setup-and-configuration)
3. [Command Line Interface](#command-line-interface)
4. [REST API Endpoints](#rest-api-endpoints)
5. [Event System](#event-system)
6. [User Management System](#user-management-system)
7. [Vector Embedding System](#vector-embedding-system)
8. [Plugin System](#plugin-system)
9. [Client-Side Components](#client-side-components)
10. [Utility Functions](#utility-functions)
11. [Middleware Components](#middleware-components)
12. [Examples and Usage](#examples-and-usage)

## Overview

SillyTavern is a Node.js-based web application for AI-powered chat interactions. It provides a comprehensive API for managing characters, chats, user accounts, and AI model integrations.

**Key Features:**
- Multi-user support with authentication
- Character and chat management
- AI model integrations (OpenAI, Anthropic, NovelAI, etc.)
- Vector embeddings for enhanced conversations
- Plugin system for extensibility
- Real-time event system
- REST API for all operations

**System Requirements:**
- Node.js >= 18
- Supported platforms: Linux, Windows, macOS, Android

## Server Setup and Configuration

### Starting the Server

```bash
# Basic startup
node server.js

# With custom configuration
node server.js --port 3000 --listen --dataRoot ./custom-data

# Debug mode
npm run debug

# Global mode
npm run start:global
```

### Configuration File

The server uses a YAML configuration file (`config.yaml`). Key configuration options:

```yaml
# Server settings
port: 8000
listen: false
enableIPv4: true
enableIPv6: false

# Security settings
enableCorsProxy: false
disableCsrf: false
whitelistMode: true
basicAuthMode: false

# SSL configuration
ssl: false
certPath: 'certs/cert.pem'
keyPath: 'certs/privkey.pem'

# User accounts
enableUserAccounts: false
autheliaAuth: false
perUserBasicAuth: false

# Performance settings
performance:
  memoryCacheCapacity: '100mb'
  lazyLoadCharacters: false
  useDiskCache: true
```

### Environment Variables

Configuration can be overridden using environment variables with the prefix `SILLYTAVERN_`:

```bash
# Example environment variables
SILLYTAVERN_PORT=3000
SILLYTAVERN_LISTEN=true
SILLYTAVERN_ENABLEUSERACCOUNTS=true
```

## Command Line Interface

### CommandLineParser Class

The `CommandLineParser` class handles command-line argument parsing and configuration.

```javascript
import { CommandLineParser } from './src/command-line.js';

const parser = new CommandLineParser();
const args = parser.parse(process.argv);
```

### Available Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `--port` | number | 8000 | Server port |
| `--listen` | boolean | false | Listen on all interfaces |
| `--dataRoot` | string | './data' | Data directory path |
| `--configPath` | string | './config.yaml' | Config file path |
| `--ssl` | boolean | false | Enable SSL |
| `--certPath` | string | 'certs/cert.pem' | SSL certificate path |
| `--keyPath` | string | 'certs/privkey.pem' | SSL private key path |
| `--whitelistMode` | boolean | true | Enable whitelist mode |
| `--basicAuthMode` | boolean | false | Enable basic auth |
| `--disableCsrf` | boolean | false | Disable CSRF protection |
| `--enableCorsProxy` | boolean | false | Enable CORS proxy |

### Configuration Methods

```javascript
// Get default configuration
const defaultConfig = parser.getDefaultConfig(isGlobal);

// Parse arguments with config fallback
const config = parser.parse(process.argv);

// Access parsed values
console.log(config.port);
console.log(config.dataRoot);
console.log(config.getIPv4ListenUrl());
```

## REST API Endpoints

### Authentication Endpoints

#### POST /api/users/login
Login with username and password.

**Request:**
```json
{
  "handle": "username",
  "password": "password"
}
```

**Response:**
```json
{
  "success": true,
  "handle": "username",
  "name": "Display Name"
}
```

#### POST /api/users/logout
Logout current user.

**Response:**
```json
{
  "success": true
}
```

### Character Management

#### GET /api/characters
Get list of all characters.

**Response:**
```json
[
  {
    "name": "Character Name",
    "description": "Character description",
    "avatar": "character_avatar.png",
    "create_date": "2024-01-01T00:00:00.000Z",
    "chat": "character_chat_id"
  }
]
```

#### POST /api/characters/create
Create a new character.

**Request:**
```json
{
  "name": "Character Name",
  "description": "Character description",
  "personality": "Character personality",
  "first_mes": "First message",
  "scenario": "Character scenario",
  "mes_example": "Example messages",
  "avatar": "base64_image_data"
}
```

#### GET /api/characters/:name
Get specific character data.

**Response:**
```json
{
  "name": "Character Name",
  "description": "Character description",
  "personality": "Character personality",
  "first_mes": "First message",
  "scenario": "Character scenario",
  "mes_example": "Example messages",
  "create_date": "2024-01-01T00:00:00.000Z",
  "avatar": "character_avatar.png"
}
```

#### POST /api/characters/edit
Edit existing character.

**Request:**
```json
{
  "name": "Character Name",
  "description": "Updated description",
  "personality": "Updated personality"
}
```

#### POST /api/characters/delete
Delete a character.

**Request:**
```json
{
  "name": "Character Name"
}
```

### Chat Management

#### GET /api/chats
Get list of all chats.

**Response:**
```json
[
  {
    "file_name": "chat_id.jsonl",
    "character_name": "Character Name",
    "create_date": "2024-01-01T00:00:00.000Z",
    "last_mes": "Last message preview"
  }
]
```

#### POST /api/chats/get
Get chat messages.

**Request:**
```json
{
  "ch_name": "Character Name",
  "file_name": "chat_id.jsonl"
}
```

**Response:**
```json
[
  {
    "name": "User",
    "mes": "Hello!",
    "send_date": "2024-01-01T00:00:00.000Z",
    "is_user": true
  },
  {
    "name": "Character Name",
    "mes": "Hello there!",
    "send_date": "2024-01-01T00:00:00.000Z",
    "is_user": false
  }
]
```

#### POST /api/chats/save
Save chat message.

**Request:**
```json
{
  "ch_name": "Character Name",
  "file_name": "chat_id.jsonl",
  "chat": [
    {
      "name": "User",
      "mes": "New message",
      "send_date": "2024-01-01T00:00:00.000Z",
      "is_user": true
    }
  ]
}
```

### AI Model Integration

#### POST /api/openai/generate
Generate text using OpenAI-compatible API.

**Request:**
```json
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Hello!"
    }
  ],
  "max_tokens": 150,
  "temperature": 0.7
}
```

#### POST /api/anthropic/generate
Generate text using Anthropic Claude API.

**Request:**
```json
{
  "model": "claude-3-sonnet-20240229",
  "messages": [
    {
      "role": "user",
      "content": "Hello!"
    }
  ],
  "max_tokens": 150
}
```

### Vector Embeddings

#### POST /api/vectors/query
Query vector database.

**Request:**
```json
{
  "source": "characters",
  "input": "search query",
  "top_k": 5
}
```

**Response:**
```json
{
  "results": [
    {
      "text": "matching text",
      "score": 0.85,
      "metadata": {}
    }
  ]
}
```

#### POST /api/vectors/insert
Insert text into vector database.

**Request:**
```json
{
  "source": "characters",
  "input": "text to embed",
  "metadata": {}
}
```

### Settings Management

#### GET /api/settings
Get user settings.

**Response:**
```json
{
  "username": "user",
  "theme": "dark",
  "language": "en",
  "api_server": "http://localhost:5000"
}
```

#### POST /api/settings/save
Save user settings.

**Request:**
```json
{
  "username": "user",
  "theme": "dark",
  "language": "en"
}
```

## Event System

### Server Events

The server uses an EventEmitter-based system for real-time notifications.

```javascript
import { serverEvents, EVENT_NAMES } from './src/server-events.js';

// Listen for server started event
serverEvents.on(EVENT_NAMES.SERVER_STARTED, (event) => {
  console.log(`Server started at: ${event.url}`);
});
```

### Available Events

#### SERVER_STARTED
Emitted when the server has fully started.

**Payload:**
```typescript
interface ServerStartedEvent {
  url: URL; // The URL the server is listening on
}
```

**Usage:**
```javascript
serverEvents.on(EVENT_NAMES.SERVER_STARTED, (event) => {
  console.log(`Server is running at: ${event.url}`);
});
```

### Global Event Access

Events are also available globally via `process.serverEvents`:

```javascript
process.serverEvents.on(EVENT_NAMES.SERVER_STARTED, (event) => {
  // Handle server started event
});
```

## User Management System

### User Object Structure

```typescript
interface User {
  handle: string;      // Short handle for directories
  name: string;        // Display name
  created: number;     // Creation timestamp
  password: string;    // Scrypt hash
  salt: string;        // Password salt
  enabled: boolean;    // Account status
  admin: boolean;      // Admin privileges
}
```

### User Directory Structure

```typescript
interface UserDirectoryList {
  root: string;                // User root directory
  thumbnails: string;          // Thumbnail storage
  thumbnailsBg: string;        // Background thumbnails
  thumbnailsAvatar: string;    // Avatar thumbnails
  thumbnailsPersona: string;   // Persona thumbnails
  worlds: string;              // World info storage
  user: string;                // User public data
  avatars: string;             // Avatar storage
  userImages: string;          // Image storage
  groups: string;              // Group storage
  groupChats: string;          // Group chat storage
  chats: string;               // Chat storage
  characters: string;          // Character storage
  backgrounds: string;         // Background storage
  themes: string;              // Theme storage
  extensions: string;          // Extension storage
  files: string;               // File uploads
  vectors: string;             // Vector storage
  backups: string;             // Backup storage
}
```

### User Management Functions

```javascript
import {
  initUserStorage,
  getUserDirectories,
  requireLoginMiddleware,
  setUserDataMiddleware
} from './src/users.js';

// Initialize user storage
await initUserStorage();

// Get user directories
const directories = getUserDirectories('username');

// Middleware for authentication
app.use('/api/protected', requireLoginMiddleware);

// Middleware to set user data
app.use(setUserDataMiddleware);
```

### Authentication Middleware

```javascript
// Require login for protected routes
app.use('/api/admin', requireLoginMiddleware);

// Set user data in request object
app.use(setUserDataMiddleware);

// Access user data in routes
app.get('/api/user-info', (req, res) => {
  const user = req.user.profile;
  const directories = req.user.directories;
  res.json({ user, directories });
});
```

## Vector Embedding System

### Supported Providers

- **OpenAI**: GPT-based embeddings
- **Cohere**: Cohere embeddings
- **Google**: Google AI embeddings
- **Ollama**: Local embeddings
- **LlamaCpp**: Local LLM embeddings
- **NovelAI**: NovelAI embeddings
- **VLLM**: VLLM embeddings

### Vector Operations

```javascript
import { VectorStore } from './src/vectors/embedding.js';

// Initialize vector store
const vectorStore = new VectorStore('openai');

// Insert embeddings
await vectorStore.insert('source_id', 'text to embed', metadata);

// Query embeddings
const results = await vectorStore.query('search query', {
  top_k: 5,
  threshold: 0.7
});

// Delete embeddings
await vectorStore.delete('source_id');
```

### Configuration

```yaml
# Vector settings in config.yaml
vectors:
  provider: 'openai'
  model: 'text-embedding-ada-002'
  dimensions: 1536
  apiKey: 'your-api-key'
```

## Plugin System

### Plugin Structure

```javascript
// plugin-example.js
export default {
  name: 'Example Plugin',
  version: '1.0.0',
  description: 'An example plugin',
  
  // Plugin initialization
  init: async function(app, config) {
    console.log('Plugin initialized');
    
    // Add routes
    app.get('/api/plugin/example', (req, res) => {
      res.json({ message: 'Hello from plugin!' });
    });
  },
  
  // Plugin cleanup
  destroy: async function() {
    console.log('Plugin destroyed');
  }
};
```

### Loading Plugins

```javascript
import { loadPlugins } from './src/plugin-loader.js';

// Load all plugins from plugins directory
await loadPlugins(app);
```

### Plugin API

Plugins have access to:
- Express app instance
- Configuration object
- User management functions
- Database connections
- Event system

## Client-Side Components

### Main Application (`script.js`)

The main client-side application provides:

- Character management UI
- Chat interface
- Settings panels
- AI model integration
- Real-time updates

### Key Client Components

#### Chat Management (`chats.js`)
```javascript
// Load chat
async function loadChat(characterName, chatId) {
  const response = await fetch('/api/chats/get', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      ch_name: characterName,
      file_name: chatId
    })
  });
  return await response.json();
}

// Send message
async function sendMessage(message) {
  const response = await fetch('/api/chats/save', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      ch_name: currentCharacter,
      file_name: currentChat,
      chat: [...existingMessages, newMessage]
    })
  });
}
```

#### Character Management (`personas.js`)
```javascript
// Load characters
async function loadCharacters() {
  const response = await fetch('/api/characters');
  return await response.json();
}

// Create character
async function createCharacter(characterData) {
  const response = await fetch('/api/characters/create', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(characterData)
  });
}
```

#### Settings Management (`power-user.js`)
```javascript
// Load settings
async function loadSettings() {
  const response = await fetch('/api/settings');
  return await response.json();
}

// Save settings
async function saveSettings(settings) {
  const response = await fetch('/api/settings/save', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(settings)
  });
}
```

### UI Components

#### Popup System (`popup.js`)
```javascript
// Show popup
function showPopup(title, content, options = {}) {
  const popup = new Popup(title, content, options);
  popup.show();
  return popup;
}

// Confirmation dialog
async function confirmDialog(message) {
  return new Promise((resolve) => {
    const popup = showPopup('Confirm', message, {
      buttons: [
        { text: 'OK', onClick: () => resolve(true) },
        { text: 'Cancel', onClick: () => resolve(false) }
      ]
    });
  });
}
```

#### Tag System (`tags.js`)
```javascript
// Add tag to character
function addTag(characterName, tagName) {
  const tags = getCharacterTags(characterName);
  tags.push(tagName);
  saveCharacterTags(characterName, tags);
}

// Filter by tags
function filterCharactersByTags(tags) {
  return characters.filter(char => 
    tags.every(tag => getCharacterTags(char.name).includes(tag))
  );
}
```

## Utility Functions

### Configuration Utilities

```javascript
import {
  getConfig,
  getConfigValue,
  setConfigFilePath,
  keyToEnv
} from './src/util.js';

// Set config file path
setConfigFilePath('./custom-config.yaml');

// Get full config
const config = getConfig();

// Get specific config value with default
const port = getConfigValue('port', 8000, 'number');
const enabled = getConfigValue('feature.enabled', false, 'boolean');

// Convert config key to environment variable
const envKey = keyToEnv('api.openai.key'); // 'SILLYTAVERN_API_OPENAI_KEY'
```

### File System Utilities

```javascript
import {
  ensureDirectoryExists,
  sanitizeFilename,
  getFileType,
  isValidImageUrl
} from './src/util.js';

// Ensure directory exists
ensureDirectoryExists('./data/characters');

// Sanitize filename
const safe = sanitizeFilename('unsafe/filename.txt'); // 'unsafe_filename.txt'

// Get file MIME type
const mimeType = getFileType('./image.png'); // 'image/png'

// Validate image URL
const isValid = isValidImageUrl('https://example.com/image.jpg');
```

### String and Data Utilities

```javascript
import {
  tryParse,
  deepMerge,
  generateTimestamp,
  humanizedISO8601DateTime,
  color
} from './src/util.js';

// Safe JSON parsing
const data = tryParse('{"key": "value"}', {}); // Returns parsed object or default

// Deep merge objects
const merged = deepMerge(obj1, obj2);

// Generate timestamp
const timestamp = generateTimestamp(); // Current timestamp string

// Format date
const formatted = humanizedISO8601DateTime(new Date());

// Colored console output
console.log(color.red('Error message'));
console.log(color.green('Success message'));
console.log(color.yellow('Warning message'));
```

### Network Utilities

```javascript
import {
  getIPAddress,
  isValidUrl,
  makeHttpRequest
} from './src/util.js';

// Get local IP address
const ip = getIPAddress();

// Validate URL
const valid = isValidUrl('https://example.com');

// Make HTTP request
const response = await makeHttpRequest({
  url: 'https://api.example.com/data',
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ key: 'value' })
});
```

## Middleware Components

### Authentication Middleware

```javascript
import basicAuthMiddleware from './src/middleware/basicAuth.js';
import { requireLoginMiddleware } from './src/users.js';

// Basic authentication
app.use(basicAuthMiddleware);

// Login requirement
app.use('/api/admin', requireLoginMiddleware);
```

### Security Middleware

```javascript
import getWhitelistMiddleware from './src/middleware/whitelist.js';
import corsProxyMiddleware from './src/middleware/corsProxy.js';

// IP whitelist
const whitelistMiddleware = getWhitelistMiddleware(['127.0.0.1', '::1']);
app.use(whitelistMiddleware);

// CORS proxy
app.use('/api/proxy', corsProxyMiddleware);
```

### Performance Middleware

```javascript
import cacheBuster from './src/middleware/cacheBuster.js';
import accessLoggerMiddleware from './src/middleware/accessLogWriter.js';

// Cache busting
app.use('/static', cacheBuster);

// Access logging
app.use(accessLoggerMiddleware);
```

### File Upload Middleware

```javascript
import multerMonkeyPatch from './src/middleware/multerMonkeyPatch.js';
import validateAvatarUrlMiddleware from './src/middleware/validateFileName.js';

// Enhanced multer
app.use(multerMonkeyPatch);

// File validation
app.use('/api/upload', validateAvatarUrlMiddleware);
```

## Examples and Usage

### Basic Server Setup

```javascript
#!/usr/bin/env node
import { CommandLineParser } from './src/command-line.js';
import { serverDirectory } from './src/server-directory.js';

// Parse command line arguments
const cliArgs = new CommandLineParser().parse(process.argv);
globalThis.DATA_ROOT = cliArgs.dataRoot;
globalThis.COMMAND_LINE_ARGS = cliArgs;

// Change to server directory
process.chdir(serverDirectory);

// Import and start the main server
try {
  await import('./src/server-main.js');
} catch (error) {
  console.error('Server startup failed:', error);
}
```

### Creating a Custom Plugin

```javascript
// plugins/my-plugin.js
export default {
  name: 'My Custom Plugin',
  version: '1.0.0',
  description: 'A custom plugin for SillyTavern',
  
  init: async function(app, config) {
    console.log('Initializing My Custom Plugin');
    
    // Add custom API endpoint
    app.get('/api/my-plugin/status', (req, res) => {
      res.json({ 
        status: 'active',
        version: this.version,
        timestamp: new Date().toISOString()
      });
    });
    
    // Add custom middleware
    app.use('/api/my-plugin', (req, res, next) => {
      console.log(`Plugin API called: ${req.path}`);
      next();
    });
    
    // Listen to server events
    process.serverEvents.on('server-started', (event) => {
      console.log(`Plugin: Server started at ${event.url}`);
    });
  },
  
  destroy: async function() {
    console.log('Destroying My Custom Plugin');
    // Cleanup resources here
  }
};
```

### User Authentication Example

```javascript
import express from 'express';
import {
  requireLoginMiddleware,
  setUserDataMiddleware,
  getUserDirectories
} from './src/users.js';

const app = express();

// Set user data middleware
app.use(setUserDataMiddleware);

// Protected route example
app.get('/api/user/profile', requireLoginMiddleware, (req, res) => {
  const user = req.user.profile;
  const directories = req.user.directories;
  
  res.json({
    handle: user.handle,
    name: user.name,
    admin: user.admin,
    directories: Object.keys(directories)
  });
});

// Admin-only route
app.get('/api/admin/users', requireLoginMiddleware, (req, res) => {
  if (!req.user.profile.admin) {
    return res.status(403).json({ error: 'Admin access required' });
  }
  
  // Return user list for admins
  res.json({ users: [] });
});
```

### Vector Database Integration

```javascript
import { VectorStore } from './src/vectors/embedding.js';

// Initialize vector store
const vectorStore = new VectorStore('openai', {
  apiKey: process.env.OPENAI_API_KEY,
  model: 'text-embedding-ada-002'
});

// Add character descriptions to vector database
async function indexCharacter(character) {
  const text = `${character.name}: ${character.description}`;
  const metadata = {
    type: 'character',
    name: character.name,
    created: character.create_date
  };
  
  await vectorStore.insert(character.name, text, metadata);
}

// Search for similar characters
async function findSimilarCharacters(query, limit = 5) {
  const results = await vectorStore.query(query, {
    top_k: limit,
    threshold: 0.7
  });
  
  return results.map(result => ({
    name: result.metadata.name,
    similarity: result.score,
    text: result.text
  }));
}
```

### Custom API Endpoint

```javascript
import express from 'express';
import { getConfigValue } from './src/util.js';
import { requireLoginMiddleware } from './src/users.js';

const router = express.Router();

// Custom endpoint with authentication
router.post('/api/custom/analyze', requireLoginMiddleware, async (req, res) => {
  try {
    const { text } = req.body;
    
    if (!text) {
      return res.status(400).json({ error: 'Text is required' });
    }
    
    // Perform analysis
    const analysis = {
      length: text.length,
      words: text.split(/\s+/).length,
      sentiment: 'neutral', // Placeholder
      timestamp: new Date().toISOString()
    };
    
    res.json(analysis);
  } catch (error) {
    console.error('Analysis error:', error);
    res.status(500).json({ error: 'Analysis failed' });
  }
});

export default router;
```

### Client-Side Integration

```html
<!DOCTYPE html>
<html>
<head>
  <title>SillyTavern Integration</title>
</head>
<body>
  <script>
    // Initialize SillyTavern client integration
    class SillyTavernClient {
      constructor(baseUrl = '') {
        this.baseUrl = baseUrl;
      }
      
      async getCharacters() {
        const response = await fetch(`${this.baseUrl}/api/characters`);
        return await response.json();
      }
      
      async sendMessage(characterName, message) {
        const response = await fetch(`${this.baseUrl}/api/chats/save`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            ch_name: characterName,
            message: message
          })
        });
        return await response.json();
      }
      
      async generateResponse(prompt, options = {}) {
        const response = await fetch(`${this.baseUrl}/api/openai/generate`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            messages: [{ role: 'user', content: prompt }],
            ...options
          })
        });
        return await response.json();
      }
    }
    
    // Usage example
    const client = new SillyTavernClient();
    
    async function main() {
      // Get available characters
      const characters = await client.getCharacters();
      console.log('Available characters:', characters);
      
      // Generate AI response
      const response = await client.generateResponse('Hello, how are you?');
      console.log('AI Response:', response);
    }
    
    main().catch(console.error);
  </script>
</body>
</html>
```

## Error Handling

### Server-Side Error Handling

```javascript
// Global error handler
app.use((error, req, res, next) => {
  console.error('Server error:', error);
  
  if (res.headersSent) {
    return next(error);
  }
  
  const statusCode = error.statusCode || 500;
  const message = error.message || 'Internal server error';
  
  res.status(statusCode).json({
    error: message,
    timestamp: new Date().toISOString()
  });
});

// Async error wrapper
function asyncHandler(fn) {
  return (req, res, next) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
}

// Usage
app.get('/api/example', asyncHandler(async (req, res) => {
  const data = await someAsyncOperation();
  res.json(data);
}));
```

### Client-Side Error Handling

```javascript
// API request wrapper with error handling
async function apiRequest(url, options = {}) {
  try {
    const response = await fetch(url, options);
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    
    return await response.json();
  } catch (error) {
    console.error('API request failed:', error);
    
    // Show user-friendly error message
    showNotification('Error: ' + error.message, 'error');
    
    throw error;
  }
}
```

## Security Considerations

### CSRF Protection

```javascript
import { csrfSync } from 'csrf-sync';

const { csrfSynchronisedProtection } = csrfSync({
  getTokenFromRequest: (req) => req.body._csrf,
});

app.use(csrfSynchronisedProtection);
```

### Input Validation

```javascript
import { body, validationResult } from 'express-validator';

// Validation middleware
const validateCharacter = [
  body('name').trim().isLength({ min: 1, max: 100 }),
  body('description').trim().isLength({ max: 2000 }),
  body('personality').optional().trim().isLength({ max: 1000 })
];

app.post('/api/characters/create', validateCharacter, (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  
  // Process valid input
});
```

### Rate Limiting

```javascript
import { RateLimiterMemory } from 'rate-limiter-flexible';

const rateLimiter = new RateLimiterMemory({
  points: 100, // Number of requests
  duration: 60, // Per 60 seconds
});

app.use(async (req, res, next) => {
  try {
    await rateLimiter.consume(req.ip);
    next();
  } catch (error) {
    res.status(429).json({ error: 'Too many requests' });
  }
});
```

This documentation provides comprehensive coverage of SillyTavern's APIs, functions, and components. For specific implementation details, refer to the source code and configuration files.