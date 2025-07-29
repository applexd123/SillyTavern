# SillyTavern Getting Started Guide

## Table of Contents

1. [Quick Start](#quick-start)
2. [Installation](#installation)
3. [Basic Configuration](#basic-configuration)
4. [First Steps](#first-steps)
5. [Creating Your First Character](#creating-your-first-character)
6. [Setting Up AI Models](#setting-up-ai-models)
7. [Advanced Configuration](#advanced-configuration)
8. [Extension Development](#extension-development)
9. [API Integration](#api-integration)
10. [Troubleshooting](#troubleshooting)

## Quick Start

### Prerequisites

- **Node.js 18+** (Latest LTS recommended)
- **Git** (for cloning the repository)
- **Web browser** (Chrome, Firefox, Safari, Edge)

### 5-Minute Setup

```bash
# Clone the repository
git clone https://github.com/SillyTavern/SillyTavern.git
cd SillyTavern

# Install dependencies
npm install

# Start the server
npm start
```

Open your browser and navigate to `http://localhost:8000`

## Installation

### Method 1: Git Clone (Recommended)

```bash
# Clone the repository
git clone https://github.com/SillyTavern/SillyTavern.git
cd SillyTavern

# Install dependencies
npm install

# Start the server
node server.js
```

### Method 2: Download ZIP

1. Download the latest release from [GitHub Releases](https://github.com/SillyTavern/SillyTavern/releases)
2. Extract the ZIP file
3. Open terminal/command prompt in the extracted folder
4. Run `npm install`
5. Run `node server.js`

### Method 3: Docker

```bash
# Using Docker
docker run -p 8000:8000 -v st-data:/app/data ghcr.io/sillytavern/sillytavern:latest

# Using Docker Compose
version: '3.8'
services:
  sillytavern:
    image: ghcr.io/sillytavern/sillytavern:latest
    ports:
      - "8000:8000"
    volumes:
      - st-data:/app/data
    environment:
      - ST_PORT=8000
      - ST_LISTEN=true

volumes:
  st-data:
```

### Method 4: Global Installation

```bash
# Install globally via npm
npm install -g sillytavern

# Run from anywhere
sillytavern --port 3000 --listen
```

## Basic Configuration

### Configuration File

Create a `config.yaml` file in the root directory:

```yaml
# Basic server configuration
port: 8000
listen: false  # Set to true to allow external connections
enableIPv4: true
enableIPv6: false

# Security settings
whitelistMode: true
basicAuthMode: false
disableCsrf: false
enableCorsProxy: false

# SSL configuration (optional)
ssl: false
certPath: 'certs/cert.pem'
keyPath: 'certs/privkey.pem'

# User accounts (optional)
enableUserAccounts: false

# Performance settings
performance:
  memoryCacheCapacity: '100mb'
  lazyLoadCharacters: false
  useDiskCache: true

# Extensions
extensions:
  enabled: true
  autoUpdate: true

# OpenAI settings (example)
openai:
  captionSystemPrompt: 'Describe this image in detail.'
```

### Environment Variables

You can override configuration using environment variables:

```bash
# Server settings
export SILLYTAVERN_PORT=3000
export SILLYTAVERN_LISTEN=true
export SILLYTAVERN_DATAROOT=/custom/data/path

# Security settings
export SILLYTAVERN_WHITELISTMODE=false
export SILLYTAVERN_BASICAUTHMODE=true

# SSL settings
export SILLYTAVERN_SSL=true
export SILLYTAVERN_CERTPATH=/path/to/cert.pem
export SILLYTAVERN_KEYPATH=/path/to/privkey.pem
```

### Command Line Arguments

```bash
# Start with custom settings
node server.js --port 3000 --listen --dataRoot ./custom-data

# Enable SSL
node server.js --ssl --certPath ./certs/cert.pem --keyPath ./certs/privkey.pem

# Disable CSRF protection (for development)
node server.js --disableCsrf

# Enable CORS proxy
node server.js --enableCorsProxy
```

## First Steps

### 1. Access the Interface

1. Start SillyTavern: `node server.js`
2. Open your browser to `http://localhost:8000`
3. You'll see the main interface with an empty character list

### 2. Configure API Connection

Before creating characters, set up your AI model connection:

1. Click the **Settings** button (⚙️)
2. Navigate to **API Connections**
3. Choose your preferred AI service:

#### OpenAI Setup
```javascript
// In the OpenAI settings panel:
{
  "api_url": "https://api.openai.com/v1",
  "api_key": "your-openai-api-key",
  "model": "gpt-3.5-turbo",
  "max_tokens": 300,
  "temperature": 0.7
}
```

#### Local AI Setup (Ollama)
```javascript
// For local Ollama installation:
{
  "api_url": "http://localhost:11434/v1",
  "model": "llama2",
  "max_tokens": 300,
  "temperature": 0.7
}
```

### 3. Test Connection

1. Click **Test Connection** in the API settings
2. You should see a success message
3. If it fails, check your API key and URL

## Creating Your First Character

### Method 1: Using the Character Creator

1. Click the **Create Character** button (+)
2. Fill in the basic information:

```javascript
// Character data structure
{
  "name": "Alice",
  "description": "A friendly AI assistant who loves to help with any task. She's knowledgeable, patient, and always tries to provide accurate information.",
  "personality": "Helpful, curious, optimistic, patient, knowledgeable",
  "first_mes": "Hello! I'm Alice, your AI assistant. How can I help you today?",
  "scenario": "You're having a conversation with Alice, an AI assistant designed to help with various tasks and answer questions.",
  "mes_example": "<START>\n{{user}}: Can you help me with math?\n{{char}}: Of course! I'd be happy to help you with math. What specific topic or problem would you like to work on?\n<START>\n{{user}}: What's the weather like?\n{{char}}: I don't have access to current weather data, but I can help you find weather information or discuss weather-related topics. What would you like to know?",
  "tags": ["assistant", "helpful", "knowledgeable"]
}
```

3. Upload an avatar image (optional)
4. Click **Create** to save the character

### Method 2: Import from File

1. Click **Import Character**
2. Select a character file (PNG with embedded data, JSON, or YAML)
3. The character will be automatically imported

### Method 3: Using the API

```javascript
// Create character via API
const characterData = {
  name: "Bob",
  description: "A wise mentor character",
  personality: "Wise, patient, experienced",
  first_mes: "Greetings, student. What wisdom do you seek today?",
  scenario: "Learning from an experienced mentor",
  avatar: "data:image/png;base64,..." // Base64 encoded image
};

const response = await fetch('/api/characters/create', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(characterData)
});

const result = await response.json();
console.log('Character created:', result);
```

## Setting Up AI Models

### OpenAI Configuration

1. Get an API key from [OpenAI](https://platform.openai.com/api-keys)
2. In SillyTavern settings, go to **API Connections > OpenAI**
3. Configure:

```yaml
# OpenAI settings
openai:
  api_url: "https://api.openai.com/v1"
  api_key: "sk-your-api-key-here"
  model: "gpt-3.5-turbo"
  max_tokens: 300
  temperature: 0.7
  top_p: 1.0
  frequency_penalty: 0.0
  presence_penalty: 0.0
  streaming: true
```

### Anthropic Claude

```yaml
# Anthropic settings
anthropic:
  api_url: "https://api.anthropic.com/v1"
  api_key: "your-anthropic-key"
  model: "claude-3-sonnet-20240229"
  max_tokens: 300
  temperature: 0.7
```

### Local AI (Ollama)

1. Install [Ollama](https://ollama.ai/)
2. Pull a model: `ollama pull llama2`
3. Configure in SillyTavern:

```yaml
# Ollama settings
ollama:
  api_url: "http://localhost:11434/v1"
  model: "llama2"
  max_tokens: 300
  temperature: 0.7
```

### AI Horde (Free)

1. Register at [AI Horde](https://aihorde.net/)
2. Get your API key
3. Configure:

```yaml
# AI Horde settings
horde:
  api_key: "your-horde-key"
  models: ["PygmalionAI/mythalion-13b"]
  max_length: 300
  temperature: 0.7
```

## Advanced Configuration

### User Account System

Enable multi-user support:

```yaml
# Enable user accounts
enableUserAccounts: true

# Optional: Enable external auth (Authelia)
autheliaAuth: false

# Optional: Per-user basic auth
perUserBasicAuth: false
```

Create admin user:
```bash
# Using the CLI
node server.js --createUser admin "Administrator" password123 --admin

# Or via API after enabling accounts
curl -X POST http://localhost:8000/api/users/create \
  -H "Content-Type: application/json" \
  -d '{"handle": "admin", "name": "Administrator", "password": "password123", "admin": true}'
```

### SSL/HTTPS Setup

1. Generate certificates:
```bash
# Using OpenSSL
openssl req -x509 -newkey rsa:4096 -keyout privkey.pem -out cert.pem -days 365 -nodes

# Using Let's Encrypt (for public servers)
certbot certonly --standalone -d yourdomain.com
```

2. Configure SSL:
```yaml
ssl: true
certPath: 'certs/cert.pem'
keyPath: 'certs/privkey.pem'
```

### Reverse Proxy Setup

#### Nginx Configuration

```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    location / {
        proxy_pass http://localhost:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

#### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName your-domain.com
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://localhost:8000/
    ProxyPassReverse / http://localhost:8000/
    
    ProxyPass /ws ws://localhost:8000/ws
    ProxyPassReverse /ws ws://localhost:8000/ws
</VirtualHost>
```

### Performance Optimization

```yaml
# Performance settings
performance:
  memoryCacheCapacity: '500mb'  # Increase for more characters
  lazyLoadCharacters: true      # Enable for large character collections
  useDiskCache: true           # Enable disk caching
  
# Rate limiting
rateLimiting:
  enabled: true
  windowMs: 60000  # 1 minute
  max: 100         # requests per window
```

## Extension Development

### Creating a Basic Extension

1. Create extension directory:
```bash
mkdir extensions/my-extension
cd extensions/my-extension
```

2. Create `manifest.json`:
```json
{
  "name": "My Extension",
  "version": "1.0.0",
  "description": "A sample extension",
  "author": "Your Name",
  "main": "index.js",
  "dependencies": [],
  "permissions": ["storage", "ui"]
}
```

3. Create `index.js`:
```javascript
// Extension main file
class MyExtension {
  constructor() {
    this.name = 'My Extension';
    this.version = '1.0.0';
  }
  
  // Called when extension is loaded
  async init() {
    console.log('My Extension initialized');
    
    // Add UI elements
    this.addUI();
    
    // Register event listeners
    this.registerEvents();
    
    // Load extension settings
    this.settings = await this.loadSettings();
  }
  
  // Add UI elements
  addUI() {
    const settingsPanel = document.getElementById('extensions_settings');
    const extensionDiv = document.createElement('div');
    extensionDiv.innerHTML = `
      <h3>My Extension</h3>
      <div class="inline-drawer">
        <div class="inline-drawer-toggle inline-drawer-header">
          <b>Extension Settings</b>
          <div class="inline-drawer-icon fa-solid fa-circle-chevron-down down"></div>
        </div>
        <div class="inline-drawer-content">
          <label for="my-extension-enabled">
            <input type="checkbox" id="my-extension-enabled" />
            Enable Extension
          </label>
          <button id="my-extension-test">Test Extension</button>
        </div>
      </div>
    `;
    
    settingsPanel.appendChild(extensionDiv);
    
    // Add event listeners
    document.getElementById('my-extension-test').addEventListener('click', () => {
      this.testFunction();
    });
  }
  
  // Register global event listeners
  registerEvents() {
    // Listen for character selection
    document.addEventListener('characterSelected', (event) => {
      console.log('Character selected:', event.detail.character);
    });
    
    // Listen for message sent
    document.addEventListener('messageSent', (event) => {
      console.log('Message sent:', event.detail.message);
    });
  }
  
  // Test function
  testFunction() {
    toastr.success('Extension test successful!');
  }
  
  // Load extension settings
  async loadSettings() {
    const response = await fetch('/api/extensions/settings/my-extension');
    if (response.ok) {
      return await response.json();
    }
    return {};
  }
  
  // Save extension settings
  async saveSettings(settings) {
    await fetch('/api/extensions/settings/my-extension', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(settings)
    });
  }
  
  // Called when extension is unloaded
  destroy() {
    console.log('My Extension destroyed');
  }
}

// Register the extension
if (typeof registerExtension === 'function') {
  registerExtension(new MyExtension());
}
```

### Advanced Extension Features

#### Slash Commands

```javascript
// Register custom slash command
this.registerSlashCommand('mycommand', (args) => {
  const message = args.join(' ') || 'Hello from my extension!';
  toastr.info(message);
  return message;
}, 'Custom command that displays a message');

// Usage in chat: /mycommand Hello world!
```

#### Character Data Modification

```javascript
// Modify character data before display
document.addEventListener('characterLoaded', (event) => {
  const character = event.detail.character;
  
  // Add custom data
  if (!character.extensions) character.extensions = {};
  if (!character.extensions.myExtension) {
    character.extensions.myExtension = {
      customField: 'default value',
      enabled: true
    };
  }
});
```

#### Message Processing

```javascript
// Process messages before sending
document.addEventListener('beforeMessageSent', (event) => {
  let message = event.detail.message;
  
  // Modify message content
  message = message.replace(/\[custom\]/g, 'Custom replacement');
  
  // Update the message
  event.detail.message = message;
});

// Process received messages
document.addEventListener('messageReceived', (event) => {
  const message = event.detail.message;
  
  // Add custom styling or processing
  if (message.mes.includes('important')) {
    // Highlight important messages
    const messageElement = document.querySelector(`[data-message-id="${message.mes_id}"]`);
    if (messageElement) {
      messageElement.classList.add('important-message');
    }
  }
});
```

## API Integration

### Basic API Usage

```javascript
// API client class
class SillyTavernAPI {
  constructor(baseUrl = '', apiKey = null) {
    this.baseUrl = baseUrl;
    this.apiKey = apiKey;
  }
  
  // Make authenticated request
  async request(endpoint, options = {}) {
    const url = `${this.baseUrl}${endpoint}`;
    const headers = {
      'Content-Type': 'application/json',
      ...options.headers
    };
    
    if (this.apiKey) {
      headers['Authorization'] = `Bearer ${this.apiKey}`;
    }
    
    const response = await fetch(url, {
      ...options,
      headers
    });
    
    if (!response.ok) {
      throw new Error(`API request failed: ${response.status} ${response.statusText}`);
    }
    
    return await response.json();
  }
  
  // Character methods
  async getCharacters() {
    return await this.request('/api/characters');
  }
  
  async getCharacter(name) {
    return await this.request(`/api/characters/${encodeURIComponent(name)}`);
  }
  
  async createCharacter(characterData) {
    return await this.request('/api/characters/create', {
      method: 'POST',
      body: JSON.stringify(characterData)
    });
  }
  
  async updateCharacter(name, updates) {
    return await this.request('/api/characters/edit', {
      method: 'POST',
      body: JSON.stringify({ name, ...updates })
    });
  }
  
  async deleteCharacter(name) {
    return await this.request('/api/characters/delete', {
      method: 'POST',
      body: JSON.stringify({ name })
    });
  }
  
  // Chat methods
  async getChats() {
    return await this.request('/api/chats');
  }
  
  async getChat(characterName, fileName) {
    return await this.request('/api/chats/get', {
      method: 'POST',
      body: JSON.stringify({
        ch_name: characterName,
        file_name: fileName
      })
    });
  }
  
  async saveChat(characterName, fileName, messages) {
    return await this.request('/api/chats/save', {
      method: 'POST',
      body: JSON.stringify({
        ch_name: characterName,
        file_name: fileName,
        chat: messages
      })
    });
  }
  
  // AI generation
  async generateText(messages, options = {}) {
    return await this.request('/api/openai/generate', {
      method: 'POST',
      body: JSON.stringify({
        messages,
        max_tokens: 300,
        temperature: 0.7,
        ...options
      })
    });
  }
}

// Usage example
const api = new SillyTavernAPI('http://localhost:8000');

// Get all characters
const characters = await api.getCharacters();
console.log('Characters:', characters);

// Create a new character
const newCharacter = await api.createCharacter({
  name: 'API Character',
  description: 'Created via API',
  personality: 'Friendly and helpful',
  first_mes: 'Hello! I was created via the API.'
});

// Generate AI response
const response = await api.generateText([
  { role: 'user', content: 'Hello, how are you?' }
]);
console.log('AI Response:', response);
```

### WebSocket Integration

```javascript
// WebSocket client for real-time updates
class SillyTavernWebSocket {
  constructor(url = 'ws://localhost:8000') {
    this.url = url;
    this.ws = null;
    this.handlers = {};
  }
  
  connect() {
    this.ws = new WebSocket(this.url);
    
    this.ws.onopen = () => {
      console.log('WebSocket connected');
      this.emit('connected');
    };
    
    this.ws.onmessage = (event) => {
      const data = JSON.parse(event.data);
      this.emit(data.type, data.payload);
    };
    
    this.ws.onclose = () => {
      console.log('WebSocket disconnected');
      this.emit('disconnected');
    };
    
    this.ws.onerror = (error) => {
      console.error('WebSocket error:', error);
      this.emit('error', error);
    };
  }
  
  on(event, handler) {
    if (!this.handlers[event]) {
      this.handlers[event] = [];
    }
    this.handlers[event].push(handler);
  }
  
  emit(event, data) {
    if (this.handlers[event]) {
      this.handlers[event].forEach(handler => handler(data));
    }
  }
  
  send(type, payload) {
    if (this.ws && this.ws.readyState === WebSocket.OPEN) {
      this.ws.send(JSON.stringify({ type, payload }));
    }
  }
  
  disconnect() {
    if (this.ws) {
      this.ws.close();
    }
  }
}

// Usage
const ws = new SillyTavernWebSocket();

ws.on('characterLoaded', (character) => {
  console.log('Character loaded:', character);
});

ws.on('messageSent', (message) => {
  console.log('Message sent:', message);
});

ws.connect();
```

## Troubleshooting

### Common Issues

#### 1. Server Won't Start

**Error**: `EADDRINUSE: address already in use`
```bash
# Solution: Use different port
node server.js --port 3000

# Or kill process using port 8000
lsof -ti:8000 | xargs kill -9
```

**Error**: `Cannot find module`
```bash
# Solution: Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

#### 2. API Connection Issues

**Error**: `401 Unauthorized`
- Check your API key is correct
- Verify the API service is working
- Check rate limits

**Error**: `CORS error`
```yaml
# Solution: Enable CORS proxy
enableCorsProxy: true
```

#### 3. Character Issues

**Error**: Character won't load
- Check character file format
- Verify character data is valid JSON
- Check file permissions

**Error**: Avatar not displaying
- Ensure image is in supported format (PNG, JPG, WebP)
- Check image size (recommended: 512x512 or smaller)
- Verify image data is valid base64

#### 4. Performance Issues

**Slow character loading**:
```yaml
# Enable lazy loading
performance:
  lazyLoadCharacters: true
  useDiskCache: true
  memoryCacheCapacity: '200mb'
```

**High memory usage**:
- Reduce `memoryCacheCapacity`
- Enable `lazyLoadCharacters`
- Clear browser cache

### Debug Mode

Enable debug logging:
```bash
# Start with debug output
DEBUG=* node server.js

# Or specific debug categories
DEBUG=sillytavern:* node server.js
```

### Log Files

Check log files for errors:
```bash
# View server logs
tail -f logs/server.log

# View error logs
tail -f logs/error.log

# View access logs
tail -f logs/access.log
```

### Browser Developer Tools

1. Open Developer Tools (F12)
2. Check Console for JavaScript errors
3. Check Network tab for failed requests
4. Check Application tab for storage issues

### Getting Help

- **GitHub Issues**: [Report bugs and request features](https://github.com/SillyTavern/SillyTavern/issues)
- **Discord**: [Join the community](https://discord.gg/sillytavern)
- **Documentation**: [Official docs](https://docs.sillytavern.app/)
- **Reddit**: [r/SillyTavern](https://reddit.com/r/SillyTavern)

### Backup and Recovery

#### Backup Data

```bash
# Backup entire data directory
cp -r data/ backups/data-$(date +%Y%m%d-%H%M%S)/

# Backup specific components
cp -r data/characters/ backups/characters-$(date +%Y%m%d)/
cp -r data/chats/ backups/chats-$(date +%Y%m%d)/
cp data/settings.json backups/settings-$(date +%Y%m%d).json
```

#### Restore Data

```bash
# Restore from backup
cp -r backups/data-20240101-120000/* data/

# Restore specific components
cp -r backups/characters-20240101/* data/characters/
```

#### Automated Backup Script

```bash
#!/bin/bash
# backup.sh

BACKUP_DIR="backups"
DATE=$(date +%Y%m%d-%H%M%S)
DATA_DIR="data"

# Create backup directory
mkdir -p "$BACKUP_DIR"

# Create compressed backup
tar -czf "$BACKUP_DIR/sillytavern-backup-$DATE.tar.gz" "$DATA_DIR"

# Keep only last 10 backups
ls -t "$BACKUP_DIR"/sillytavern-backup-*.tar.gz | tail -n +11 | xargs rm -f

echo "Backup created: $BACKUP_DIR/sillytavern-backup-$DATE.tar.gz"
```

This getting started guide provides comprehensive instructions for setting up and using SillyTavern, from basic installation to advanced configuration and development. Follow the sections relevant to your needs and refer to the troubleshooting section if you encounter any issues.