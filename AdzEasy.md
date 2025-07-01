# AdzEasy - AI-Powered Ad Creative Generator
## Complete Technical Documentation

## 📋 1. Project Overview
AdzEasy is a browser-based, locally hosted tool that leverages AI to generate professional product advertisement creatives. It combines user-uploaded product images with AI-generated backgrounds, custom taglines, and logos to create compelling marketing visuals.

### Key Characteristics:
- Fully Client-Side: Runs entirely in the browser with no backend infrastructure
- AI-Powered: Utilizes DeepSeek's Janus Pro Vision API for image generation and Chat API for text generation
- Privacy-Focused: All processing happens locally except for API calls
- Zero Dependencies: No databases, authentication, or cloud storage required
- Instant Export: Direct download of generated creatives

### Target Users:
- Small business owners
- Digital marketers
- E-commerce sellers
- Content creators
- Anyone needing quick, professional ad creatives

## 🏗️ 2. System Architecture
<pre>┌─────────────────────────────────────────────────────────────┐
│                        USER BROWSER                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────┐     ┌──────────────────┐             │
│  │   HTML Interface │     │  JavaScript Core  │             │
│  │  - Upload Forms  │────▶│  - Event Handlers │             │
│  │  - Canvas View   │     │  - API Manager    │             │
│  │  - Controls      │◀────│  - Canvas Engine  │             │
│  └─────────────────┘     └──────────────────┘             │
│                                  │                          │
│                                  ▼                          │
│                    ┌─────────────────────────┐             │
│                    │    Browser APIs         │             │
│                    │  - FileReader API       │             │
│                    │  - Canvas API           │             │
│                    │  - Blob/Download API    │             │
│                    └─────────────────────────┘             │
│                                                             │
└─────────────────────────────┬───────────────────────────────┘
                              │ HTTPS
                              ▼
              ┌───────────────────────────────┐
              │      External APIs            │
              ├───────────────────────────────┤
              │  • DeepSeek Janus Pro Vision  │
              │  • DeepSeek Chat API          │
              └───────────────────────────────┘</pre>

### Data Flow:
1. User uploads product image → Stored in browser memory
2. User inputs/selects background idea → Sent to DeepSeek Vision API
3. AI generates background with product → Returned as base64 image
4. User requests tagline → DeepSeek Chat API generates text
5. Canvas API composites all elements → Final image rendered
6. User downloads result → Blob URL created and downloaded

## 🧩 3. Modular Architecture
AdzEasy is built with a modular architecture that promotes maintainability, scalability, and easy transition to a SaaS model.

### Core Modules:
<pre>┌─────────────────────────────────────────────────────────┐
│                    Presentation Layer                    │
│               (index.html + styles.css)                  │
├─────────────────────────────────────────────────────────┤
│                    Application Core                      │
│                      (app.js)                           │
├─────────┬──────────┬──────────┬────────────────────────┤
│ API     │ Canvas   │ Image    │ Utilities              │
│ Module  │ Engine   │ Processor│ Module                 │
│(api.js) │(canvas.js)│(imageProcessor.js)│(utils.js)   │
└─────────┴──────────┴──────────┴────────────────────────┘</pre>

### Module Responsibilities:
#### 1. API Module (api.js)
- Isolated API communication logic
- Request/response handling
- Error management
- Easy to swap for backend proxy

#### 2. Canvas Engine (canvas.js)
- Self-contained rendering logic
- Layer management
- Export functionality
- Platform-agnostic design

#### 3. Image Processor (imageProcessor.js)
- Upload handling
- Validation
- Preprocessing
- Format conversions

### SaaS Transition Benefits:
```javascript
// Current: Direct API calls
const response = await callDeepSeekAPI(data);

// Future SaaS: Backend proxy
const response = await callBackendAPI('/api/generate', data);
// Backend handles: authentication, rate limiting, billing, caching
```

This modular design allows adding:
- Authentication Layer: User login/signup without touching core logic
- Payment Integration: Subscription handling in separate module
- User Dashboard: Profile management as new module
- Analytics: Usage tracking without modifying existing code

## 📁 4. Folder Structure
<pre>adzeasy/
│
├── index.html              # Main application HTML
├── css/
│   └── styles.css         # Application styling
├── js/
│   ├── app.js            # Main application logic
│   ├── api.js            # API communication module
│   ├── canvas.js         # Canvas rendering engine
│   ├── imageProcessor.js # Image upload/processing
│   └── utils.js          # Utility functions
├── assets/
│   ├── icons/            # UI icons
│   └── placeholders/     # Default images
├── config/
│   └── config.js         # API keys and configuration
└── README.md             # User documentation</pre>

### File Descriptions:
- index.html: Single-page application structure with all UI elements
- app.js: Orchestrates the entire application flow and user interactions
- api.js: Handles all DeepSeek API communications with error handling
- canvas.js: Manages image composition, layering, and rendering
- imageProcessor.js: Handles image uploads, validation, and preprocessing
- utils.js: Common functions for formatting, validation, and helpers

## 💻 5. Tech Stack Justification
### Frontend Framework: Vanilla JavaScript
- Rationale: Zero build process, immediate deployment, maximum compatibility
- Benefits: No transpilation needed, direct browser execution, lightweight

### Styling: CSS (with optional Tailwind)
- Rationale: Simple styling needs, fast loading, easy customization
- Benefits: No CSS-in-JS overhead, clean separation of concerns

### Image Processing: HTML Canvas API
- Rationale: Native browser support, powerful compositing capabilities
- Benefits: Hardware acceleration, no external dependencies, full control

### Storage: Browser Local Storage + Downloads
- Rationale: Privacy-first approach, no server infrastructure needed
- Benefits: Instant access, no data retention concerns, offline capability

### APIs: DeepSeek Suite
- Rationale: Unified AI provider, strong vision-language capabilities
- Benefits: Consistent API design, high-quality outputs, reasonable pricing

## 🚀 6. Detailed Feature Breakdown
### 6.1 Product Image Upload
```javascript
// Feature Implementation
- Accepts: PNG, JPG, JPEG (max 10MB)
- Validation: File type, size, dimensions
- Processing: Resize to optimal dimensions for API
- Storage: Convert to base64 for API transmission
```

### 6.2 Background Generation
```javascript
// Three modes available:
1. Preset Backgrounds: "Office desk", "Nature scene", "Urban setting"
2. Custom Prompt: User-defined scene description
3. Auto-Generate: AI suggests based on product analysis
```

### 6.3 AI Scene Composition
```javascript
// DeepSeek Janus Pro Vision workflow:
- Input: Product image + background description
- Process: Contextual product placement
- Output: Coherent scene with natural lighting/shadows
```

### 6.4 Tagline Generation
```javascript
// Intelligent copywriting:
- Analyzes: Product type, target audience, tone
- Generates: 3-5 word catchy phrases
- Options: Formal, casual, playful, professional
```

### 6.5 Logo Integration
```javascript
// Flexible positioning:
- Positions: Top-left, top-right, bottom-left, bottom-right
- Sizing: Auto-scale to 15% of canvas width
- Opacity: Adjustable for subtle branding
```

### 6.6 Orientation Support
```javascript
// Three canvas presets:
- Square: 1080x1080 (Instagram, Facebook)
- Portrait: 1080x1920 (Stories, TikTok)
- Landscape: 1920x1080 (YouTube, Twitter)
```

### 6.7 Final Composition
```javascript
// Layer order (bottom to top):
1. AI-generated background
2. Product overlay (if separate)
3. Logo watermark
4. Tagline text overlay
5. Optional filters/effects
```

## 🔌 7. API Usage Documentation
### 7.1 DeepSeek Janus Pro Vision API

**Endpoint:** https://api.deepseek.com/v1/images/generate

**Method:** POST

**Headers:**
```javascript
{
  "Authorization": "Bearer YOUR_API_KEY",
  "Content-Type": "application/json"
}
```

**Request Payload:**
```javascript
{
  "model": "janus-pro-vision",
  "prompt": "A professional product photo of [product] placed on a modern office desk with soft lighting",
  "image": "data:image/jpeg;base64,/9j/4AAQSkZJRg...", // Base64 product image
  "num_images": 1,
  "size": "1024x1024",
  "style": "photorealistic",
  "guidance_scale": 7.5
}
```

**Response:**
```javascript
{
  "images": [
    {
      "url": "https://api.deepseek.com/generated/img_abc123.png",
      "base64": "iVBORw0KGgoAAAANSUhEUgAAA..."
    }
  ],
  "usage": {
    "prompt_tokens": 45,
    "completion_tokens": 0
  }
}
```

### 7.2 DeepSeek Chat API (Tagline Generation)

**Endpoint:** https://api.deepseek.com/v1/chat/completions

**Method:** POST

**Request Payload:**
```javascript
{
  "model": "deepseek-chat",
  "messages": [
    {
      "role": "system",
      "content": "You are a creative copywriter. Generate catchy, memorable taglines."
    },
    {
      "role": "user",
      "content": "Create a tagline for a wireless bluetooth headphone. Tone: professional, modern."
    }
  ],
  "max_tokens": 50,
  "temperature": 0.8
}
```

**Response:**
```javascript{
  "choices": [
    {
      "message": {
        "content": "Sound Without Boundaries"
      }
    }
  ]
}
```

### 7.3 Rate Limit Management
DeepSeek APIs enforce rate limits to ensure fair usage. Implement these strategies to handle limits gracefully:

**Rate Limit Headers:**
```javascript
// Monitor response headers
{
  "X-RateLimit-Limit": "100",      // Requests per minute
  "X-RateLimit-Remaining": "95",   // Remaining requests
  "X-RateLimit-Reset": "1640995200" // Reset timestamp
}
```

**Implementation Strategy:**
```javascript
class RateLimitManager {
  constructor() {
    this.queue = [];
    this.processing = false;
    this.retryDelay = 1000; // Start with 1 second
  }

  async addRequest(apiCall) {
    return new Promise((resolve, reject) => {
      this.queue.push({ apiCall, resolve, reject });
      this.processQueue();
    });
  }

  async processQueue() {
    if (this.processing || this.queue.length === 0) return;
    
    this.processing = true;
    const { apiCall, resolve, reject } = this.queue.shift();
    
    try {
      const response = await apiCall();
      
      // Check rate limit headers
      const remaining = response.headers.get('X-RateLimit-Remaining');
      if (remaining && parseInt(remaining) < 10) {
        console.warn('Approaching rate limit, slowing down requests');
        await this.delay(2000); // Add delay when near limit
      }
      
      this.retryDelay = 1000; // Reset delay on success
      resolve(response);
    } catch (error) {
      if (error.status === 429) { // Rate limit exceeded
        console.log(`Rate limited. Retrying in ${this.retryDelay}ms`);
        this.queue.unshift({ apiCall, resolve, reject }); // Re-queue
        await this.delay(this.retryDelay);
        this.retryDelay = Math.min(this.retryDelay * 2, 60000); // Exponential backoff
      } else {
        reject(error);
      }
    }
    
    this.processing = false;
    this.processQueue(); // Process next request
  }

  delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}

// Usage
const rateLimiter = new RateLimitManager();
const response = await rateLimiter.addRequest(() => callDeepSeekAPI(data));
```

**User-Friendly Error Messages:**
```javascript
function handleRateLimitError() {
  showNotification({
    type: 'warning',
    title: 'Slow Down!',
    message: 'You\'ve made too many requests. Please wait a moment before trying again.',
    duration: 5000
  });
}
```

## 🎨 8. Frontend Workflow
### Step-by-Step User Journey:
```javascript
// 1. Initialize Application
window.onload = () => {
  initializeCanvas();
  setupEventListeners();
  loadConfiguration();
};

// 2. Product Upload Flow
function handleProductUpload(file) {
  validateFile(file);
  const reader = new FileReader();
  reader.onload = (e) => {
    productImage = e.target.result;
    displayPreview(productImage);
    enableBackgroundSelection();
  };
  reader.readAsDataURL(file);
}

// 3. Background Generation
async function generateBackground() {
  showLoader();
  const prompt = buildPrompt(selectedBackground, productDescription);
  const response = await callDeepSeekVision(productImage, prompt);
  backgroundImage = response.base64;
  renderToCanvas();
  hideLoader();
}

// 4. Tagline Creation
async function generateTagline() {
  const context = analyzeProduct(productImage);
  const tagline = await callDeepSeekChat(context);
  displayTaglineOptions(tagline);
}

// 5. Final Composition
function composeAdCreative() {
  const canvas = document.getElementById('adCanvas');
  const ctx = canvas.getContext('2d');
  
  // Set canvas dimensions based on orientation
  setCanvasSize(selectedOrientation);
  
  // Draw layers
  drawBackground(ctx, backgroundImage);
  drawLogo(ctx, logoImage, logoPosition);
  drawTagline(ctx, taglineText, taglineStyle);
  
  enableDownload();
}
```

## 🖼️ 9. Canvas Rendering Logic
### Core Rendering Engine:
```javascript
class CanvasRenderer {
  constructor(canvasId) {
    this.canvas = document.getElementById(canvasId);
    this.ctx = this.canvas.getContext('2d');
    this.layers = [];
  }

  setDimensions(orientation) {
    const dimensions = {
      square: { width: 1080, height: 1080 },
      portrait: { width: 1080, height: 1920 },
      landscape: { width: 1920, height: 1080 }
    };
    
    const { width, height } = dimensions[orientation];
    this.canvas.width = width;
    this.canvas.height = height;
  }

  addLayer(type, content, options = {}) {
    this.layers.push({ type, content, options });
  }

  render() {
    // Clear canvas
    this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
    
    // Render each layer
    this.layers.forEach(layer => {
      switch(layer.type) {
        case 'image':
          this.drawImage(layer.content, layer.options);
          break;
        case 'text':
          this.drawText(layer.content, layer.options);
          break;
        case 'logo':
          this.drawLogo(layer.content, layer.options);
          break;
      }
    });
  }

  drawImage(imageSrc, { x = 0, y = 0, width, height, opacity = 1 }) {
    const img = new Image();
    img.onload = () => {
      this.ctx.globalAlpha = opacity;
      this.ctx.drawImage(img, x, y, width || img.width, height || img.height);
      this.ctx.globalAlpha = 1;
    };
    img.src = imageSrc;
  }

  drawText(text, { x, y, font, color, align, shadow }) {
    this.ctx.font = font || '48px Arial';
    this.ctx.fillStyle = color || '#FFFFFF';
    this.ctx.textAlign = align || 'center';
    
    if (shadow) {
      this.ctx.shadowColor = 'rgba(0, 0, 0, 0.5)';
      this.ctx.shadowBlur = 10;
      this.ctx.shadowOffsetX = 2;
      this.ctx.shadowOffsetY = 2;
    }
    
    this.ctx.fillText(text, x, y);
    
    // Reset shadow
    this.ctx.shadowColor = 'transparent';
  }

  drawLogo(logoSrc, { position, size = 0.15 }) {
    const img = new Image();
    img.onload = () => {
      const logoWidth = this.canvas.width * size;
      const logoHeight = (img.height / img.width) * logoWidth;
      
      const positions = {
        'top-left': { x: 20, y: 20 },
        'top-right': { x: this.canvas.width - logoWidth - 20, y: 20 },
        'bottom-left': { x: 20, y: this.canvas.height - logoHeight - 20 },
        'bottom-right': { 
          x: this.canvas.width - logoWidth - 20, 
          y: this.canvas.height - logoHeight - 20 
        }
      };
      
      const { x, y } = positions[position];
      this.ctx.drawImage(img, x, y, logoWidth, logoHeight);
    };
    img.src = logoSrc;
  }
}
```

## 💾 10. Export/Download Mechanism
### Download Implementation:
```javascript
class ExportManager {
  constructor(canvas) {
    this.canvas = canvas;
  }

  async exportAsImage(format = 'png', quality = 0.9) {
    return new Promise((resolve) => {
      this.canvas.toBlob((blob) => {
        resolve(blob);
      }, `image/${format}`, quality);
    });
  }

  async downloadImage(filename = 'adzeasy-creative') {
    const format = document.getElementById('exportFormat').value;
    const blob = await this.exportAsImage(format);
    
    // Create download link
    const url = URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.href = url;
    link.download = `${filename}.${format}`;
    
    // Trigger download
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    
    // Clean up
    setTimeout(() => URL.revokeObjectURL(url), 100);
  }

  getImageDataURL(format = 'png', quality = 0.9) {
    return this.canvas.toDataURL(`image/${format}`, quality);
  }

  // Optional: Copy to clipboard
  async copyToClipboard() {
    const blob = await this.exportAsImage('png');
    const item = new ClipboardItem({ 'image/png': blob });
    await navigator.clipboard.write([item]);
  }
}
```

## 📈 11. Scalability Suggestions
### Current Limitations & Solutions:
#### 1. API Rate Limits
- Implement request queuing
- Add retry logic with exponential backoff
- Cache generated content locally

#### 2. Large Image Processing
- Implement client-side image compression
- Use Web Workers for heavy processing
- Progressive loading for better UX

#### 3. Feature Expansion Ready
```javascript
// Modular architecture supports:
- Multiple AI providers (OpenAI, Stability AI)
- Batch processing capabilities
- Template system for recurring designs
- Plugin architecture for custom filters
```

#### 4. Performance Optimizations
```javascript
// Implement lazy loading
const modules = {
  heavyFeature: () => import('./heavyFeature.js'),
  advancedFilters: () => import('./filters.js')
};
```

## 🚀 12. Deployment Instructions
### Local Setup:
#### 1. Clone or Download Project
```bash
git clone https://github.com/yourusername/adzeasy.git
cd adzeasy
```

#### 2. Configure API Keys
```javascript
// config/config.js
const CONFIG = {
  DEEPSEEK_API_KEY: 'your-api-key-here',
  API_ENDPOINTS: {
    VISION: 'https://api.deepseek.com/v1/images/generate',
    CHAT: 'https://api.deepseek.com/v1/chat/completions'
  }
};
```

#### 3. Serve Locally
```bash
# Using Python
python -m http.server 8000

# Using Node.js
npx http-server -p 8000

# Using PHP
php -S localhost:8000
```

#### 4. Access Application
- Open browser to http://localhost:8000
- No build process required!

### API Key Management:
#### Personal Use Configuration:
```javascript
// config/config.js - Simple approach for personal use
const CONFIG = {
  DEEPSEEK_API_KEY: 'sk-...' // Your personal key
};
```

#### Demo/Public Use Configuration:
```javascript
// Implement dynamic API key input
class APIKeyManager {
  constructor() {
    this.apiKey = null;
  }

  // Check for stored key (session only)
  getStoredKey() {
    return sessionStorage.getItem('deepseek_api_key');
  }

  // Prompt user for API key
  async promptForKey() {
    const existingKey = this.getStoredKey();
    if (existingKey) {
      this.apiKey = existingKey;
      return existingKey;
    }

    // Show API key input modal
    const modal = document.getElementById('apiKeyModal');
    modal.style.display = 'block';
    
    return new Promise((resolve) => {
      document.getElementById('saveApiKey').onclick = () => {
        const key = document.getElementById('apiKeyInput').value;
        if (key) {
          this.apiKey = key;
          sessionStorage.setItem('deepseek_api_key', key);
          modal.style.display = 'none';
          resolve(key);
        }
      };
    });
  }

  // Validate API key
  async validateKey(key) {
    try {
      const response = await fetch('https://api.deepseek.com/v1/models', {
        headers: { 'Authorization': `Bearer ${key}` }
      });
      return response.ok;
    } catch {
      return false;
    }
  }

  // Handle expired/invalid keys
  handleInvalidKey() {
    sessionStorage.removeItem('deepseek_api_key');
    this.apiKey = null;
    showNotification({
      type: 'error',
      message: 'Invalid API key. Please enter a valid key.',
      action: () => this.promptForKey()
    });
  }
}
```

#### Future SaaS Configuration:
```javascript
// Backend proxy approach (future implementation)
const CONFIG = {
  API_ENDPOINT: 'https://your-backend.com/api',
  // API key stored securely on backend
};

// All API calls go through backend
async function callAPI(endpoint, data) {
  const response = await fetch(`${CONFIG.API_ENDPOINT}${endpoint}`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${userToken}`, // User auth token
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  });
  return response.json();
}
```

### Production Considerations:
#### 1. Secure API Keys
```javascript
// Never expose keys in frontend
// Consider proxy server for production
const getApiKey = async () => {
  // Prompt user to enter their own key
  return localStorage.getItem('userApiKey');
};
```

#### 2. HTTPS Requirement
- Modern browser APIs require HTTPS
- Use GitHub Pages or Netlify for free HTTPS hosting

## 🏆 13. Best Practices & Recommendations
### Code Quality:
#### 1. Error Handling
```javascript
async function safeApiCall(fn) {
  try {
    return await fn();
  } catch (error) {
    console.error('API Error:', error);
    showUserNotification('Something went wrong. Please try again.');
    return null;
  }
}
```

#### 2. User Feedback
```javascript
// Always indicate loading states
function showLoader() {
  document.getElementById('loader').style.display = 'block';
}

// Provide progress updates
function updateProgress(step, total) {
  const percent = (step / total) * 100;
  document.getElementById('progress').style.width = `${percent}%`;
}
```

#### 3. Memory Management
```javascript
// Clean up large objects
function cleanup() {
  URL.revokeObjectURL(currentImageUrl);
  canvas.getContext('2d').clearRect(0, 0, canvas.width, canvas.height);
  layers = [];
}
```

### Security Considerations:
#### 1. Input Validation
   - Sanitize all user inputs
   - Validate file types and sizes
   - Prevent XSS in text inputs

#### 2. API Key Security
   - Store keys in session storage, not local storage
   - Clear keys on window close
   - Implement key rotation reminders
   - Add usage monitoring

### Performance Tips:
#### 1. Optimize Images
```javascript
function resizeImage(img, maxWidth, maxHeight) {
  const canvas = document.createElement('canvas');
  let width = img.width;
  let height = img.height;

  if (width > height) {
    if (width > maxWidth) {
      height *= maxWidth / width;
      width = maxWidth;
    }
  } else {
    if (height > maxHeight) {
      width *= maxHeight / height;
      height = maxHeight;
    }
  }

  canvas.width = width;
  canvas.height = height;
  canvas.getContext('2d').drawImage(img, 0, 0, width, height);
  return canvas.toDataURL('image/jpeg', 0.8);
}
```

#### 2. Implement Caching
```javascript
class CacheManager {
  constructor() {
    this.cache = new Map();
    this.maxSize = 50; // Maximum cached items
    this.maxAge = 3600000; // 1 hour in milliseconds
  }

  generateKey(type, params) {
    return `${type}_${JSON.stringify(params)}`;
  }

  set(type, params, data) {
    const key = this.generateKey(type, params);
    
    // Implement LRU cache
    if (this.cache.size >= this.maxSize) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }

    this.cache.set(key, {
      data: data,
      timestamp: Date.now()
    });

    // Optional: Persist to IndexedDB for session persistence
    this.persistToIndexedDB(key, data);
  }

  get(type, params) {
    const key = this.generateKey(type, params);
    const cached = this.cache.get(key);

    if (!cached) return null;

    // Check if cache is expired
    if (Date.now() - cached.timestamp > this.maxAge) {
      this.cache.delete(key);
      return null;
    }

    return cached.data;
  }

  async persistToIndexedDB(key, data) {
    // Implementation for persistent caching
    const db = await this.openDB();
    const transaction = db.transaction(['cache'], 'readwrite');
    const store = transaction.objectStore('cache');
    store.put({ key, data, timestamp: Date.now() });
  }

  clear() {
    this.cache.clear();
    // Also clear IndexedDB if implemented
  }
}

// Usage in API calls
const cache = new CacheManager();

async function generateBackgroundWithCache(prompt, image) {
  // Check cache first
  const cached = cache.get('background', { prompt, imageHash: hashImage(image) });
  if (cached) {
    console.log('Using cached background');
    return cached;
  }

  // Make API call
  const result = await callDeepSeekVision(image, prompt);
  
  // Cache the result
  cache.set('background', { prompt, imageHash: hashImage(image) }, result);
  
  return result;
}
```

#### 3. Debounce User Input
```javascript
const debounce = (func, wait) => {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
};
```

### Future Enhancements:
#### 1. PWA Support
   - Add service worker for offline functionality
   - Implement app manifest for installability

#### 2. Accessibility
   - ARIA labels for all interactive elements
   - Keyboard navigation support
   - Screen reader compatibility

#### 3. Internationalization
   - Implement i18n framework
   - Support RTL languages
   - Localized AI prompts

## 📞 Support & Maintenance
### Debugging Tips:
- Enable browser DevTools for network monitoring
- Check console for API response details
- Use Canvas inspector for rendering issues

### Common Issues:
- CORS Errors: Ensure API supports browser requests
- Memory Leaks: Monitor blob URL creation/cleanup
- Slow Performance: Reduce image dimensions before processing
- Rate Limits: Implement proper queuing and retry logic

### Update Schedule:
- Monitor DeepSeek API changes
- Update browser compatibility quarterly
- Security patches as needed

This documentation represents the complete technical specification for AdzEasy v1.0. The modular architecture ensures easy maintenance and future scalability to a full SaaS platform.
