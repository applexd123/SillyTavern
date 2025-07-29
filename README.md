# SillyTavern API Documentation

## 📚 Complete Documentation Suite

This repository contains comprehensive documentation for all SillyTavern APIs, functions, and components. Whether you're a developer integrating with SillyTavern, creating extensions, or just getting started, you'll find everything you need here.

## 🚀 Quick Start

### For New Users
👉 **[Getting Started Guide](GETTING_STARTED_GUIDE.md)** - Complete setup and usage instructions

### For Developers
👉 **[API Documentation](API_DOCUMENTATION.md)** - Server setup, configuration, and core APIs  
👉 **[REST API Endpoints](ENDPOINT_DOCUMENTATION.md)** - Detailed endpoint documentation with examples  
👉 **[Client Components](CLIENT_COMPONENTS_DOCUMENTATION.md)** - Frontend JavaScript components and APIs

## 📖 Documentation Index

### Core Documentation
| Document | Description | Use Case |
|----------|-------------|----------|
| **[API Documentation](API_DOCUMENTATION.md)** | Server setup, configuration, command-line interface, core APIs | Setting up SillyTavern, understanding the architecture |
| **[REST API Endpoints](ENDPOINT_DOCUMENTATION.md)** | Complete REST API reference with request/response examples | API integration, building clients |
| **[Client Components](CLIENT_COMPONENTS_DOCUMENTATION.md)** | Frontend JavaScript components and their public APIs | Extension development, UI customization |
| **[Getting Started Guide](GETTING_STARTED_GUIDE.md)** | Step-by-step setup and usage instructions | New users, installation, troubleshooting |

### Key Features Covered

#### 🖥️ Server-Side APIs
- **Command Line Interface** - CLI arguments and configuration options
- **REST API Endpoints** - Authentication, characters, chats, AI models, vectors
- **User Management** - Multi-user support, authentication, permissions
- **Plugin System** - Creating and loading server-side plugins
- **Event System** - Server events and real-time notifications
- **Vector Embeddings** - Semantic search and similarity matching
- **Middleware** - Security, caching, logging, and performance

#### 🌐 Client-Side Components
- **Popup System** - Modal dialogs and user interactions
- **Utility Functions** - Helper functions for common tasks
- **Chat Management** - Message handling and conversation flow
- **Character Management** - Creating, editing, and organizing characters
- **Settings System** - Configuration and preferences
- **UI Components** - Reusable interface elements
- **Event System** - Client-side events and communication
- **Extensions Framework** - Building and integrating extensions
- **Tag System** - Organization and categorization
- **Search & Filtering** - Finding and organizing content
- **Internationalization** - Multi-language support

## 🎯 Common Use Cases

### Getting Started
```bash
# Quick setup
git clone https://github.com/SillyTavern/SillyTavern.git
cd SillyTavern
npm install
npm start
```
📖 **[Full Installation Guide →](GETTING_STARTED_GUIDE.md#installation)**

### API Integration
```javascript
// Basic API usage
const response = await fetch('/api/characters');
const characters = await response.json();
```
📖 **[Complete API Reference →](ENDPOINT_DOCUMENTATION.md)**

### Creating Extensions
```javascript
// Extension structure
const extension = {
  name: 'My Extension',
  init: function() {
    // Extension initialization
  }
};
```
📖 **[Extension Development Guide →](GETTING_STARTED_GUIDE.md#extension-development)**

### Character Management
```javascript
// Create character via API
const character = await fetch('/api/characters/create', {
  method: 'POST',
  body: JSON.stringify({
    name: 'Alice',
    description: 'A helpful assistant'
  })
});
```
📖 **[Character API Documentation →](ENDPOINT_DOCUMENTATION.md#character-management)**

## 🏗️ Architecture Overview

SillyTavern follows a modern web application architecture:

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Client-Side   │    │   Server-Side   │    │   External APIs │
│                 │    │                 │    │                 │
│ • UI Components │◄──►│ • REST API      │◄──►│ • OpenAI        │
│ • Extensions    │    │ • WebSockets    │    │ • Anthropic     │
│ • Event System │    │ • User System   │    │ • Local AI      │
│ • Utilities     │    │ • Plugin System │    │ • AI Horde      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Core Components

- **Server** (`src/server-main.js`) - Express.js application with middleware
- **API Endpoints** (`src/endpoints/`) - RESTful API for all operations  
- **Client Scripts** (`public/scripts/`) - Frontend JavaScript components
- **Extensions** (`extensions/`) - Plugin system for extensibility
- **Configuration** (`config.yaml`) - YAML-based configuration
- **Data Storage** (`data/`) - File-based storage for characters, chats, settings

## 🔧 Configuration

### Basic Configuration
```yaml
# config.yaml
port: 8000
listen: false
enableUserAccounts: false
extensions:
  enabled: true
```

### Environment Variables
```bash
SILLYTAVERN_PORT=3000
SILLYTAVERN_LISTEN=true
SILLYTAVERN_ENABLEUSERACCOUNTS=true
```

📖 **[Complete Configuration Guide →](API_DOCUMENTATION.md#server-setup-and-configuration)**

## 🔌 Extension Development

SillyTavern supports both server-side and client-side extensions:

### Server-Side Plugin
```javascript
// plugins/my-plugin.js
export default {
  name: 'My Plugin',
  init: async function(app, config) {
    app.get('/api/my-plugin/status', (req, res) => {
      res.json({ status: 'active' });
    });
  }
};
```

### Client-Side Extension
```javascript
// extensions/my-extension/index.js
class MyExtension {
  init() {
    // Add UI elements
    // Register event listeners
    // Load settings
  }
}
```

📖 **[Extension Development Guide →](GETTING_STARTED_GUIDE.md#extension-development)**

## 🌐 API Examples

### Authentication
```javascript
// Login
const response = await fetch('/api/users/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    handle: 'username',
    password: 'password'
  })
});
```

### Character Operations
```javascript
// Get all characters
const characters = await fetch('/api/characters').then(r => r.json());

// Create character
const newCharacter = await fetch('/api/characters/create', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: 'Alice',
    description: 'A helpful AI assistant'
  })
});
```

### AI Generation
```javascript
// Generate with OpenAI
const response = await fetch('/api/openai/generate', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    messages: [
      { role: 'user', content: 'Hello!' }
    ],
    max_tokens: 150,
    temperature: 0.7
  })
});
```

📖 **[Complete API Examples →](ENDPOINT_DOCUMENTATION.md)**

## 🎨 UI Components

### Popup System
```javascript
import { Popup, POPUP_TYPE } from './scripts/popup.js';

// Show confirmation dialog
const confirmed = await Popup.show.confirm(
  'Delete Character',
  'Are you sure you want to delete this character?'
);

// Show input dialog
const name = await Popup.show.input(
  'Character Name',
  'Enter the character name:'
);
```

### Utility Functions
```javascript
import { debounce, deepMerge, uuidv4 } from './scripts/utils.js';

// Debounce function calls
const debouncedSave = debounce(saveFunction, 500);

// Deep merge objects
const merged = deepMerge(defaultSettings, userSettings);

// Generate unique IDs
const messageId = uuidv4();
```

📖 **[Complete UI Component Guide →](CLIENT_COMPONENTS_DOCUMENTATION.md)**

## 🔍 Search and Discovery

### Character Search
```javascript
// Search characters by name and tags
const results = await searchCharacters({
  query: 'helpful assistant',
  tags: ['ai', 'assistant'],
  limit: 10
});
```

### Vector Search
```javascript
// Semantic search using embeddings
const similar = await fetch('/api/vectors/query', {
  method: 'POST',
  body: JSON.stringify({
    source: 'characters',
    input: 'friendly helper',
    top_k: 5
  })
});
```

📖 **[Search Documentation →](CLIENT_COMPONENTS_DOCUMENTATION.md#search-and-filtering)**

## 🔒 Security Features

- **CSRF Protection** - Cross-site request forgery prevention
- **Rate Limiting** - API request throttling
- **Input Validation** - Server-side data validation
- **User Authentication** - Multi-user support with sessions
- **IP Whitelisting** - Network access control
- **SSL/HTTPS** - Encrypted connections

📖 **[Security Configuration →](API_DOCUMENTATION.md#security-considerations)**

## 🚀 Performance Features

- **Memory Caching** - In-memory data caching
- **Disk Caching** - Persistent cache storage
- **Lazy Loading** - On-demand character loading
- **Compression** - Response compression
- **Asset Optimization** - Image and file optimization

📖 **[Performance Optimization →](GETTING_STARTED_GUIDE.md#advanced-configuration)**

## 🌍 Multi-language Support

```javascript
import { t, setLocale } from './scripts/i18n.js';

// Translate text
const message = t('Hello World');

// Change language
await setLocale('es');
```

📖 **[Internationalization Guide →](CLIENT_COMPONENTS_DOCUMENTATION.md#internationalization)**

## 🔧 Development Tools

### Debug Mode
```bash
# Enable debug logging
DEBUG=* node server.js

# Specific debug categories
DEBUG=sillytavern:* node server.js
```

### API Testing
```bash
# Test character endpoint
curl -X GET "http://localhost:8000/api/characters" \
  -H "Accept: application/json"

# Test with authentication
curl -X POST "http://localhost:8000/api/characters/create" \
  -H "Content-Type: application/json" \
  -H "Cookie: session=abc123" \
  -d '{"name": "Test", "description": "Test character"}'
```

📖 **[Development Guide →](GETTING_STARTED_GUIDE.md#troubleshooting)**

## 📊 Monitoring and Logging

### Statistics API
```javascript
// Get usage statistics
const stats = await fetch('/api/stats').then(r => r.json());
console.log('Characters:', stats.characters.total);
console.log('Messages today:', stats.chats.messages_today);
```

### Access Logs
```bash
# View access logs
tail -f logs/access.log

# View error logs  
tail -f logs/error.log
```

📖 **[Monitoring Documentation →](ENDPOINT_DOCUMENTATION.md#statistics)**

## 🤝 Contributing

We welcome contributions to improve the documentation! Here's how you can help:

1. **Report Issues** - Found something unclear or incorrect? [Open an issue](https://github.com/SillyTavern/SillyTavern/issues)
2. **Suggest Improvements** - Have ideas for better documentation? Let us know!
3. **Submit Pull Requests** - Fix typos, add examples, or improve explanations
4. **Share Examples** - Contribute real-world usage examples

### Documentation Standards
- Use clear, concise language
- Include working code examples
- Provide both basic and advanced usage
- Keep examples up-to-date with latest API changes

## 📞 Support and Community

- **GitHub Issues** - [Bug reports and feature requests](https://github.com/SillyTavern/SillyTavern/issues)
- **Discord** - [Join the community chat](https://discord.gg/sillytavern)
- **Reddit** - [r/SillyTavern community](https://reddit.com/r/SillyTavern)
- **Documentation** - [Official documentation site](https://docs.sillytavern.app/)

## 📝 License

This documentation is part of the SillyTavern project and is licensed under the AGPL-3.0 license. See the [LICENSE](LICENSE) file for details.

## 🏷️ Version Information

- **Documentation Version**: 1.0.0
- **SillyTavern Version**: 1.13.2+
- **Last Updated**: January 2024
- **Node.js Requirement**: 18+

---

## 📚 Quick Navigation

| Topic | Documentation | Description |
|-------|---------------|-------------|
| **Setup** | [Getting Started](GETTING_STARTED_GUIDE.md) | Installation, configuration, first steps |
| **Server API** | [API Documentation](API_DOCUMENTATION.md) | Server setup, CLI, core APIs |
| **REST Endpoints** | [Endpoint Documentation](ENDPOINT_DOCUMENTATION.md) | Complete API reference |
| **Client Components** | [Client Documentation](CLIENT_COMPONENTS_DOCUMENTATION.md) | Frontend components and utilities |
| **Extensions** | [Extension Guide](GETTING_STARTED_GUIDE.md#extension-development) | Creating plugins and extensions |
| **Troubleshooting** | [Debug Guide](GETTING_STARTED_GUIDE.md#troubleshooting) | Common issues and solutions |

**Happy coding! 🎉**