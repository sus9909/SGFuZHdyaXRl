# Handwriting Generation API

This is a static API that runs the original JavaScript handwriting generation model entirely in the browser.

## Setup

1. **Copy the `d.bin` file** from the original calligrapher-ai project to this `api/` directory
2. **Deploy to GitHub Pages**:
   - Create a new repository or use an existing one
   - Copy the contents of the `api/` folder to your repository
   - Enable GitHub Pages in repository settings
   - Point it to the main branch (or gh-pages branch)

## Files Required

```
api/
├── index.html      (API interface)
├── logic.js        (Original generation logic)
└── d.bin           (Model weights - REQUIRED!)
```

⚠️ **Important**: You need to copy `d.bin` from the original calligrapher-ai project. This file contains the neural network weights.

## Usage

### Method 1: URL Parameters (Testing)

```
https://your-username.github.io/api/?text=Hello%20World&style=5&bias=0.5
```

### Method 2: Python Bot Integration

The Discord bot will make requests to your API endpoint. Update the bot.py with your GitHub Pages URL:

```python
API_URL = "https://your-username.github.io/api/"
```

### Method 3: Direct JavaScript

```javascript
// Load in an iframe
const iframe = document.createElement('iframe');
iframe.src = 'https://your-username.github.io/api/';
iframe.style.display = 'none';
document.body.appendChild(iframe);

// Wait for load
iframe.onload = () => {
    // Send generation request
    iframe.contentWindow.postMessage({
        action: 'generate',
        text: 'Hello World',
        style: '5',
        bias: '0.5',
        speed: '1',
        width: '1'
    }, '*');
};

// Listen for response
window.addEventListener('message', (event) => {
    if (event.data.success) {
        const imageData = event.data.image; // base64 PNG
        // Use the image
        const img = document.createElement('img');
        img.src = 'data:image/png;base64,' + imageData;
        document.body.appendChild(img);
    }
});
```

## Parameters

- **text** (required): Text to convert to handwriting
- **style**: Style variation `-` for random, or `0-9` for specific style (default: `-`)
- **bias**: Bias value `0-1` (default: `0.5`)
- **speed**: Speed value (default: `1`)
- **width**: Stroke width value (default: `1`)

## Response Format

```json
{
  "success": true,
  "image": "base64_encoded_png_data",
  "format": "png"
}
```

## Notes

- Generation takes 2-3 seconds
- The model runs entirely client-side (no server needed)
- All original logic.js functionality is preserved
- CORS is handled automatically by GitHub Pages
- No API key or authentication required

## Deployment Checklist

- [ ] Copy `d.bin` to the api/ directory
- [ ] Push to GitHub repository
- [ ] Enable GitHub Pages
- [ ] Test the URL in browser
- [ ] Update bot.py with your API URL
- [ ] Test bot integration
