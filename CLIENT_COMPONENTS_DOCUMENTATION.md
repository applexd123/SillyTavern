# SillyTavern Client-Side Components Documentation

## Table of Contents

1. [Overview](#overview)
2. [Popup System](#popup-system)
3. [Utility Functions](#utility-functions)
4. [Chat Management](#chat-management)
5. [Character Management](#character-management)
6. [Settings and Configuration](#settings-and-configuration)
7. [UI Components](#ui-components)
8. [Event System](#event-system)
9. [Extensions Framework](#extensions-framework)
10. [Tag System](#tag-system)
11. [Search and Filtering](#search-and-filtering)
12. [Internationalization](#internationalization)

## Overview

SillyTavern's client-side components provide a rich JavaScript API for building interactive web applications. The components are modular, well-documented, and designed for extensibility.

**Key Features:**
- Modular component architecture
- Event-driven communication
- Comprehensive popup system
- Utility functions for common tasks
- Internationalization support
- Extension framework
- Tag management system

## Popup System

### Popup Class

The `Popup` class provides a comprehensive modal dialog system with various types and customization options.

#### Basic Usage

```javascript
import { Popup, POPUP_TYPE, POPUP_RESULT } from './scripts/popup.js';

// Simple text popup
const popup = new Popup('Hello World!', POPUP_TYPE.TEXT);
const result = await popup.show();

// Input popup
const userInput = await Popup.show.input('Enter Name', 'Please enter your name:');
if (userInput) {
    console.log('User entered:', userInput);
}

// Confirmation popup
const confirmed = await Popup.show.confirm('Delete Item', 'Are you sure you want to delete this item?');
if (confirmed) {
    // User confirmed
}
```

#### Popup Types

```javascript
// Available popup types
export const POPUP_TYPE = {
    TEXT: 1,        // Main popup with content and buttons
    CONFIRM: 2,     // Yes/No confirmation dialog
    INPUT: 3,       // Input field dialog
    DISPLAY: 4,     // Display-only popup with X button
    CROP: 5         // Image cropping dialog
};

// Popup results
export const POPUP_RESULT = {
    AFFIRMATIVE: 1,    // OK/Yes button
    NEGATIVE: 0,       // Cancel/No button
    CANCELLED: null,   // Popup was cancelled
    CUSTOM1: 1001,     // Custom button results
    // ... CUSTOM2-CUSTOM9
};
```

#### Advanced Popup Options

```javascript
const popupOptions = {
    okButton: 'Save',              // Custom OK button text
    cancelButton: 'Cancel',        // Custom Cancel button text
    rows: 5,                       // Input field rows
    wide: true,                    // Wide mode
    wider: false,                  // Wider mode
    large: false,                  // Large mode (90% screen)
    transparent: false,            // Transparent background
    allowHorizontalScrolling: true, // Enable horizontal scroll
    allowVerticalScrolling: true,   // Enable vertical scroll
    leftAlign: false,              // Left-align content
    animation: 'slow',             // Animation speed
    defaultResult: POPUP_RESULT.AFFIRMATIVE,
    
    // Custom buttons
    customButtons: [
        {
            text: 'Custom Action',
            result: POPUP_RESULT.CUSTOM1,
            classes: ['btn-primary'],
            action: () => console.log('Custom action'),
            appendAtEnd: false
        }
    ],
    
    // Custom inputs
    customInputs: [
        {
            id: 'enableFeature',
            label: 'Enable Feature',
            tooltip: 'This enables the special feature',
            defaultState: true,
            type: 'checkbox'
        }
    ],
    
    // Event handlers
    onOpen: async (popup) => {
        console.log('Popup opened');
    },
    onClosing: async (popup) => {
        // Return false to prevent closing
        return true;
    },
    onClose: async (popup) => {
        console.log('Popup closed');
    }
};

const popup = new Popup('Content', POPUP_TYPE.TEXT, null, popupOptions);
const result = await popup.show();
```

#### Helper Methods

```javascript
// Input popup
const name = await Popup.show.input('Title', 'Enter name:', 'default');

// Confirmation popup
const confirmed = await Popup.show.confirm('Title', 'Are you sure?');

// Text popup
await Popup.show.text('Title', 'Information message');

// Display popup
await Popup.show.display('Title', 'Display content');

// Image crop popup
const croppedImage = await Popup.show.crop('Select Area', imageUrl, {
    cropAspect: 1.0 // Square crop
});
```

## Utility Functions

### Core Utilities

```javascript
import {
    deepMerge,
    debounce,
    throttle,
    uuidv4,
    removeFromArray,
    runAfterAnimation,
    delay,
    waitUntilCondition,
    createThumbnail,
    getFileText,
    saveAsFile
} from './scripts/utils.js';

// Deep merge objects
const merged = deepMerge(obj1, obj2);

// Debounce function calls
const debouncedFn = debounce(() => {
    console.log('Debounced call');
}, 300);

// Generate UUID
const id = uuidv4();

// Remove item from array
removeFromArray(array, item);

// Wait for animation to complete
await runAfterAnimation(element, () => {
    // Animation completed
});

// Delay execution
await delay(1000); // Wait 1 second

// Wait for condition
await waitUntilCondition(() => someCondition, 5000);

// Create thumbnail from image
const thumbnail = await createThumbnail(imageFile, 150, 150);

// Read file as text
const text = await getFileText(file);

// Save content as file
saveAsFile('filename.txt', 'file content');
```

### String Utilities

```javascript
import {
    escapeHtml,
    unescapeHtml,
    extractTextFromHTML,
    stringFormat,
    fuzzySearchCharacters,
    highlightRegex,
    collapseNewlines
} from './scripts/utils.js';

// HTML escape/unescape
const escaped = escapeHtml('<script>alert("xss")</script>');
const unescaped = unescapeHtml('&lt;div&gt;');

// Extract text from HTML
const plainText = extractTextFromHTML('<p>Hello <b>world</b></p>');

// String formatting
const formatted = stringFormat('Hello {0}, you have {1} messages', 'John', 5);

// Fuzzy search
const results = fuzzySearchCharacters(characters, 'alice');

// Highlight regex matches
const highlighted = highlightRegex('Hello world', /world/g, 'highlight');

// Collapse multiple newlines
const collapsed = collapseNewlines('Line 1\n\n\n\nLine 2');
```

### DOM Utilities

```javascript
import {
    getElementByIdWithFallback,
    findElementWithAttribute,
    isElementVisible,
    scrollToElement,
    animateElement,
    measureElement
} from './scripts/utils.js';

// Get element with fallback
const element = getElementByIdWithFallback('myId', 'fallbackId');

// Find element by attribute
const found = findElementWithAttribute('data-character', 'Alice');

// Check if element is visible
if (isElementVisible(element)) {
    // Element is visible
}

// Scroll to element
scrollToElement(element, { behavior: 'smooth' });

// Animate element
await animateElement(element, { opacity: 0 }, 300);

// Measure element dimensions
const { width, height } = measureElement(element);
```

### File and Image Utilities

```javascript
import {
    loadFileToDataURL,
    dataUrlToBlob,
    compressImage,
    resizeImage,
    cropImage,
    getImageDimensions
} from './scripts/utils.js';

// Load file to data URL
const dataUrl = await loadFileToDataURL(file);

// Convert data URL to blob
const blob = dataUrlToBlob(dataUrl);

// Compress image
const compressed = await compressImage(imageFile, 0.8, 1920, 1080);

// Resize image
const resized = await resizeImage(imageFile, 300, 300);

// Crop image
const cropped = await cropImage(imageFile, 100, 100, 200, 200);

// Get image dimensions
const { width, height } = await getImageDimensions(imageUrl);
```

## Chat Management

### Chat API

```javascript
import {
    loadChat,
    saveChat,
    deleteChat,
    sendMessage,
    regenerateMessage,
    editMessage,
    deleteMessage,
    exportChat
} from './scripts/chats.js';

// Load chat for character
const messages = await loadChat('Alice', 'alice_20240101.jsonl');

// Save chat
await saveChat('Alice', 'alice_20240101.jsonl', messages);

// Send new message
const response = await sendMessage('Hello Alice!', {
    character: 'Alice',
    regenerate: false
});

// Regenerate last message
await regenerateMessage();

// Edit message
await editMessage(messageIndex, 'Updated message text');

// Delete message
await deleteMessage(messageIndex);

// Export chat
const exportData = await exportChat('Alice', 'alice_20240101.jsonl', 'json');
```

### Message Object Structure

```javascript
const message = {
    name: 'Alice',                    // Sender name
    mes: 'Hello there!',              // Message content
    send_date: '2024-01-01T10:00:00.000Z', // Timestamp
    is_user: false,                   // Is user message
    is_system: false,                 // Is system message
    extra: {                          // Extra metadata
        api: 'openai',
        model: 'gpt-3.5-turbo',
        gen_started: Date.now(),
        gen_finished: Date.now(),
        token_count: 15
    },
    swipes: ['Alternative response'],  // Alternative responses
    swipe_id: 0,                      // Current swipe index
    mes_id: 'msg_12345'               // Unique message ID
};
```

### Chat Events

```javascript
// Listen for chat events
document.addEventListener('chatLoaded', (event) => {
    const { character, chatFile } = event.detail;
    console.log(`Chat loaded for ${character}`);
});

document.addEventListener('messageSent', (event) => {
    const { message, character } = event.detail;
    console.log('Message sent:', message);
});

document.addEventListener('messageReceived', (event) => {
    const { message, character } = event.detail;
    console.log('Message received:', message);
});
```

## Character Management

### Character API

```javascript
import {
    loadCharacters,
    getCharacter,
    createCharacter,
    editCharacter,
    deleteCharacter,
    importCharacter,
    exportCharacter,
    duplicateCharacter
} from './scripts/personas.js';

// Load all characters
const characters = await loadCharacters();

// Get specific character
const alice = await getCharacter('Alice');

// Create new character
const newCharacter = await createCharacter({
    name: 'Bob',
    description: 'A helpful assistant',
    personality: 'Friendly and knowledgeable',
    first_mes: 'Hello! How can I help you?',
    scenario: 'A conversation with an AI assistant',
    mes_example: '<START>\n{{user}}: Hi\n{{char}}: Hello!',
    avatar: avatarDataUrl,
    tags: ['assistant', 'helpful']
});

// Edit character
await editCharacter('Alice', {
    description: 'Updated description',
    personality: 'Updated personality'
});

// Delete character
await deleteCharacter('Bob');

// Import character from file
const imported = await importCharacter(characterFile);

// Export character
const exportData = await exportCharacter('Alice', 'png');

// Duplicate character
const duplicate = await duplicateCharacter('Alice', 'Alice Copy');
```

### Character Object Structure

```javascript
const character = {
    name: 'Alice',                    // Character name
    description: 'A friendly AI',     // Character description
    personality: 'Helpful, curious',  // Personality traits
    first_mes: 'Hello!',              // First message
    scenario: 'Conversation',         // Scenario description
    mes_example: '<START>...',        // Example messages
    create_date: '2024-01-01T00:00:00.000Z', // Creation date
    avatar: 'alice.png',              // Avatar filename
    chat: 'alice_20240101',           // Default chat
    tags: ['assistant', 'ai'],        // Character tags
    fav: false,                       // Is favorite
    talkativeness: 0.5,               // Talkativeness (0-1)
    extensions: {                     // Extension data
        expressions: {},
        depth_prompt: {}
    },
    data: {                          // Additional metadata
        creator: 'User',
        character_version: '1.0.0'
    }
};
```

### Character Events

```javascript
// Character events
document.addEventListener('characterLoaded', (event) => {
    const { character } = event.detail;
    console.log('Character loaded:', character.name);
});

document.addEventListener('characterCreated', (event) => {
    const { character } = event.detail;
    console.log('Character created:', character.name);
});

document.addEventListener('characterUpdated', (event) => {
    const { character, changes } = event.detail;
    console.log('Character updated:', character.name, changes);
});
```

## Settings and Configuration

### Settings API

```javascript
import {
    loadSettings,
    saveSettings,
    resetSettings,
    getSettingValue,
    setSettingValue,
    exportSettings,
    importSettings
} from './scripts/power-user.js';

// Load user settings
const settings = await loadSettings();

// Save settings
await saveSettings(updatedSettings);

// Reset to defaults
await resetSettings();

// Get specific setting
const theme = getSettingValue('theme', 'dark');

// Set specific setting
setSettingValue('language', 'en');

// Export settings
const exportData = await exportSettings();

// Import settings
await importSettings(settingsData);
```

### Settings Object Structure

```javascript
const settings = {
    // UI Settings
    theme: 'dark',
    language: 'en',
    ui_mode: 'normal',
    
    // Chat Settings
    max_context_length: 4096,
    streaming: true,
    auto_save_msg: true,
    
    // AI Settings
    temperature: 0.7,
    top_p: 1.0,
    repetition_penalty: 1.1,
    
    // Advanced Settings
    tokenizer: 'openai',
    token_padding: 64,
    pygmalion_formatting: 0,
    
    // Extension Settings
    extensions: {
        enabled: true,
        autoUpdate: true
    }
};
```

### Configuration Events

```javascript
// Settings events
document.addEventListener('settingsLoaded', (event) => {
    const { settings } = event.detail;
    console.log('Settings loaded');
});

document.addEventListener('settingChanged', (event) => {
    const { key, value, oldValue } = event.detail;
    console.log(`Setting ${key} changed from ${oldValue} to ${value}`);
});
```

## UI Components

### Toast Notifications

```javascript
import { toastr } from './scripts/RossAscends-mods.js';

// Show success toast
toastr.success('Operation completed successfully!');

// Show error toast
toastr.error('An error occurred');

// Show warning toast
toastr.warning('This is a warning');

// Show info toast
toastr.info('Information message');

// Custom toast options
toastr.success('Message', 'Title', {
    timeOut: 5000,
    positionClass: 'toast-top-right',
    closeButton: true,
    progressBar: true
});
```

### Progress Indicators

```javascript
import { showLoader, hideLoader, updateProgress } from './scripts/loader.js';

// Show loading spinner
showLoader('Processing...');

// Update progress
updateProgress(50, 'Processing files...');

// Hide loader
hideLoader();

// Progress bar
const progressBar = document.createElement('div');
progressBar.className = 'progress-bar';
progressBar.style.width = '75%';
```

### Modal Dialogs

```javascript
// Show modal dialog
function showModal(title, content, options = {}) {
    const modal = document.createElement('div');
    modal.className = 'modal fade';
    modal.innerHTML = `
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h4 class="modal-title">${title}</h4>
                    <button type="button" class="close" data-dismiss="modal">×</button>
                </div>
                <div class="modal-body">${content}</div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    `;
    
    document.body.appendChild(modal);
    $(modal).modal('show');
    
    return modal;
}
```

### Form Components

```javascript
// Create form elements
function createFormField(type, name, label, options = {}) {
    const field = document.createElement('div');
    field.className = 'form-group';
    
    const labelEl = document.createElement('label');
    labelEl.textContent = label;
    labelEl.setAttribute('for', name);
    
    const input = document.createElement(type === 'textarea' ? 'textarea' : 'input');
    input.type = type;
    input.name = name;
    input.id = name;
    input.className = 'form-control';
    
    if (options.placeholder) input.placeholder = options.placeholder;
    if (options.value) input.value = options.value;
    if (options.required) input.required = true;
    
    field.appendChild(labelEl);
    field.appendChild(input);
    
    return field;
}

// Form validation
function validateForm(form) {
    const errors = [];
    const inputs = form.querySelectorAll('input, textarea, select');
    
    inputs.forEach(input => {
        if (input.required && !input.value.trim()) {
            errors.push(`${input.name} is required`);
        }
    });
    
    return errors;
}
```

## Event System

### Custom Events

```javascript
// Dispatch custom event
function dispatchEvent(eventName, detail = {}) {
    const event = new CustomEvent(eventName, { detail });
    document.dispatchEvent(event);
}

// Listen for custom events
document.addEventListener('customEvent', (event) => {
    console.log('Custom event:', event.detail);
});

// Common SillyTavern events
const EVENTS = {
    CHARACTER_LOADED: 'characterLoaded',
    CHAT_LOADED: 'chatLoaded',
    MESSAGE_SENT: 'messageSent',
    MESSAGE_RECEIVED: 'messageReceived',
    SETTINGS_CHANGED: 'settingsChanged',
    EXTENSION_LOADED: 'extensionLoaded'
};
```

### Event Emitter

```javascript
class EventEmitter {
    constructor() {
        this.events = {};
    }
    
    on(event, callback) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(callback);
    }
    
    off(event, callback) {
        if (this.events[event]) {
            this.events[event] = this.events[event].filter(cb => cb !== callback);
        }
    }
    
    emit(event, ...args) {
        if (this.events[event]) {
            this.events[event].forEach(callback => callback(...args));
        }
    }
    
    once(event, callback) {
        const onceCallback = (...args) => {
            callback(...args);
            this.off(event, onceCallback);
        };
        this.on(event, onceCallback);
    }
}

// Usage
const emitter = new EventEmitter();
emitter.on('dataChanged', (data) => console.log('Data:', data));
emitter.emit('dataChanged', { key: 'value' });
```

## Extensions Framework

### Extension Structure

```javascript
// Extension manifest
const extension = {
    name: 'My Extension',
    version: '1.0.0',
    description: 'A sample extension',
    author: 'Developer',
    
    // Extension initialization
    init: function() {
        console.log('Extension initialized');
        this.setupUI();
        this.registerEvents();
    },
    
    // Setup UI elements
    setupUI: function() {
        const button = document.createElement('button');
        button.textContent = 'Extension Button';
        button.onclick = () => this.handleClick();
        document.getElementById('extensions_settings').appendChild(button);
    },
    
    // Register event handlers
    registerEvents: function() {
        document.addEventListener('characterLoaded', (event) => {
            this.onCharacterLoaded(event.detail.character);
        });
    },
    
    // Handle button click
    handleClick: function() {
        console.log('Extension button clicked');
    },
    
    // Handle character loaded
    onCharacterLoaded: function(character) {
        console.log('Character loaded in extension:', character.name);
    },
    
    // Extension cleanup
    destroy: function() {
        console.log('Extension destroyed');
    }
};

// Register extension
if (typeof registerExtension === 'function') {
    registerExtension(extension);
}
```

### Extension API

```javascript
import { getContext } from './scripts/extensions.js';

// Get extension context
const context = getContext();

// Access SillyTavern data
const characters = context.characters;
const currentCharacter = context.characterId;
const settings = context.settings;

// Extension storage
context.saveExtensionSettings('myExtension', { key: 'value' });
const settings = context.loadExtensionSettings('myExtension');

// Register slash commands
context.registerSlashCommand('mycommand', (args) => {
    console.log('Custom command executed:', args);
}, ['Custom command help text']);

// Add extension UI
context.addExtensionUI(`
    <div id="my-extension-panel">
        <h3>My Extension</h3>
        <button id="my-extension-btn">Click Me</button>
    </div>
`);
```

## Tag System

### Tag Management

```javascript
import {
    getTagsList,
    createTag,
    deleteTag,
    addTagToCharacter,
    removeTagFromCharacter,
    getCharacterTags,
    filterCharactersByTags
} from './scripts/tags.js';

// Get all tags
const tags = getTagsList();

// Create new tag
const newTag = createTag('new-tag', 'New Tag', '#ff6b6b');

// Delete tag
deleteTag('old-tag');

// Add tag to character
addTagToCharacter('Alice', 'assistant');

// Remove tag from character
removeTagFromCharacter('Alice', 'old-tag');

// Get character tags
const characterTags = getCharacterTags('Alice');

// Filter characters by tags
const filtered = filterCharactersByTags(['assistant', 'helpful']);
```

### Tag Object Structure

```javascript
const tag = {
    id: 'assistant',              // Tag ID
    name: 'Assistant',            // Display name
    color: '#4a90e2',            // Tag color
    color2: '#ffffff',           // Secondary color
    create_date: Date.now(),     // Creation timestamp
    exclusion: [],               // Excluded tags
    sort_order: 0                // Sort order
};
```

## Search and Filtering

### Search API

```javascript
import {
    searchCharacters,
    searchChats,
    fuzzySearch,
    filterByTags,
    sortResults
} from './scripts/search.js';

// Search characters
const characterResults = await searchCharacters({
    query: 'friendly assistant',
    tags: ['helper'],
    sort: 'name',
    limit: 10
});

// Search chat messages
const chatResults = await searchChats({
    query: 'hello world',
    character: 'Alice',
    dateFrom: '2024-01-01',
    dateTo: '2024-01-31'
});

// Fuzzy search
const fuzzyResults = fuzzySearch(items, 'search term', {
    keys: ['name', 'description'],
    threshold: 0.6
});

// Filter by tags
const tagFiltered = filterByTags(characters, ['assistant', 'ai']);

// Sort results
const sorted = sortResults(results, 'name', 'asc');
```

### Search Options

```javascript
const searchOptions = {
    query: 'search term',           // Search query
    tags: ['tag1', 'tag2'],        // Filter by tags
    sort: 'name',                  // Sort field
    order: 'asc',                  // Sort order
    limit: 20,                     // Result limit
    offset: 0,                     // Result offset
    fuzzy: true,                   // Enable fuzzy search
    threshold: 0.6,                // Fuzzy search threshold
    caseSensitive: false,          // Case sensitive search
    wholeWord: false               // Whole word search
};
```

## Internationalization

### i18n API

```javascript
import { t, getCurrentLocale, setLocale, loadTranslations } from './scripts/i18n.js';

// Translate text
const translated = t('Hello World');
const withParams = t('Hello {0}', 'Alice');

// Get current locale
const locale = getCurrentLocale();

// Set locale
await setLocale('es');

// Load custom translations
await loadTranslations('custom', {
    'Hello': 'Hola',
    'Goodbye': 'Adiós'
});

// Template literal translation
const message = t`Welcome to ${appName}`;

// Pluralization
const count = 5;
const plural = t('You have {0} message{{s}}', count);
```

### Translation Files

```javascript
// en.json
{
    "Hello World": "Hello World",
    "Character Name": "Character Name",
    "Delete Character": "Delete Character",
    "Are you sure?": "Are you sure you want to delete this character?",
    "Save": "Save",
    "Cancel": "Cancel"
}

// es.json
{
    "Hello World": "Hola Mundo",
    "Character Name": "Nombre del Personaje",
    "Delete Character": "Eliminar Personaje",
    "Are you sure?": "¿Estás seguro de que quieres eliminar este personaje?",
    "Save": "Guardar",
    "Cancel": "Cancelar"
}
```

### Dynamic Translation

```javascript
// Register translation observer
function observeTranslations() {
    const observer = new MutationObserver((mutations) => {
        mutations.forEach((mutation) => {
            mutation.addedNodes.forEach((node) => {
                if (node.nodeType === Node.ELEMENT_NODE) {
                    translateElement(node);
                }
            });
        });
    });
    
    observer.observe(document.body, {
        childList: true,
        subtree: true
    });
}

// Translate element
function translateElement(element) {
    // Translate text content
    const textNodes = getTextNodes(element);
    textNodes.forEach(node => {
        const translated = t(node.textContent.trim());
        if (translated !== node.textContent.trim()) {
            node.textContent = translated;
        }
    });
    
    // Translate attributes
    const translatableAttrs = ['title', 'placeholder', 'alt'];
    translatableAttrs.forEach(attr => {
        const value = element.getAttribute(attr);
        if (value) {
            element.setAttribute(attr, t(value));
        }
    });
}
```

## Error Handling

### Global Error Handler

```javascript
// Global error handler
window.addEventListener('error', (event) => {
    console.error('Global error:', event.error);
    
    // Show user-friendly error message
    toastr.error('An unexpected error occurred. Please refresh the page.');
    
    // Report error to logging service
    reportError(event.error);
});

// Promise rejection handler
window.addEventListener('unhandledrejection', (event) => {
    console.error('Unhandled promise rejection:', event.reason);
    
    // Prevent default browser behavior
    event.preventDefault();
    
    // Show error message
    toastr.error('An error occurred while processing your request.');
});
```

### Error Utilities

```javascript
// Safe async function wrapper
function safeAsync(fn) {
    return async (...args) => {
        try {
            return await fn(...args);
        } catch (error) {
            console.error('Async error:', error);
            toastr.error('Operation failed: ' + error.message);
            throw error;
        }
    };
}

// Error boundary for UI components
class ErrorBoundary {
    constructor(element, fallback) {
        this.element = element;
        this.fallback = fallback;
        this.setupErrorHandling();
    }
    
    setupErrorHandling() {
        this.element.addEventListener('error', (event) => {
            this.handleError(event.error);
        });
    }
    
    handleError(error) {
        console.error('Component error:', error);
        this.element.innerHTML = this.fallback;
    }
}

// Usage
const errorBoundary = new ErrorBoundary(
    document.getElementById('main-content'),
    '<div class="error">Something went wrong. Please refresh the page.</div>'
);
```

This documentation provides comprehensive coverage of SillyTavern's client-side components and their public APIs. The components are designed to be modular, extensible, and easy to use for building rich interactive web applications.